# RTO / RPO Worksheet
**ISO 27001:2022 — A.5.29, A.5.30 | Version 1.0**
**Owner:** DevOps Lead / CTO
**Updated:** [Date] | Review: After each DR test

---

## How to Use This Worksheet

1. **Section 1** — Set your targets (business decision, not technical)
2. **Section 2** — Document your current backup/replication setup (technical reality)
3. **Section 3** — Record actual DR test results (what you actually achieved)
4. **Section 4** — Identify and close gaps between targets and actuals

**RTO** = Recovery Time Objective — maximum time from failure to restored service.
**RPO** = Recovery Point Objective — maximum data loss window (i.e., how old can the backup be?).

---

## Section 1: Business-Defined Targets

Set by CEO/CTO based on business impact, customer SLAs, and contractual obligations.

| System / Function | MTD (Max Tolerable Downtime) | RTO Target | RPO Target | Business Justification |
|------------------|------------------------------|------------|------------|----------------------|
| Customer-facing SaaS platform | 4 hours | 2 hours | 1 hour | Customer contracts; SLA penalty at >4h |
| Production database (primary) | 4 hours | 2 hours | 1 hour | Core data store for all operations |
| Authentication service (Auth0/Okta) | 4 hours | 1 hour | N/A (vendor-managed) | No login = complete platform outage |
| API gateway / load balancer | 2 hours | 30 minutes | N/A (stateless) | Entry point for all traffic |
| Payment processing (Stripe) | 8 hours | N/A (Stripe manages) | N/A | Revenue; customer SLAs |
| Customer support (Zendesk) | 24 hours | 4 hours | N/A (vendor-managed) | Reputational; SLA response times |
| Internal communications (Slack) | 4 hours | 1 hour | N/A (vendor-managed) | Incident response coordination |
| Source code repository (GitHub) | 8 hours | 2 hours | N/A (vendor-managed) | Emergency hotfix capability |
| CI/CD pipeline | 24 hours | 4 hours | N/A | Non-critical during incident |
| Monitoring / alerting (Datadog) | 2 hours | 1 hour | N/A | Visibility during recovery |

**SLA commitments to customers:**
- Enterprise tier: 99.9% uptime = 8.76h/year downtime allowance ≈ max 43 min/month
- Standard tier: 99.5% uptime = 43.8h/year ≈ max 3.6h/month
- RTO of 2h implies single outage can consume 2 months of SLA budget — plan accordingly.

---

## Section 2: Current Backup and Replication Configuration

Document what is actually in place today.

### 2.1 Database (RDS PostgreSQL)

| Setting | Current Value | Required Value | Gap? |
|---------|--------------|----------------|------|
| Automated backup enabled? | Y / N | Y | |
| Backup retention period | [X] days | 35 days | |
| Backup window | [HH:MM-HH:MM UTC] | Off-peak window | |
| Point-in-time recovery (PITR) enabled? | Y / N | Y | |
| PITR granularity | 5 minutes | ≤5 min | |
| Read replica in DR region? | Y / N | Y | |
| Replica lag (average) | [X] seconds | <300 seconds | |
| Cross-region backup copy? | Y / N | Y | |
| Backup encryption (KMS)? | Y / N | Y | |
| Last backup test date | [date] | Within 90 days | |
| Last restore test result | Pass / Fail / Not tested | Pass | |

**Effective RPO from PITR:** 5 minutes (assuming replica lag <5 min)
**Effective RTO for DB restore from snapshot:** ~30-45 minutes

```bash
# Verify current backup configuration
aws rds describe-db-instances \
  --db-instance-identifier prod-db \
  --query 'DBInstances[0].{BackupRetention:BackupRetentionPeriod,PITR:LatestRestorableTime,MultiAZ:MultiAZ,StorageEncrypted:StorageEncrypted}'

# Check replica lag
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name ReplicaLag \
  --dimensions Name=DBInstanceIdentifier,Value=prod-db-replica \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%SZ) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%SZ) \
  --period 300 \
  --statistics Average Maximum \
  --region eu-west-1
```

---

### 2.2 Object Storage (S3)

| Bucket | Data Type | Versioning | CRR to DR Region | Last Modified |
|--------|-----------|------------|-----------------|---------------|
| prod-application-data | Application uploads | Enabled | Enabled | |
| prod-audit-logs | Security logs | Enabled | Enabled | |
| prod-backups | DB exports | Enabled | Enabled | |
| prod-static-assets | Frontend assets | Enabled | Not required (CDN cache) | |

```bash
# Verify versioning and CRR
aws s3api get-bucket-versioning --bucket prod-application-data
aws s3api get-bucket-replication --bucket prod-application-data
```

---

### 2.3 Application State (ECS / EKS)

