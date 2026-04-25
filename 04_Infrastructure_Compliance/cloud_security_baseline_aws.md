# AWS Cloud Security Baseline
**ISO 27001:2022 — A.5.23, A.8.2, A.8.5, A.8.15, A.8.16, A.8.20 | CIS AWS Foundations Benchmark v3.0**
**Owner:** DevOps Lead
**Review Cycle:** Quarterly + on architecture change

For each item: ✅ Done | ⚠️ Partial | ❌ Gap | N/A

---

## 1. Identity & Access Management (IAM)

### Root Account
| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 1.1 | Root account has MFA enabled (hardware or virtual) | | | A.8.5 |
| 1.2 | Root account has no active access keys | | `aws iam get-account-summary` → AccountAccessKeysPresent=0 | A.8.2 |
| 1.3 | Root account is not used for daily tasks | | CloudTrail shows no root API calls | A.8.2 |
| 1.4 | Root credentials stored offline in break-glass procedure | | | A.8.2 |

```bash
# Check root account status
aws iam get-account-summary --query 'SummaryMap.{RootMFA:AccountMFAEnabled,RootKeys:AccountAccessKeysPresent}'

# Check for recent root activity (last 90 days)
aws cloudtrail lookup-events --lookup-attributes AttributeKey=Username,AttributeValue=root \
  --start-time $(date -d '90 days ago' +%s) --query 'Events[].{Time:EventTime,Event:EventName}'
```

### IAM Users
| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 1.5 | All IAM users have MFA enabled | | `aws iam generate-credential-report` | A.8.5 |
| 1.6 | No IAM users with console access AND programmatic access (split roles) | | Credential report | A.8.2 |
| 1.7 | Access keys rotated within 90 days | | Credential report: `access_key_1_last_rotated` | A.5.17 |
| 1.8 | No access keys older than 90 days | | Credential report | A.5.17 |
| 1.9 | IAM users with no activity in 90+ days are disabled | | Credential report: `password_last_used` | A.5.18 |
| 1.10 | No IAM users with AdministratorAccess managed policy attached | | Use IAM roles; no inline admin policies | A.8.2 |
| 1.11 | IAM password policy: min 14 chars, no reuse (last 24), MFA required | | | A.5.17 |

```bash
# Generate and decode credential report
aws iam generate-credential-report
sleep 5
aws iam get-credential-report --output text --query Content | base64 --decode > iam_credential_report.csv

# Check password policy
aws iam get-account-password-policy

# List users with AdministratorAccess
aws iam list-users | jq '.Users[].UserName' | xargs -I {} sh -c \
  'echo "=== {} ===" && aws iam list-attached-user-policies --user-name {} | jq .AttachedPolicies'
```

### IAM Roles and Policies
| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 1.12 | CI/CD uses OIDC roles (not long-lived access keys) | | GitHub Actions OIDC configured | A.8.5 |
| 1.13 | No wildcard (*) resource in production IAM policies | | Policy review; aws_iam_policy analysis | A.8.2 |
| 1.14 | EC2/Lambda/ECS tasks use IAM roles (not instance keys) | | Instance metadata service v2 (IMDSv2) enforced | A.8.5 |
| 1.15 | IMDSv2 enforced on all EC2 instances | | Prevents SSRF-based metadata credential theft | A.8.5 |
| 1.16 | Service Control Policies (SCPs) applied to restrict dangerous actions | | Org SCPs: deny public S3, deny root usage, deny region exceptions | A.8.2 |

```bash
# Check IMDSv2 on all EC2 instances
aws ec2 describe-instances --query \
  'Reservations[].Instances[].{ID:InstanceId,IMDSv2:MetadataOptions.HttpTokens}' \
  --output table

# Find overly permissive policies
aws iam list-policies --scope Local | jq '.Policies[].PolicyName' | xargs -I {} sh -c \
  'aws iam get-policy-version --policy-arn $(aws iam list-policies --scope Local \
  --query "Policies[?PolicyName==\`{}\`].Arn" --output text) \
  --version-id $(aws iam list-policy-versions --policy-arn ... --query "Versions[?IsDefaultVersion].VersionId" --output text) \
  --query "PolicyVersion.Document.Statement[?Resource==\`*\`]"'
```

---

