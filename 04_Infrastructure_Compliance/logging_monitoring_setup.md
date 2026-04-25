# Logging & Monitoring Setup
**ISO 27001:2022 — A.8.15 (Logging), A.8.16 (Monitoring) | Version 1.0**
**Owner:** DevOps Lead

---

## 1. What Must Be Logged (A.8.15)

ISO 27001 and security best practice require logs for:

| Category | What to Log | Retention |
|----------|------------|-----------|
| Authentication | Login attempts (success + failure), MFA events, password resets | 12 months |
| Authorization | Access grants, access denials, privilege escalation | 12 months |
| Administrative | Config changes, IAM changes, security group changes | 12 months |
| System | Service starts/stops, OS events, crash logs | 90 days |
| Application | Errors, exceptions, API calls with user context | 90 days |
| Data access | Access to Restricted data (PII queries, admin panel) | 12 months |
| Network | VPC Flow Logs, WAF logs, DNS query logs | 90 days |
| Endpoint | EDR events, MDM compliance events | 90 days |

---

## 2. AWS Logging Stack

### Minimum Required Services

```
AWS CloudTrail     → API calls (who did what, when, from where)
AWS GuardDuty      → Threat detection (ML-based anomaly detection)
AWS Security Hub   → Aggregated security findings + compliance posture
AWS Config         → Resource configuration tracking + drift detection
VPC Flow Logs      → Network traffic metadata
CloudWatch Logs    → Centralized log aggregation
```

### CloudTrail Setup

```bash
# Create S3 bucket for CloudTrail (in a logging account or locked bucket)
BUCKET="org-cloudtrail-logs-$(date +%Y)"
aws s3api create-bucket --bucket $BUCKET --region eu-west-1 \
  --create-bucket-configuration LocationConstraint=eu-west-1

# Block public access on log bucket
aws s3api put-public-access-block --bucket $BUCKET \
  --public-access-block-configuration \
  "BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true"

# Enable Object Lock (immutable logs — governance mode allows override by admin)
aws s3api put-object-lock-configuration --bucket $BUCKET \
  --object-lock-configuration \
  '{"ObjectLockEnabled":"Enabled","Rule":{"DefaultRetention":{"Mode":"GOVERNANCE","Days":365}}}'

# Create multi-region CloudTrail
aws cloudtrail create-trail \
  --name org-management-trail \
  --s3-bucket-name $BUCKET \
  --is-multi-region-trail \
  --include-global-service-events \
  --enable-log-file-validation \
  --cloud-watch-logs-log-group-arn [cloudwatch-log-group-arn] \
  --cloud-watch-logs-role-arn [cloudtrail-cloudwatch-role-arn]

aws cloudtrail start-logging --name org-management-trail
```

### CloudWatch Log Groups — Required

```bash
# Create log groups with 12-month retention
for log_group in \
  "/aws/cloudtrail/management-events" \
  "/aws/guardduty/findings" \
  "/aws/vpc/flowlogs" \
  "/aws/eks/cluster/[cluster-name]/audit" \
  "/application/myapp/production"; do
  
  aws logs create-log-group --log-group-name "$log_group"
  aws logs put-retention-policy \
    --log-group-name "$log_group" \
    --retention-in-days 365
done
```

---

## 3. Security Alerting — CloudWatch Alarms (Must-Have)

Create metric filters + alarms for these events:

```bash
# Helper: create alarm from metric filter
create_alarm() {
  local name=$1 filter=$2 metric=$3 threshold=$4
  
  aws logs put-metric-filter \
    --log-group-name "/aws/cloudtrail/management-events" \
    --filter-name "$name" \
    --filter-pattern "$filter" \
    --metric-transformations metricName="$metric",metricNamespace="CIS/Alarms",metricValue=1
  
  aws cloudwatch put-metric-alarm \
    --alarm-name "$name" \
    --metric-name "$metric" \
    --namespace "CIS/Alarms" \
    --statistic Sum \
    --period 300 \
    --threshold $threshold \
    --comparison-operator GreaterThanOrEqualToThreshold \
    --evaluation-periods 1 \
    --alarm-actions [sns-topic-arn] \
    --treat-missing-data notBreaching
}

# Alarm: Root account usage
create_alarm "root-account-usage" \
  '{$.userIdentity.type="Root" && $.userIdentity.invokedBy NOT EXISTS && $.eventType !="AwsServiceEvent"}' \
  "RootAccountUsage" 1

# Alarm: Console sign-in without MFA
create_alarm "console-signin-no-mfa" \
  '{$.eventName="ConsoleLogin" && $.additionalEventData.MFAUsed="No"}' \
  "ConsoleSigninNoMFA" 1

# Alarm: IAM policy change
create_alarm "iam-policy-change" \
  '{$.eventName=DeleteGroupPolicy || $.eventName=DeleteRolePolicy || $.eventName=PutGroupPolicy || $.eventName=PutRolePolicy || $.eventName=AttachGroupPolicy || $.eventName=AttachRolePolicy || $.eventName=DetachGroupPolicy || $.eventName=DetachRolePolicy}' \
  "IAMPolicyChange" 1

# Alarm: CloudTrail configuration change
create_alarm "cloudtrail-config-change" \
  '{$.eventName=CreateTrail || $.eventName=UpdateTrail || $.eventName=DeleteTrail || $.eventName=StartLogging || $.eventName=StopLogging}' \
  "CloudTrailChange" 1

# Alarm: Security group change
create_alarm "security-group-change" \
  '{$.eventName=AuthorizeSecurityGroupIngress || $.eventName=AuthorizeSecurityGroupEgress || $.eventName=RevokeSecurityGroupIngress || $.eventName=RevokeSecurityGroupEgress || $.eventName=CreateSecurityGroup || $.eventName=DeleteSecurityGroup}' \
  "SecurityGroupChange" 1

# Alarm: S3 bucket policy change
create_alarm "s3-bucket-policy-change" \
  '{$.eventSource="s3.amazonaws.com" && $.eventName=PutBucketAcl || $.eventName=PutBucketPolicy || $.eventName=PutBucketCors || $.eventName=PutBucketLifecycle || $.eventName=PutBucketReplication || $.eventName=DeleteBucketPolicy || $.eventName=DeleteBucketCors || $.eventName=DeleteBucketLifecycle || $.eventName=DeleteBucketReplication}' \
  "S3BucketPolicyChange" 1

# Alarm: KMS CMK deletion or disable
create_alarm "kms-key-disable-delete" \
  '{$.eventSource="kms.amazonaws.com" && $.errorCode!="*" && ($.eventName=DisableKey || $.eventName=ScheduleKeyDeletion)}' \
  "KMSKeyChange" 1

# Alarm: Failed AWS console logins (brute force indicator)
create_alarm "failed-console-logins" \
  '{$.eventName=ConsoleLogin && $.errorMessage="Failed authentication"}' \
  "FailedConsoleLogin" 3
```

