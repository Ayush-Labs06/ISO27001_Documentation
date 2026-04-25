# Disaster Recovery Runbook
**ISO 27001:2022 — A.5.29, A.5.30 | Version 1.0**
**Owner:** DevOps Lead / CTO
**Review:** Annual + after any DR test or architecture change

> **Activation criteria:** Invoke this runbook when BCP Coordinator declares a DR event, or when primary region is unavailable >1 hour with no recovery ETA.

---

## 0. Pre-DR Checklist (Read Before Starting)

- [ ] IRP has been invoked if there is a security component
- [ ] BCP Coordinator has declared DR event
- [ ] You have access to AWS console via break-glass account (if normal SSO is unavailable)
- [ ] You have access to production secrets (AWS Secrets Manager or 1Password break-glass)
- [ ] You have current Terraform state or last known-good IaC commit hash
- [ ] Comms lead has posted status page update: "We are aware of a service disruption and are actively working to restore service."

---

## 1. DR Architecture Overview

```
Primary Region (us-east-1)                  DR Region (eu-west-1)
┌─────────────────────────┐                 ┌─────────────────────────┐
│  Route 53 (active)      │                 │  Route 53 (standby)     │
│  ALB → ECS/EKS cluster  │   Replication   │  ALB → ECS/EKS cluster  │
│  RDS (primary)          │ ──────────────► │  RDS (replica)          │
│  S3 (CRR source)        │                 │  S3 (CRR destination)   │
│  Secrets Manager        │   Manual sync   │  Secrets Manager        │
│  ECR images             │                 │  ECR (replicated)       │
└─────────────────────────┘                 └─────────────────────────┘
```

**Current DR tier:** Warm standby (RDS replica running, app tier starts on failover)
**Target RTO:** 2 hours | **Target RPO:** 1 hour

---

## 2. Step-by-Step Failover Procedure

### Step 1: Verify Primary Region Status (T+0 — T+15 min)

```bash
# Check AWS service health
curl https://health.aws.amazon.com/health/status | jq '.services[] | select(.region == "us-east-1")'

# Check if RDS primary is reachable
aws rds describe-db-instances \
  --db-instance-identifier prod-db \
  --region us-east-1 \
  --query 'DBInstances[0].DBInstanceStatus'

# Check ECS/EKS service status
aws ecs describe-services \
  --cluster prod-cluster \
  --services app-service \
  --region us-east-1 \
  --query 'services[0].runningCount'

# Document: what is unavailable, since when, last known good state
```

**Decision gate:** If primary is unrecoverable within 1 hour → proceed to Step 2.

---

### Step 2: Promote RDS Read Replica (T+15 — T+45 min)

> The read replica in eu-west-1 is typically 1–5 minutes behind primary. Check replica lag before promoting.

```bash
# Check replica lag before promoting
aws rds describe-db-instances \
  --db-instance-identifier prod-db-replica \
  --region eu-west-1 \
  --query 'DBInstances[0].StatusInfos'

# Check replication lag in seconds (CloudWatch)
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name ReplicaLag \
  --dimensions Name=DBInstanceIdentifier,Value=prod-db-replica \
  --start-time $(date -u -d '5 minutes ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 60 \
  --statistics Average \
  --region eu-west-1

# PROMOTE the replica (this breaks replication — it becomes a standalone writable instance)
aws rds promote-read-replica \
  --db-instance-identifier prod-db-replica \
  --region eu-west-1

# Wait for promotion to complete (typically 3-10 minutes)
aws rds wait db-instance-available \
  --db-instance-identifier prod-db-replica \
  --region eu-west-1

# Get new endpoint
aws rds describe-db-instances \
  --db-instance-identifier prod-db-replica \
  --region eu-west-1 \
  --query 'DBInstances[0].Endpoint.Address'
```

**Record the new DB endpoint — you'll need it in the next step.**

---

### Step 3: Update Application Configuration (T+45 — T+60 min)

Update the database endpoint in Secrets Manager (eu-west-1):

```bash
# Update DB connection string in Secrets Manager (DR region)
aws secretsmanager update-secret \
  --secret-id prod/database/connection \
  --secret-string '{"host":"[new-rds-endpoint]","port":5432,"database":"prod","username":"appuser","password":"[from-existing-secret]"}' \
  --region eu-west-1
```

If using environment variables via ECS task definitions or Kubernetes ConfigMaps:

```bash
# ECS: Update task definition with new DB endpoint
aws ecs register-task-definition \
  --cli-input-json file://task-definition-dr.json \
  --region eu-west-1

# Or update via Terraform:
cd terraform/dr-region
terraform apply -var="db_endpoint=[new-endpoint]" -auto-approve
```

---

### Step 4: Start Application Tier in DR Region (T+60 — T+90 min)

**Option A: ECS (if using Fargate):**

```bash
# Update ECS service to use new task definition
aws ecs update-service \
  --cluster prod-cluster-dr \
  --service app-service \
  --task-definition app-service:latest \
  --desired-count 3 \
  --region eu-west-1

# Monitor deployment
aws ecs wait services-stable \
  --cluster prod-cluster-dr \
  --services app-service \
  --region eu-west-1
```

**Option B: EKS (if using Kubernetes):**

```bash
# Update kubeconfig for DR cluster
aws eks update-kubeconfig \
  --name prod-cluster-dr \
  --region eu-west-1

# Apply updated configmap with DR database endpoint
kubectl apply -f kubernetes/dr-configmap.yaml

# Scale up deployment
kubectl scale deployment app-deployment --replicas=3 -n production

# Watch rollout
kubectl rollout status deployment/app-deployment -n production
```

**Option C: Terraform (full infrastructure):**

```bash
cd terraform/environments/dr
terraform plan  # verify what will be created
terraform apply -auto-approve
```

---

### Step 5: Verify Application Health (T+90 — T+100 min)

```bash
# Check ALB health
aws elbv2 describe-target-health \
  --target-group-arn arn:aws:elasticloadbalancing:eu-west-1:[account-id]:targetgroup/[tg-name]/[id] \
  --region eu-west-1

# Run smoke tests against DR endpoint
curl -f https://dr.internal.[company.com]/health
curl -f https://dr.internal.[company.com]/api/v1/ping

# Check database connectivity from application
kubectl exec -it deploy/app-deployment -n production -- \
  python -c "import psycopg2; conn = psycopg2.connect(host='[dr-db-endpoint]', ...); print('DB OK')"

# Check error rate in CloudWatch
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApplicationELB \
  --metric-name HTTPCode_Target_5XX_Count \
  --dimensions Name=LoadBalancer,Value=[alb-arn-suffix] \
  --start-time $(date -u -d '5 minutes ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 300 \
  --statistics Sum \
  --region eu-west-1
```

---

### Step 6: Failover DNS (T+100 — T+110 min)

> Only fail over DNS after application health checks pass. DNS failover is the customer-visible step.

**If using Route 53 health-check-based failover (recommended — automatic):**

```bash
# Verify health check status
aws route53 list-health-checks \
  --query 'HealthChecks[?HealthCheckConfig.FullyQualifiedDomainName==`[primary-endpoint]`].Id'

# If automatic failover hasn't triggered, manually update the failover routing
aws route53 change-resource-record-sets \
  --hosted-zone-id [ZONE_ID] \
  --change-batch file://dns-failover-dr.json
```

`dns-failover-dr.json`:
```json
{
  "Changes": [{
    "Action": "UPSERT",
    "ResourceRecordSet": {
      "Name": "api.[company.com]",
      "Type": "CNAME",
      "TTL": 60,
      "ResourceRecords": [{"Value": "[dr-alb-dns-name]"}]
    }
  }]
}
```

**If using Cloudflare:**
```bash
# Update Cloudflare DNS via API
curl -X PATCH "https://api.cloudflare.com/client/v4/zones/[ZONE_ID]/dns_records/[RECORD_ID]" \
  -H "Authorization: Bearer [CF_API_TOKEN]" \
  -H "Content-Type: application/json" \
  --data '{"content":"[dr-alb-dns-name]","proxied":true}'
```

**After DNS failover:** Wait 60–120 seconds (TTL) then verify via external DNS lookup:
```bash
dig +short api.[company.com]  # should return DR ALB IP
curl -f https://api.[company.com]/health
```

---

### Step 7: Post-Failover Monitoring (T+110 — T+240 min)