| Component | Stateful? | Backup Method | RTO Impact |
|-----------|-----------|--------------|-----------|
| ECS tasks / Fargate | No — stateless | N/A; redeploy from ECR image | ~10-20 min to redeploy |
| Kubernetes pods | No — stateless | N/A; redeploy from Helm/manifests | ~10-20 min |
| Persistent volumes (EBS) | Yes (if any) | EBS snapshots (daily) | ~20-30 min to restore |
| Redis cache (ElastiCache) | Warm cache only | Backup enabled; acceptable to lose | Cache miss on restore; DB handles load |
| ECR container images | Yes | Images immutable; replicated to DR ECR | ~5 min to pull to DR |

```bash
# Check ECR replication configuration
aws ecr describe-registry --query 'replicationConfiguration'

# Verify images exist in DR region
aws ecr list-images \
  --repository-name [app-name] \
  --region eu-west-1
```

---

### 2.4 Secrets and Configuration

| Secret Store | Backup Method | DR Access |
|-------------|--------------|----------|
| AWS Secrets Manager (us-east-1) | Manual export to DR region | DR Secrets Manager (eu-west-1) — manual sync required |
| Terraform state (S3) | Versioned S3 + DynamoDB lock | Replicated via CRR |
| GitHub Actions secrets | Not backed up | Must recreate manually if GitHub unavailable |
| 1Password (break-glass) | Vendor-managed HA | Accessible from any region |

**Gap:** Secrets Manager sync is manual. Automate with Lambda or document the sync procedure in the DR runbook.

---

## Section 3: DR Test Results

Record every test here. Auditors want to see that targets are actually tested, not just documented.

### Test 1 — [Date]

| Metric | Target | Actual | Pass/Fail | Notes |
|--------|--------|--------|-----------|-------|
| Time to detect failure | — | [X] min | | Time from failure to alert firing |
| Time to make DR decision | — | [X] min | | Decision authority available? |
| DB replica promotion time | 30 min | [X] min | | |
| Application startup time | 20 min | [X] min | | |
| DNS propagation time | 5 min | [X] min | | TTL matters here |
| Health check pass time | 10 min | [X] min | | Time from start to clean health |
| **Total RTO achieved** | **2 hours** | **[X] hours** | | |
| **RPO achieved** (data age at failover) | **1 hour** | **[X] min** | | Check replica lag log |

**Test type:** [ ] Tabletop only  [ ] Partial (DB failover only)  [ ] Full failover  [ ] Full failover + failback

**Issues found:**
1.
2.

**Actions taken after test:**
1.
2.

---

### Test 2 — [Date]

| Metric | Target | Actual | Pass/Fail | Notes |
|--------|--------|--------|-----------|-------|
| **Total RTO achieved** | **2 hours** | | | |
| **RPO achieved** | **1 hour** | | | |

---

## Section 4: Gap Analysis and Improvement Actions

| Gap Identified | Impact | Priority | Owner | Target Closure | Status |
|---------------|--------|---------|-------|---------------|--------|
| Secrets Manager not synced to DR | RTO impact +30 min (manual secret recreation) | High | DevOps Lead | [date] | Open |
| DR failover not automated (Route 53 health checks not configured) | Manual DNS step adds 30 min to RTO | High | DevOps Lead | [date] | Open |
| DR app tier cold start (needs image pull) | Adds 15 min to RTO | Medium | DevOps Lead | [date] | Open |
| Failback procedure not tested | Unknown data loss risk on failback | High | DevOps Lead | [date] | Open |
| No automated DR test in CI/CD | Tests only when someone remembers | Medium | ISMS Owner | [date] | Open |

---

## Section 5: RTO/RPO Improvement Levers

If current RTO/RPO don't meet targets, options by cost/complexity:

| Current State | Improvement | New RTO | Cost Impact |
|--------------|-------------|---------|------------|
| Manual DNS failover | Route 53 health check + automated failover | -30 min | Low (configuration) |
| Cold app tier in DR | Warm standby (minimum 1 task/pod always running) | -15 min | Medium (running cost) |
| Read replica only | Multi-AZ primary + read replica | -30 min | High (2× DB cost) |
| Manual secrets sync | Lambda replication for Secrets Manager | -30 min | Low (development time) |
| Monolith architecture | Microservices with independent failover | -60 min | Very High |
| Single cloud | Multi-cloud (AWS + Azure active-active) | -90 min | Very High |

**Recommendation for most startups:** Automated DNS failover + warm app tier + PITR → achieves 2h RTO / 1h RPO at reasonable cost.

---

## Section 6: Approval and Sign-Off

| Field | Value |
|-------|-------|
| Targets approved by | [CEO / CTO name] |
| Approval date | [date] |
| Last test date | [date] |
| Next test scheduled | [date] |
| Next review date | [date] |

**Retain test records for minimum 3 years as ISMS evidence.**