---

## 4. GuardDuty Configuration

```bash
# Enable GuardDuty with malware protection
aws guardduty create-detector \
  --enable \
  --finding-publishing-frequency FIFTEEN_MINUTES \
  --features '[{"Name":"S3_DATA_EVENTS","Status":"ENABLED"},{"Name":"EKS_AUDIT_LOGS","Status":"ENABLED"},{"Name":"MALWARE_PROTECTION","Status":"ENABLED"}]'

# Get detector ID
DETECTOR_ID=$(aws guardduty list-detectors --query 'DetectorIds[0]' --output text)

# Create SNS alert for HIGH/CRITICAL findings
aws guardduty create-threat-intel-set \
  --detector-id $DETECTOR_ID \
  --name "ThreatIntelFeed" \
  --format TXT \
  --location s3://[bucket]/threat-intel.txt \
  --activate

# EventBridge rule to alert on GuardDuty HIGH+ findings
aws events put-rule \
  --name "guardduty-high-findings" \
  --event-pattern '{
    "source": ["aws.guardduty"],
    "detail-type": ["GuardDuty Finding"],
    "detail": {
      "severity": [{"numeric": [">=", 7]}]
    }
  }'
```

---

## 5. Application Logging Standards

Every application log entry must include:

```json
{
  "timestamp": "2024-01-15T10:23:45.123Z",
  "level": "INFO|WARN|ERROR|SECURITY",
  "service": "myapp-api",
  "environment": "production",
  "request_id": "req-abc123",
  "user_id": "usr-xyz789",  // anonymized/tokenized if PII
  "ip": "1.2.3.4",
  "method": "POST",
  "path": "/api/users",
  "status": 201,
  "duration_ms": 145,
  "event": "user.created"
}
```

**Security-critical events to log:**
- Authentication success/failure (with IP)
- Authorization failure (user, resource, action)
- Sensitive data access (admin panel, PII queries)
- Account changes (password change, MFA enable/disable)
- Admin actions (user created/deleted, role assigned)
- Bulk data operations (exports, mass deletes)

**Never log:**
- Passwords (even failed login passwords)
- Full credit card numbers or bank account numbers
- Unmasked PII in debug logs (use tokenized/hashed IDs)
- Session tokens or JWT payloads

---

## 6. Log Integrity and Tamper Prevention

```bash
# CloudTrail log file validation (enabled at creation)
# Verify log file integrity
aws cloudtrail validate-logs \
  --trail-arn [trail-arn] \
  --start-time 2024-01-01T00:00:00Z \
  --end-time 2024-01-31T23:59:59Z \
  --verbose

# S3 server access logging (who accessed the log bucket)
aws s3api put-bucket-logging --bucket [cloudtrail-bucket] \
  --bucket-logging-status '{
    "LoggingEnabled": {
      "TargetBucket": "[log-access-bucket]",
      "TargetPrefix": "cloudtrail-access-logs/"
    }
  }'
```

---

## 7. Log Review Process (A.8.16)

| Review | Frequency | Who | Method |
|--------|-----------|-----|--------|
| GuardDuty HIGH/CRITICAL findings | Real-time (alerting) | On-call | PagerDuty / Slack alert |
| Security Hub score + recommendations | Weekly | ISMS Owner | Dashboard review |
| CloudWatch security alarms | Real-time (alerting) | On-call | PagerDuty / SNS → Slack |
| IAM credential report | Monthly | IT Lead | Scripted report |
| VPC Flow Logs anomalies | Weekly | DevOps | Athena query |
| Application error logs | Daily | Engineering | CloudWatch Insights |
| Failed login reports | Weekly | IT Lead | CloudWatch Insights |

```bash
# CloudWatch Insights: failed logins in last 7 days
aws logs start-query \
  --log-group-name "/aws/cloudtrail/management-events" \
  --start-time $(date -d '7 days ago' +%s) \
  --end-time $(date +%s) \
  --query-string 'fields @timestamp, @message
    | filter eventName = "ConsoleLogin" and errorMessage = "Failed authentication"
    | stats count(*) as failures by sourceIPAddress
    | sort failures desc
    | limit 20'
```
