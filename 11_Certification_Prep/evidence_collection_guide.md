# Evidence Collection Guide
**ISO 27001:2022 — Certification Preparation | Version 1.0**
**Owner:** ISMS Owner
**Purpose:** Per-control guide on what evidence auditors expect and how to collect/present it

---

## How to Use This Guide

For each control domain, this guide lists:
- What the auditor will ask for
- How to collect the evidence
- How to present it
- Common mistakes

Collect evidence into a structured folder: `Evidence/[Control-Domain]/[Evidence-Item].[ext]`

---

## A.5 Organisational Controls — Key Evidence

### A.5.1 — Policies for Information Security

**What auditors want:**
- IS Policy document signed by CEO
- Evidence it was communicated to staff (training records, email distribution)
- Evidence of review (version history, review date in document)

**How to collect:**
- PDF of signed IS Policy
- LMS screenshot showing policy module completion OR email evidence of distribution
- Document version history showing it was updated within the last 12 months

**Common mistake:** Policy signed but version hasn't changed in 3 years = questions about whether it's actually reviewed.

---

### A.5.9 — Asset Register

**What auditors want:**
- A real asset register with meaningful entries (not 3 rows)
- Asset owners named
- Classification applied to each asset

**How to collect:**
- Export or screenshot of asset register
- Show at least 15–20 assets covering different types (data, software, infrastructure, people)

---

### A.5.15/A.5.18 — Access Control and Access Rights

**What auditors want:**
- Access control policy
- Evidence of access reviews (who reviewed, when, what was found)
- Evidence of provisioning/deprovisioning process

**How to collect:**

```bash
# IAM credential report (run fresh on audit day or day before)
aws iam generate-credential-report
sleep 15
aws iam get-credential-report --query 'Content' --output text | base64 -d > iam_credential_report_$(date +%Y%m%d).csv
```
- Completed access review templates (last 2 quarters)
- Offboarding checklist — 2-3 completed examples

---

### A.5.19–A.5.22 — Supplier Management

**What auditors want:**
- Subprocessor register with Tier 1 suppliers clearly marked
- Completed assessments for Tier 1 suppliers (questionnaire + risk rating)
- DPA records

**How to collect:**
- Subprocessor register (current version, review dates visible)
- PDF of completed supplier assessments (at least 2 Tier 1)
- DPA screenshots or PDFs (AWS click-through DPA confirmation, etc.)

---

### A.5.24–A.5.27 — Incident Response

**What auditors want:**
- IRP document
- Evidence of incidents being logged and managed
- Evidence of a tabletop exercise
- Post-incident reviews

**How to collect:**
- IRP v[X] signed document
- Incident log showing at least 3-6 months of records (even if only P3/P4)
- Tabletop exercise: agenda + attendance list + findings log
- At least 1 completed post-incident review section from an incident log

---

### A.5.29–A.5.30 — Business Continuity

**What auditors want:**
- BCP document
- RTO/RPO defined and documented
- Evidence of testing (tabletop or actual DR test)

**How to collect:**
- BCP document
- RTO/RPO worksheet
- DR test record (what was tested, when, outcome, RTO/RPO achieved)
- Backup test records (quarterly restore test logs)

---

## A.6 People Controls — Key Evidence

### A.6.1 — Screening

**What auditors want:**
- Screening policy
- Evidence that background checks occur pre-employment

**How to collect:**
- HR policy describing screening process
- Records showing checks were conducted (DBS/criminal check receipt, reference check confirmation)
- Note: auditors typically don't review actual check results — just evidence checks occur

---

### A.6.3 — Awareness, Education and Training

**What auditors want:**
- Annual training programme plan
- Completion records for all mandatory training
- Phishing simulation results

**How to collect:**
- Training programme plan (from `08_Awareness_Training/`)
- LMS export showing: employee name, course, date, result
- Phishing simulation reports (last 2-4 quarterly reports)

```
# Evidence format auditors prefer for training:
Name | Course | Date Completed | Pass Mark | Score
Jane Smith | IS Fundamentals | 2024-02-15 | 80% | 92%
```

---

### A.6.5 — Termination Responsibilities

**What auditors want:**
- Offboarding procedure
- Completed offboarding checklists for recent leavers

**How to collect:**
- Offboarding procedure document
- 2-3 completed offboarding checklists (anonymise name if preferred)
- Sample showing: date, all access systems checked, signed by IT Lead and HR

---

## A.7 Physical Controls — Key Evidence

### A.7.7 — Clear Desk and Clear Screen

**What auditors want:**
- Clear desk/screen policy
- MDM evidence showing screen lock enforced

**How to collect:**

```bash
# Intune — export device compliance report showing screen lock policy applied
# Navigate: Endpoint Manager > Devices > Monitor > Device compliance
```
- Screenshot of MDM screen lock policy configuration
- If spot-checking: auditor may ask to walk through office or ask engineer to lock screen in front of them

---

### A.7.9 — Security of Assets Off-Premises

**What auditors want:**
- AUP covering remote work
- MDM showing FDE on all devices

