# Backup and Recovery Procedure
**ISO 27001:2022 — A.8.13, A.5.29, A.5.30 | Version 1.0**
**Owner:** DevOps Lead

---

## 1. Backup Requirements

| System | Backup Method | Frequency | Retention | RTO | RPO | Encryption |
|--------|--------------|-----------|-----------|-----|-----|-----------|
| Production RDS | Automated snapshots + AWS Backup | Daily snapshot + continuous WAL | 35 days snapshots; 7 days PITR | 2h | 5 min | KMS CMK |
| S3 data buckets | S3 Versioning + Cross-region replication | Continuous (versioning) | 90-day lifecycle | 1h | Real-time | SSE-KMS |
| EFS / shared storage | AWS Backup | Daily | 35 days | 4h | 24h | KMS |
| Secrets Manager | AWS-managed; multi-AZ | Continuous | AWS-managed | N/A | N/A | AWS-managed |
| Terraform state | S3 versioning | Per apply | Indefinite (versioned) | N/A | N/A | SSE-KMS |
| GitHub repositories | GitHub-native; export quarterly | Per commit + quarterly export | Indefinite | N/A | N/A | GitHub-managed |
| Employee data (Google/M365) | Google Vault / M365 Backup | Continuous | Per retention policy | 24h | 24h | Provider-managed |

---

## 2. AWS Backup Configuration

```bash
# Create backup vault with KMS encryption
aws backup create-backup-vault \
  --backup-vault-name production-backup-vault \
  --encryption-key-arn arn:aws:kms:eu-west-1:[account]:key/[key-id] \
  --tags Environment=production,Classification=Restricted

# Enable vault lock (immutable backups — WORM)
aws backup put-backup-vault-lock-configuration \
  --backup-vault-name production-backup-vault \
  --min-retention-days 7 \
  --max-retention-days 365

# Create backup plan
aws backup create-backup-plan --backup-plan '{
  "BackupPlanName": "production-daily",
  "Rules": [
    {
      "RuleName": "daily-35day-retention",
      "TargetBackupVaultName": "production-backup-vault",
      "ScheduleExpression": "cron(0 3 * * ? *)",
      "StartWindowMinutes": 60,
      "CompletionWindowMinutes": 120,
      "Lifecycle": {
        "MoveToColdStorageAfterDays": 30,
        "DeleteAfterDays": 35
      }
    },
    {
      "RuleName": "monthly-1year-retention",
      "TargetBackupVaultName": "production-backup-vault",
      "ScheduleExpression": "cron(0 4 1 * ? *)",
      "Lifecycle": {
        "DeleteAfterDays": 365
      }
    }
  ]
}'

# Cross-region backup copy (disaster recovery)
aws backup create-backup-plan --backup-plan '{
  "BackupPlanName": "cross-region-dr",
  "Rules": [{
    "RuleName": "weekly-cross-region",
    "TargetBackupVaultName": "production-backup-vault",
    "ScheduleExpression": "cron(0 5 ? * SUN *)",
    "CopyActions": [{
      "DestinationBackupVaultArn": "arn:aws:backup:eu-central-1:[account]:backup-vault:dr-backup-vault",
      "Lifecycle": {"DeleteAfterDays": 90}
    }]
  }]
}'

# Assign resources to backup plan
aws backup create-backup-selection \
  --backup-plan-id [plan-id] \
  --backup-selection '{
    "SelectionName": "all-production",
    "IamRoleArn": "arn:aws:iam::[account]:role/AWSBackupDefaultServiceRole",
    "Resources": [],
    "ListOfTags": [
      {"ConditionType":"STRINGEQUALS","ConditionKey":"Environment","ConditionValue":"production"}
    ]
  }'
```

---

## 3. RDS Point-in-Time Recovery

```bash
# Enable automated backups with PITR (set on RDS creation or modification)
aws rds modify-db-instance \
  --db-instance-identifier prod-database \
  --backup-retention-period 35 \
  --preferred-backup-window "03:00-04:00" \
  --apply-immediately

# Enable deletion protection
aws rds modify-db-instance \
  --db-instance-identifier prod-database \
  --deletion-protection \
  --apply-immediately

# PITR restore to specific time
aws rds restore-db-instance-to-point-in-time \
  --source-db-instance-identifier prod-database \
  --target-db-instance-identifier prod-database-restored \
  --restore-time 2024-01-15T10:30:00Z \
  --vpc-security-group-ids [sg-id] \
  --db-subnet-group-name [subnet-group]
```