## 2. Logging & Monitoring

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 2.1 | CloudTrail enabled in all regions (multi-region trail) | | | A.8.15 |
| 2.2 | CloudTrail log file validation enabled | | Protects log integrity | A.8.15 |
| 2.3 | CloudTrail logs delivered to S3 with access logging | | | A.8.15 |
| 2.4 | CloudTrail S3 bucket is not publicly accessible | | | A.8.15 |
| 2.5 | CloudTrail S3 bucket has S3 Object Lock or MFA delete | | Log immutability | A.8.15 |
| 2.6 | Log retention ≥ 12 months (CloudWatch Logs retention set) | | CloudWatch log group retention ≥ 365 days | A.8.15 |
| 2.7 | GuardDuty enabled in all regions and accounts | | Threat detection | A.8.16 |
| 2.8 | AWS Security Hub enabled; CIS AWS Foundations standard active | | Compliance posture visibility | A.8.16 |
| 2.9 | AWS Config enabled in all regions | | Resource configuration tracking | A.8.9 |
| 2.10 | VPC Flow Logs enabled for all production VPCs | | Network traffic monitoring | A.8.16 |
| 2.11 | S3 server access logging enabled for sensitive buckets | | | A.8.15 |
| 2.12 | RDS Performance Insights + CloudWatch alarms for anomalies | | | A.8.16 |
| 2.13 | CloudWatch alarms for: root login, failed MFA, console sign-in without MFA | | | A.8.16 |

```bash
# Enable CloudTrail (multi-region)
aws cloudtrail create-trail \
  --name org-trail \
  --s3-bucket-name [your-cloudtrail-bucket] \
  --is-multi-region-trail \
  --enable-log-file-validation \
  --include-global-service-events

aws cloudtrail start-logging --name org-trail

# Enable GuardDuty
aws guardduty create-detector --enable --finding-publishing-frequency FIFTEEN_MINUTES

# Check VPC Flow Logs
aws ec2 describe-flow-logs --query 'FlowLogs[*].{VPC:ResourceId,Status:FlowLogStatus,Destination:LogDestination}'

# Set CloudWatch Log Group retention
aws logs put-retention-policy --log-group-name [group-name] --retention-in-days 365
```

---

## 3. Storage Security (S3)

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 3.1 | S3 Block Public Access enabled at account level | | Single command; highest priority | A.8.3 |
| 3.2 | All S3 buckets have encryption enabled (SSE-S3 or SSE-KMS) | | | A.8.24 |
| 3.3 | S3 buckets with sensitive data use SSE-KMS (customer CMK) | | | A.8.24 |
| 3.4 | S3 bucket versioning enabled for backup/data buckets | | | A.8.13 |
| 3.5 | S3 Object Lock enabled on backup and log buckets | | | A.8.13 |
| 3.6 | S3 bucket policies do not allow `*` principal | | | A.8.3 |
| 3.7 | AWS Config rule `s3-bucket-public-access-prohibited` active | | | A.8.3 |
| 3.8 | S3 lifecycle policies set for data retention compliance | | | A.8.10 |

```bash
# Enable S3 block public access at ACCOUNT level
aws s3control put-public-access-block \
  --account-id [account-id] \
  --public-access-block-configuration \
    "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"

# Enable default encryption on a bucket
aws s3api put-bucket-encryption --bucket [bucket-name] \
  --server-side-encryption-configuration \
  '{"Rules":[{"ApplyServerSideEncryptionByDefault":{"SSEAlgorithm":"aws:kms","KMSMasterKeyID":"[key-arn]"}}]}'

# Enable Object Lock on a bucket (bucket must be created with versioning)
aws s3api put-object-lock-configuration --bucket [bucket-name] \
  --object-lock-configuration '{"ObjectLockEnabled":"Enabled","Rule":{"DefaultRetention":{"Mode":"GOVERNANCE","Days":90}}}'

# Check all buckets for public access settings
aws s3api list-buckets --query 'Buckets[].Name' --output text | tr '\t' '\n' | \
  xargs -I {} aws s3api get-public-access-block --bucket {} 2>&1
```

---

