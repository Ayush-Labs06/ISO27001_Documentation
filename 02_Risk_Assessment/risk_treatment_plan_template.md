# Risk Treatment Plan
**ISO 27001:2022 — Clause 6.1.3 | Version 1.0**
**Owner:** [ISMS Owner]
**Approved by:** [CEO/CTO Name] | [Date]

---

## Purpose

This plan documents the selected treatment actions for all risks identified in the Risk Register where the residual risk is not within the acceptance threshold. It links each risk to:
- The treatment option (Treat / Tolerate / Transfer / Terminate)
- Specific controls to implement (with Annex A reference)
- Owner, effort, and target completion date
- Target residual risk after treatment

---

## Treatment Actions

### Priority 1 — Critical Risks (Immediate Action)

| Risk ID | Risk Summary | Treatment | Annex A Controls | Action | Owner | Effort | Target Date | Target Residual |
|---------|-------------|---------|-----------------|--------|-------|--------|-------------|----------------|
| R-001 | PII DB — no MFA on admin access | Treat | A.8.2, A.8.5, A.5.18 | Enforce MFA for all IAM users with RDS access; implement least-privilege IAM roles; remove standing admin access | Eng Lead | 2 days | [+2 weeks] | 🟡 Medium (8) |
| R-005 | AWS root account — no hardware MFA | Treat | A.8.2, A.8.5 | Enable hardware MFA (YubiKey/virtual) on root; delete root API keys; SCP preventing root API use | DevOps Lead | 1 day | [+1 week] | 🟢 Low (4) |
| R-006 | S3 public bucket exposure risk | Treat | A.8.3, A.5.15 | Enable S3 block public access at account level; AWS Config rule `s3-bucket-public-access-prohibited`; CloudTrail alert on any public access change | DevOps Lead | 1 day | [+1 week] | 🟢 Low (4) |
| R-013 | Phishing — no security training | Treat | A.6.3, A.8.7 | Launch annual training programme (KnowBe4/Proofpoint/self-hosted); phishing simulation within 3 months; track completion | HR + ISMS Owner | 2 weeks | [+6 weeks] | 🟡 Medium (9) |

---

### Priority 2 — High Risks (Within 3 months)

| Risk ID | Risk Summary | Treatment | Annex A Controls | Action | Owner | Effort | Target Date | Target Residual |
|---------|-------------|---------|-----------------|--------|-------|--------|-------------|----------------|
| R-002 | SQL injection — no SAST | Treat | A.8.25, A.8.28, A.8.29 | Add Semgrep/SonarQube to CI/CD; add OWASP ZAP DAST in staging; developer secure coding training | Eng Lead | 1 week | [+6 weeks] | 🟡 Medium (6) |
| R-003 | Secrets in Git — no scanning | Treat | A.5.17, A.8.4 | git-secrets/trufflehog pre-commit hook; GitHub push protection; audit existing repos for exposed secrets; rotate any found | DevOps Lead | 3 days | [+2 weeks] | 🟢 Low (4) |
| R-004 | Supply chain — malicious packages | Treat | A.8.25, A.5.21 | Implement Dependabot/Renovate; add Snyk or OWASP Dependency-Check in CI; pin Dockerfile FROM hashes | DevOps Lead | 1 week | [+4 weeks] | 🟡 Medium (8) |
| R-007 | Ransomware — untested backups | Treat | A.8.13, A.5.30 | Document backup procedure; implement monthly restore test (first Tuesday of month); verify S3 Object Lock on backup bucket | DevOps Lead | 3 days | [+4 weeks] | 🟡 Medium (6) |
| R-009 | Ex-employee access not revoked | Treat | A.6.5, A.5.18 | Document offboarding checklist; enable SCIM provisioning from IdP; access revoked within 24h of last day | HR + IT Lead | 1 week | [+4 weeks] | 🟢 Low (4) |
| R-011 | Accidental PII disclosure | Treat | A.5.12, A.5.13, A.8.12 | Google Workspace DLP policy for PII patterns; data classification training; AUP update | HR + IT Lead | 2 weeks | [+6 weeks] | 🟡 Medium (8) |
| R-014 | Backups in same account as production | Treat | A.8.13, A.5.29 | AWS Backup cross-account; S3 Object Lock (COMPLIANCE mode); test cross-account restore | DevOps Lead | 3 days | [+3 weeks] | 🟢 Low (4) |
| R-015 | No supplier security assessment | Treat | A.5.19, A.5.20 | Conduct supplier questionnaire for all Critical/Important suppliers; maintain supplier register; annual review | ISMS Owner | 2 weeks | [+8 weeks] | 🟡 Medium (8) |

---

### Priority 3 — Medium Risks (Within 6 months)

| Risk ID | Risk Summary | Treatment | Annex A Controls | Action | Owner | Effort | Target Date | Target Residual |
|---------|-------------|---------|-----------------|--------|-------|--------|-------------|----------------|
| R-008 | Laptop theft — FDE not enforced | Treat | A.7.9, A.8.1 | Enforce FileVault/BitLocker via MDM policy; monthly compliance report | IT Lead | 1 week | [+6 weeks] | 🟢 Low (3) |
| R-010 | Log tampering / deletion | Treat | A.8.15 | CloudTrail log file validation; S3 Object Lock on log bucket; alert on CloudTrail changes | DevOps Lead | 1 day | [+2 weeks] | 🟢 Low (3) |
| R-012 | DDoS — no rate limiting | Treat | A.8.20, A.8.26 | WAF rate limiting rules (100 req/IP/min); AWS Shield Standard (free); CloudFront caching | DevOps Lead | 2 days | [+4 weeks] | 🟢 Low (4) |

---

## Residual Risk Acceptance

After implementing all treatment actions above, the following residual risks will remain. These must be formally accepted by the relevant risk owner:

| Risk ID | Residual Score | Risk Summary | Accepted by | Date |
|---------|---------------|-------------|------------|------|
| R-002 | 🟡 6 | SQL injection risk — SAST reduces but not eliminates | Engineering Lead | |
| R-004 | 🟡 8 | Supply chain — scanning reduces but third-party risk remains | DevOps Lead | |
| R-013 | 🟡 9 | Phishing — training reduces but cannot eliminate human error | ISMS Owner | |

---

## Treatment Plan Progress

| Risk ID | Status | Completed Date | Evidence Reference |
|---------|--------|---------------|--------------------|
| R-001 | ⬜ Not started / 🟡 In progress / ✅ Complete | | |
| R-002 | | | |
| R-003 | | | |
| R-004 | | | |
| R-005 | | | |
| R-006 | | | |
| R-007 | | | |
| R-008 | | | |
| R-009 | | | |
| R-010 | | | |
| R-011 | | | |
| R-012 | | | |
| R-013 | | | |
| R-014 | | | |
| R-015 | | | |

---

## Approval Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| ISMS Owner | | | |
| CEO / Top Management | | | |