---

## 4. Backup Testing Schedule (Mandatory)

**ISO 27001 requires evidence of tested backups. Untested backups = no backups.**

| Test Type | Frequency | Owner | Evidence Required |
|-----------|-----------|-------|------------------|
| RDS restore to non-prod | Monthly (first Tuesday) | DevOps Lead | Screenshot of restored DB + connectivity test |
| S3 restore of sample files | Monthly | DevOps Lead | Files verified after restore |
| Full DR exercise | Semi-annual | DevOps + CTO | DR runbook executed; RTO/RPO measured |
| Cross-region restore | Annual | DevOps Lead | Restore to DR region verified |

### Monthly RDS Restore Test Procedure

```bash
#!/bin/bash
# Monthly backup test — run on first Tuesday of each month
# Document results in: backup_test_log.md

TEST_DATE=$(date +%Y-%m-%d)
SOURCE_DB="prod-database"
TEST_DB="prod-database-restore-test-${TEST_DATE}"

echo "=== Backup Restore Test: ${TEST_DATE} ===" | tee -a backup_test_log.md

# 1. Get latest automated snapshot
SNAPSHOT=$(aws rds describe-db-snapshots \
  --db-instance-identifier $SOURCE_DB \
  --snapshot-type automated \
  --query 'reverse(sort_by(DBSnapshots, &SnapshotCreateTime))[0].DBSnapshotIdentifier' \
  --output text)

echo "Snapshot: $SNAPSHOT" | tee -a backup_test_log.md

# 2. Restore snapshot to test instance
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier $TEST_DB \
  --db-snapshot-identifier $SNAPSHOT \
  --db-subnet-group-name [subnet-group] \
  --vpc-security-group-ids [test-sg-id]

# 3. Wait for restore to complete
aws rds wait db-instance-available --db-instance-identifier $TEST_DB
echo "Restore completed at: $(date)" | tee -a backup_test_log.md

# 4. Verify connectivity (basic connection test)
ENDPOINT=$(aws rds describe-db-instances \
  --db-instance-identifier $TEST_DB \
  --query 'DBInstances[0].Endpoint.Address' --output text)

pg_isready -h $ENDPOINT -p 5432 -U testuser && \
  echo "PASS: Database accessible" | tee -a backup_test_log.md || \
  echo "FAIL: Cannot connect to restored database" | tee -a backup_test_log.md

# 5. Run record count check
# psql -h $ENDPOINT -U testuser -d myapp -c "SELECT COUNT(*) FROM users;" 2>&1 | tee -a backup_test_log.md

# 6. Delete test instance (cost control)
aws rds delete-db-instance \
  --db-instance-identifier $TEST_DB \
  --skip-final-snapshot

echo "Test instance cleanup initiated" | tee -a backup_test_log.md
```

---

## 5. Backup Test Log Template

| Date | Type | System | Snapshot/Version | Restore Time | Result | Tester | Notes |
|------|------|--------|-----------------|-------------|--------|--------|-------|
| | RDS monthly | prod-database | rds:prod-2024-xx | ~45 min | ✅ Pass | [name] | Row count matched |
| | S3 file restore | data-bucket | versioned object | ~5 min | ✅ Pass | [name] | |
| | Full DR exercise | All systems | | 4h 15min | ⚠️ Partial | [name] | RTO exceeded by 15 min |

---

## 6. RTO / RPO Targets vs Actuals

| System | RTO Target | RPO Target | Last Tested RTO | Last Tested RPO | Result |
|--------|-----------|-----------|----------------|----------------|--------|
| RDS production | 2h | 5 min | | | |
| Application tier | 1h | 15 min | | | |
| Full platform | 4h | 30 min | | | |

> If actual RTO/RPO exceeds targets: raise as a finding and update the risk register.

---

## 7. Backup Security

- All backups encrypted with KMS CMK (not AWS-managed keys for sensitive data)
- Backup vault locked (WORM — cannot be deleted before retention expires)
- Cross-account backup copies stored in isolated backup account (separate AWS account)
- Access to restore backups: DevOps Lead + ISMS Owner only (IAM policy-controlled)
- Backup access logged via CloudTrail and AWS Backup audit logs