**How to collect:**
- AUP (relevant section on remote work)
- MDM compliance report showing encryption status per device
- VPN configuration (that it's required for remote work)

---

## A.8 Technological Controls — Key Evidence

### A.8.2 — Privileged Access Rights

**What auditors want:**
- Evidence of PAM implementation
- Evidence of JIT / least-privilege
- Break-glass account controls

**How to collect:**
- SSM Session Manager logs (showing production access goes through SSM, not SSH keys)
- Entra PIM activation log export
- Break-glass CloudWatch alarm configuration screenshot

```bash
# Export SSM session history
aws ssm describe-sessions \
  --state Active \
  --region us-east-1 \
  --query 'Sessions[*].[SessionId,StartDate,Target,Details.User]'
```

---

### A.8.8 — Management of Technical Vulnerabilities

**What auditors want:**
- Vulnerability scan results (current)
- Evidence of remediation against SLAs
- Patch compliance report

**How to collect:**

```bash
# Export Inspector findings
aws inspector2 list-findings \
  --filter-criteria '{"awsAccountId":[{"comparison":"EQUALS","value":"[account-id]"}]}' \
  --query 'findings[*].[title,severity.label,firstObservedAt,status]' \
  --output table > inspector_findings_$(date +%Y%m%d).txt

# SSM Patch Manager compliance report
aws ssm list-compliance-summaries \
  --filters Key=ComplianceType,Values=Patch \
  --query 'ComplianceSummaryItems[*].[InstanceId,Status,Details]'
```

---

### A.8.13 — Information Backup

**What auditors want:**
- Backup configuration showing automated backups
- Restore test records (quarterly)
- Cross-region backup evidence

**How to collect:**

```bash
# AWS Backup — list recent backup jobs
aws backup list-backup-jobs \
  --by-state COMPLETED \
  --by-created-after $(date -u -d '30 days ago' +%Y-%m-%dT%H:%M:%SZ) \
  --query 'BackupJobs[*].[BackupJobId,CreationDate,CompletionDate,ResourceType,Status]' \
  --output table

# Export as evidence of backup completion
```
- Restore test log (from `backup_recovery_procedure.md` test script output)

---

### A.8.15 — Logging

**What auditors want:**
- CloudTrail enabled in all regions
- Log retention configured
- Evidence logs are reviewed

**How to collect:**

```bash
# Verify CloudTrail is enabled in all regions
aws cloudtrail describe-trails --include-shadow-trails \
  --query 'trailList[*].[Name,IsMultiRegionTrail,S3BucketName,LogFileValidationEnabled]'

# Verify log retention
aws logs describe-log-groups \
  --query 'logGroups[*].[logGroupName,retentionInDays]'
```
- CloudWatch alarms list (showing alerts on critical events)
- Evidence of log review (weekly GuardDuty review notes or ticket)

---

### A.8.24 — Use of Cryptography

**What auditors want:**
- Cryptography policy
- Evidence that policy is enforced (RDS encrypted, TLS enforced, etc.)

**How to collect:**

```bash
# Verify RDS encryption
aws rds describe-db-instances \
  --query 'DBInstances[*].[DBInstanceIdentifier,StorageEncrypted,KmsKeyId]'

# Verify S3 default encryption
aws s3api get-bucket-encryption --bucket [prod-bucket]

# Check for any HTTP (non-TLS) listeners on load balancers
aws elbv2 describe-listeners \
  --load-balancer-arn [alb-arn] \
  --query 'Listeners[*].[Port,Protocol]'
```
- KMS key list + rotation status

```bash
aws kms list-keys --query 'Keys[*].KeyId' | \
  xargs -I{} aws kms describe-key --key-id {} \
  --query 'KeyMetadata.[KeyId,Description,KeyRotationEnabled,Enabled]'
```

---

### A.8.25 — Secure Development Life Cycle

**What auditors want:**
- Security in CI/CD pipeline
- SAST results
- Code review evidence (branch protection, CODEOWNERS)

**How to collect:**
- GitHub Actions workflow YAML (showing security scan jobs)
- Recent pipeline run screenshot (showing security scan passed/flagged)
- GitHub branch protection rules screenshot
- CODEOWNERS file content
- Example PR showing security review comment or approval

---

## Evidence Folder Structure (Suggested)

```
Evidence/
├── A5_Organisational/
│   ├── IS_Policy_v1.1_signed.pdf
│   ├── SOA_v2.0_signed.pdf
│   ├── Risk_Register_2024.xlsx
│   ├── Subprocessor_Register.xlsx
│   ├── Supplier_Assessment_AWS.pdf
│   ├── Incident_Log_2024.pdf
│   ├── Tabletop_Exercise_Record_2024.pdf
│   └── Management_Review_Minutes_2024.pdf
├── A6_People/
│   ├── Training_Completion_Report_Q4_2024.csv
│   ├── Phishing_Simulation_Reports/
│   ├── Signed_AUPs/ (anonymised samples)
│   └── Offboarding_Records/ (anonymised samples)
├── A7_Physical/
│   ├── MDM_Compliance_Report_2024.pdf
│   └── Clear_Screen_Policy_Evidence.png
├── A8_Technological/
│   ├── IAM_Credential_Report_[date].csv
│   ├── Access_Reviews/
│   ├── Inspector_Findings_[date].txt
│   ├── CloudTrail_Config_Screenshot.png
│   ├── RDS_Encryption_Verified.txt
│   ├── S3_BlockPublicAccess_Screenshot.png
│   ├── Backup_Test_Records/
│   ├── CI_Pipeline_Security_Screenshot.png
│   └── GuardDuty_Enabled_Screenshot.png
└── Governance/
    ├── Internal_Audit_Report_IA-2024-001.pdf
    ├── NCR_Records/
    ├── Management_Review_Pack_2024/
    └── KPI_Dashboard_Q4_2024.pdf
```