```bash
# Set up enhanced monitoring alarm in DR region
aws cloudwatch put-metric-alarm \
  --alarm-name "DR-ErrorRate-High" \
  --namespace AWS/ApplicationELB \
  --metric-name HTTPCode_Target_5XX_Count \
  --threshold 50 \
  --comparison-operator GreaterThanThreshold \
  --period 60 \
  --evaluation-periods 3 \
  --alarm-actions arn:aws:sns:eu-west-1:[account-id]:alerts \
  --region eu-west-1

# Monitor application logs
aws logs tail /aws/ecs/app-service --region eu-west-1 --follow

# Watch key metrics: error rate, latency, CPU, DB connections
```

**Declare DR Complete when:**
- [ ] Application health checks passing for 10 consecutive minutes
- [ ] Error rate < 1%
- [ ] P95 latency < [target]ms
- [ ] No critical alarms firing
- [ ] Status page updated: "Service restored. Monitoring closely."

---

## 3. Failback Procedure (Returning to Primary Region)

> Perform failback only after primary region is confirmed stable and healthy.

**Typical failback timing:** 24–48 hours after primary recovery (don't rush).

### Step 1: Restore Primary Infrastructure

```bash
# Verify primary region is healthy
aws rds describe-db-instances \
  --db-instance-identifier prod-db \
  --region us-east-1 \
  --query 'DBInstances[0].DBInstanceStatus'

# If primary DB was lost, restore from latest backup
aws rds restore-db-instance-to-point-in-time \
  --source-db-instance-identifier prod-db-replica \
  --target-db-instance-identifier prod-db-restored \
  --restore-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%SZ) \
  --region us-east-1
```

### Step 2: Sync Data from DR to Primary

```bash
# Export data that accumulated in DR during failover
pg_dump -h [dr-db-endpoint] -U appuser prod > prod_dr_export_$(date +%Y%m%d).sql

# Import into restored primary (after verifying integrity)
psql -h [primary-db-endpoint] -U appuser prod < prod_dr_export_$(date +%Y%m%d).sql
```

### Step 3: Establish New Replica in DR

After failback, recreate the DR replica from the primary:

```bash
aws rds create-db-instance-read-replica \
  --db-instance-identifier prod-db-replica \
  --source-db-instance-identifier prod-db \
  --db-instance-class db.t3.medium \
  --availability-zone eu-west-1a \
  --region eu-west-1
```

### Step 4: Fail DNS Back to Primary

```bash
# Reverse the DNS change from Step 6 of failover
# Point api.[company.com] back to primary ALB
```

---

## 4. Ransomware-Specific Recovery

> If recovering from ransomware, do NOT restore infected infrastructure. Rebuild clean.

```bash
# Step 1: Identify last known-clean backup (before infection)
aws backup list-recovery-points-by-backup-vault \
  --backup-vault-name prod-backup-vault \
  --query 'RecoveryPoints[?Status==`COMPLETED`].[RecoveryPointArn,CreationDate]' \
  --output table

# Step 2: Restore RDS from that backup
aws backup start-restore-job \
  --recovery-point-arn [clean-recovery-point-arn] \
  --iam-role-arn arn:aws:iam::[account-id]:role/AWSBackupRestoreRole \
  --resource-type RDS \
  --metadata '{"DBInstanceIdentifier":"prod-db-clean","DBSubnetGroupName":"default","MultiAZ":"false"}'

# Step 3: Rebuild application infrastructure from IaC (DO NOT restore from AMI/snapshot)
cd terraform/environments/production
git checkout [last-known-clean-commit]  # audit git log before choosing
terraform apply

# Step 4: Rotate ALL credentials before bringing service live
# - RDS master password
# - All IAM keys
# - All secrets in Secrets Manager
# - All API keys (Stripe, SendGrid, etc.)
# - Revoke all user sessions (Entra ID / Okta)
```

---

## 5. DR Test Record

| Test Date | Type | RTO Achieved | RPO Achieved | Issues Found | Actions Taken |
|-----------|------|-------------|-------------|-------------|--------------|
| | Tabletop | N/A | N/A | | |
| | Partial failover (DB only) | | | | |
| | Full failover | | | | |

**Next scheduled DR test:** [date]
**Test owner:** DevOps Lead

---

## 6. Emergency Contacts for DR

| Contact | Role | Direct Number | Notes |
|---------|------|--------------|-------|
| [Name] | DevOps Lead | | Primary runbook executor |
| [Name] | CTO | | Decision authority |
| [Name] | AWS Enterprise Support | [support case URL] | Use if AWS-level issue |
| [Name] | Cloudflare Support | | If DNS/CDN issue |
| [Name] | Database vendor support | | RDS issue escalation |