## 4. Database Security (RDS)

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 4.1 | All RDS instances encrypted at rest | | `StorageEncrypted: true` | A.8.24 |
| 4.2 | RDS instances in private subnets (not public) | | `PubliclyAccessible: false` | A.8.20 |
| 4.3 | RDS security groups: only allow access from app servers (not 0.0.0.0/0) | | | A.8.20 |
| 4.4 | RDS automated backups enabled (retention ≥ 7 days) | | | A.8.13 |
| 4.5 | Multi-AZ enabled for production RDS | | | A.8.14 |
| 4.6 | RDS minor version auto-upgrade enabled | | | A.8.8 |
| 4.7 | RDS deletion protection enabled | | Prevents accidental deletion | A.8.13 |
| 4.8 | Database credentials in Secrets Manager (not env vars) | | | A.5.17 |

```bash
# Check all RDS instances for encryption and public accessibility
aws rds describe-db-instances \
  --query 'DBInstances[*].{ID:DBInstanceIdentifier,Encrypted:StorageEncrypted,Public:PubliclyAccessible,MultiAZ:MultiAZ,BackupRetention:BackupRetentionPeriod}' \
  --output table
```

---

## 5. Network Security

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 5.1 | No security groups with inbound 0.0.0.0/0 on port 22 (SSH) | | | A.8.20 |
| 5.2 | No security groups with inbound 0.0.0.0/0 on port 3389 (RDP) | | | A.8.20 |
| 5.3 | Default VPC not used in production | | | A.8.20 |
| 5.4 | VPC has public and private subnets (production workloads in private) | | | A.8.22 |
| 5.5 | NAT Gateway used for private subnet outbound (not direct internet) | | | A.8.20 |
| 5.6 | WAF attached to CloudFront / ALB for public-facing apps | | | A.8.20 |
| 5.7 | AWS Shield Standard active (free; automatic on CloudFront/ALB) | | | A.8.20 |
| 5.8 | EKS clusters: no public API server endpoint (or restrict to CIDR) | | | A.8.20 |

```bash
# Find security groups with 0.0.0.0/0 on SSH/RDP
aws ec2 describe-security-groups \
  --query 'SecurityGroups[?IpPermissions[?((IpRanges[?CidrIp==`0.0.0.0/0`]) && (FromPort==`22` || ToPort==`3389`))]].[GroupId,GroupName]' \
  --output table
```

---

## 6. KMS and Encryption

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 6.1 | EBS encryption by default enabled at account level | | | A.8.24 |
| 6.2 | KMS CMK rotation enabled for all customer-managed keys | | | A.8.24 |
| 6.3 | KMS key policies do not allow `*` principal | | | A.8.24 |
| 6.4 | Separate KMS keys per environment (prod/dev/staging) | | | A.8.24 |

```bash
# Enable EBS encryption by default
aws ec2 enable-ebs-encryption-by-default --region [region]

# Check CMK rotation status
aws kms list-keys --query 'Keys[].KeyId' --output text | tr '\t' '\n' | \
  xargs -I {} aws kms get-key-rotation-status --key-id {}
```

---

## 7. Vulnerability Management

| # | Control | Status | Evidence / Notes | Annex A |
|---|---------|--------|-----------------|---------|
| 7.1 | Amazon Inspector enabled for EC2 + ECR scanning | | | A.8.8 |
| 7.2 | ECR image scanning enabled on push | | | A.8.8 |
| 7.3 | AWS Security Hub aggregates findings centrally | | | A.8.8 |
| 7.4 | Critical findings reviewed and remediated within 7 days | | | A.8.8 |

```bash
# Enable ECR image scanning
aws ecr put-image-scanning-configuration \
  --repository-name [repo-name] \
  --image-scanning-configuration scanOnPush=true

# Get Inspector findings summary
aws inspector2 list-findings \
  --filter-criteria '{"findingSeverity":[{"comparison":"EQUALS","value":"CRITICAL"}]}' \
  --query 'findings[*].{Title:title,Severity:severity,Resource:resources[0].id}'
```

---

## Baseline Score Summary

| Domain | Controls | Passing | % |
|--------|---------|---------|---|
| IAM | 16 | | |
| Logging & Monitoring | 13 | | |
| Storage (S3) | 8 | | |
| Database (RDS) | 8 | | |
| Networking | 8 | | |
| KMS & Encryption | 4 | | |
| Vulnerability Mgmt | 4 | | |
| **Total** | **61** | | |
