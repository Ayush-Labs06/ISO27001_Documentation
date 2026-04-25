# Data Classification Policy
**ISO 27001:2022 — A.5.12, A.5.13, A.5.14 | Version 1.0**
**Owner:** ISMS Owner
**Approved by:** [CEO] | [Date]

---

## 1. Classification Tiers

| Tier | Label | Description | Examples |
|------|-------|-------------|---------|
| 1 | **Public** | Approved for public distribution | Marketing materials, public docs, job postings, press releases |
| 2 | **Internal** | Internal use only; limited external impact if disclosed | Internal memos, non-sensitive processes, public-facing employee names |
| 3 | **Confidential** | Significant harm if disclosed | Source code, financial data, business strategy, employee salaries, contracts |
| 4 | **Restricted** | Severe harm if disclosed; regulatory obligations | Customer PII, authentication credentials, encryption keys, audit findings |

---

## 2. Handling Requirements by Classification

### Public (Tier 1)
| Handling | Requirement |
|----------|------------|
| Storage | Anywhere |
| Transmission | Any method |
| Sharing | No restrictions |
| Disposal | Normal waste |
| Labelling | Optional |

### Internal (Tier 2)
| Handling | Requirement |
|----------|------------|
| Storage | Company systems only |
| Transmission | Company email / approved tools |
| Sharing | Employees and authorized contractors only |
| Disposal | Standard deletion; no special requirement |
| Labelling | "Internal" header/footer on documents |

### Confidential (Tier 3)
| Handling | Requirement |
|----------|------------|
| Storage | Encrypted storage only; company systems |
| Transmission | Encrypted channels only (TLS; no plain email for sensitive content) |
| Sharing | Need-to-know only; manager approval for external sharing |
| Disposal | Secure deletion (shred paper; wipe / NIST-erase digital) |
| Labelling | "Confidential" header/footer; metadata where applicable |
| Cloud storage | Private buckets/drives only; no public links |
| Email | Do not attach unencrypted; use secure file share or encrypted email |

### Restricted (Tier 4)
| Handling | Requirement |
|----------|------------|
| Storage | Encrypted; access-controlled; production systems only |
| Transmission | Encrypted channels; additional access controls |
| Sharing | Strictly need-to-know; written approval from ISMS Owner or CEO |
| Disposal | NIST SP 800-88 compliant deletion; physical media destroyed |
| Labelling | "Restricted" header; access controls enforced at system level |
| Cloud storage | KMS-encrypted; SCPs preventing public access |
| Copying | No copies to personal devices, personal cloud, or unapproved storage |
| Logging | All access logged and monitored |

---

## 3. Common Data Types — Classification Reference

| Data Type | Classification | Notes |
|-----------|---------------|-------|
| Customer names, emails, addresses | Restricted | Personal data — GDPR applies |
| Customer usage data / behavioural data | Restricted | Personal data |
| Payment card data | Restricted | PCI-DSS scope (if applicable) |
| Authentication credentials (hashed) | Restricted | Must be hashed; never plaintext |
| Encryption keys, API secrets | Restricted | Secrets management required |
| Employee personal data | Restricted | Employment law + GDPR |
| Source code | Confidential | IP; competitive sensitivity |
| Financial reports, revenue figures | Confidential | M&A sensitivity |
| Business contracts, legal docs | Confidential | Legal privilege |
| Security audit reports, risk register | Confidential | Attacker value |
| System architecture diagrams | Confidential | Attacker value |
| Internal Slack messages (non-sensitive) | Internal | |
| Project documentation | Internal | |
| Employee directory | Internal | |
| Marketing materials | Public | |
| Public documentation / API docs | Public | |

---

## 4. Labelling

### Digital Documents
Apply labels in document headers, footers, or metadata:
- Google Docs: Use [Document Classification Labels plugin] or footer text
- Notion pages: Classification badge at top of page
- Emails: `[CONFIDENTIAL]` or `[RESTRICTED]` in subject line for high-sensitivity content
- Code comments: `// CONTAINS: [classification]` for files with embedded sensitive data

### Cloud Storage
- S3 bucket tagging: `DataClassification: Restricted|Confidential|Internal|Public`
- Apply using Terraform:
```hcl
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket"
  tags = {
    DataClassification = "Restricted"
    DataOwner          = "engineering-lead"
  }
}
```

---

## 5. Data Minimisation and Retention

- Collect only data necessary for business purpose (GDPR principle of data minimisation)
- Personal data retention limits:

| Data Type | Retention Period | Basis |
|-----------|-----------------|-------|
| Customer account data | Duration of account + [X] years | Contract |
| Customer PII (inactive accounts) | [X] months after account deletion | Regulatory |
| Employee personal data | Employment period + 7 years | Employment law |
| Security audit logs | 12 months minimum | ISO 27001 / A.8.15 |
| Incident records | 3 years | ISO 27001 |
| Financial records | 7 years | Statutory |

- Data deletion must use appropriate methods:
  - AWS S3: Object deletion + bucket versioning lifecycle expiry
  - RDS: `DELETE` + `VACUUM` / table drop + key deletion
  - Physical: Cross-cut shredder or certified destruction service

---

## 6. Classification in Practice

### "When in doubt — classify up"

If you're unsure whether data is Internal or Confidential, treat it as Confidential until clarified.

### Reclassification

Data may be reclassified if:
- Business context changes
- Retention period expires (downgrade to "for deletion")
- Data becomes public (e.g., on product launch)

Reclassification requires approval from the asset owner.

---

## 7. Third-Party Sharing

Restricted and Confidential data shared with third parties must:
- Be covered by a signed DPA or NDA
- Use encrypted transfer channels
- Be logged in the supplier register
- Have a defined purpose and retention period in the agreement

| Field | Detail |
|-------|--------|
| Document ID | ISMS-DC-001 |
| Owner | ISMS Owner |
| Review cycle | Annual |
| Next review | [Date] |
