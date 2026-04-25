# Information Security Risk Register
**ISO 27001:2022 — Clause 6.1.2 | Version 1.0**
**Owner:** [ISMS Owner]
**Last Updated:** YYYY-MM-DD
**Next Review:** YYYY-MM-DD

---

## How to Use This Register

1. Each row = one risk (one asset + one threat + one vulnerability combination)
2. Inherent risk = before controls
3. Existing controls = what's already in place
4. Residual risk = after existing controls
5. Treatment decision = what to do next
6. Target residual = what you aim to achieve with new controls

**Scores:** L=Likelihood (1-5), I=Impact (1-5), R=Risk Score (L×I)

---

## Risk Register

| Risk ID | Asset | Threat | Vulnerability | Inherent L | Inherent I | Inherent R | Rating | Existing Controls | Residual L | Residual I | Residual R | Rating | Treatment | New Controls Needed | Risk Owner | Due Date | Status |
|---------|-------|--------|--------------|-----------|-----------|-----------|--------|-----------------|-----------|-----------|-----------|--------|-----------|--------------------|-----------:|----------|--------|
| R-001 | Customer PII database (INF-001) | External attacker — credential stuffing (T1.4) | No MFA on admin access (V-T07) | 4 | 5 | 20 | 🔴 | Passwords required; DB in private subnet | 3 | 5 | 15 | 🟠 | Treat | Enforce MFA on all DB admin accounts; implement least privilege IAM | Engineering Lead | [date] | Open |
| R-002 | Customer PII database (INF-001) | External attacker — SQL injection (T1.5) | No SAST in CI/CD (V-T13) | 3 | 5 | 15 | 🟠 | ORM used; code review process | 2 | 5 | 10 | 🟠 | Treat | Add SAST (Semgrep/SonarQube); add DAST in staging | Engineering Lead | [date] | Open |
| R-003 | Encryption keys (INF-003) | External attacker — secret exposure (T1.14) | Credentials possibly committed to Git (V-T03) | 3 | 5 | 15 | 🟠 | .gitignore for .env files | 3 | 5 | 15 | 🟠 | Treat | git-secrets / trufflehog in pre-commit + CI; migrate to Secrets Manager | DevOps Lead | [date] | Open |
| R-004 | Source code (INF-004) | Supply chain attack — malicious npm package (T1.8) | No SCA scanning (V-T13) | 3 | 4 | 12 | 🟠 | Manual dependency review | 3 | 4 | 12 | 🟠 | Treat | Implement Dependabot/Renovate + SCA (Snyk/OWASP Dependency Check) | DevOps Lead | [date] | Open |
| R-005 | AWS Production Account (NET-001) | External attacker — IAM compromise via phishing (T1.1) | No hardware MFA on root account (V-T07) | 4 | 5 | 20 | 🔴 | Root account rarely used | 4 | 5 | 20 | 🔴 | Treat | Enable hardware MFA on root; disable root API keys; use SCPs to prevent root use | DevOps Lead | [date] | Open |
| R-006 | AWS Production Account (NET-001) | Misconfiguration — public S3 bucket (T3.2) | No S3 public access block (V-T05) | 4 | 4 | 16 | 🔴 | Review on creation | 4 | 4 | 16 | 🔴 | Treat | Enable S3 account-level public access block; enable AWS Config rule; CloudTrail alert | DevOps Lead | [date] | Open |
| R-007 | Production application (APP-001) | Ransomware — encrypt application data (T1.3) | No tested backup restore process (V-P08) | 2 | 5 | 10 | 🟠 | Daily snapshots configured | 2 | 4 | 8 | 🟡 | Treat | Document backup procedure; test monthly restore; verify backup immutability | DevOps Lead | [date] | Open |
| R-008 | Employee laptops (HW-001) | Laptop theft (T5.3) | Full disk encryption not enforced on all devices (V-T20) | 3 | 3 | 9 | 🟡 | FileVault enabled on Macs | 2 | 3 | 6 | 🟡 | Treat | Enforce full disk encryption via MDM policy; verify in monthly MDM report | IT Lead | [date] | Open |
| R-009 | All systems | Unauthorized access by ex-employee (T2.6) | No documented offboarding checklist (V-P02) | 3 | 4 | 12 | 🟠 | Manual offboarding by HR | 3 | 4 | 12 | 🟠 | Treat | Document and implement offboarding checklist; IdP auto-provision/deprovision | HR + IT Lead | [date] | Open |
| R-010 | CloudWatch Logs / S3 (INF-010) | Attacker covers tracks — log deletion (T1.13) | No log integrity controls | 2 | 4 | 8 | 🟡 | CloudTrail enabled | 2 | 3 | 6 | 🟡 | Treat | Enable CloudTrail log file validation; S3 Object Lock on log bucket | DevOps Lead | [date] | Open |
| R-011 | Customer PII database (INF-001) | Employee accidental disclosure — send to wrong recipient (T3.3) | No DLP controls | 3 | 4 | 12 | 🟠 | Training on data handling | 3 | 3 | 9 | 🟡 | Treat | Google Workspace DLP rules; data classification labels; AUP training | HR + IT Lead | [date] | Open |
| R-012 | Production application (APP-001) | DDoS attack (T1.9) | No DDoS protection | 2 | 4 | 8 | 🟡 | CloudFront in front of app | 2 | 3 | 6 | 🟡 | Treat | Enable AWS Shield Standard; configure WAF rate limiting | DevOps Lead | [date] | Open |
| R-013 | Any system | Phishing → credential compromise (T1.1) | No security awareness training (V-P05) | 4 | 4 | 16 | 🔴 | Informal advice given | 4 | 3 | 12 | 🟠 | Treat | Launch annual training programme; run phishing simulation | HR + ISMS Owner | [date] | Open |
| R-014 | Backup data (INF-009) | Ransomware encrypts backups (T1.3) | Backups in same account as production | 2 | 5 | 10 | 🟠 | Daily backups | 2 | 5 | 10 | 🟠 | Treat | Cross-account backup vault (AWS Backup); enable immutable backups (Object Lock) | DevOps Lead | [date] | Open |
| R-015 | Supplier SaaS tools | Supplier data breach (T6.1) | No supplier security assessment (V-P06) | 3 | 4 | 12 | 🟠 | Suppliers chosen by reputation | 3 | 3 | 9 | 🟡 | Treat | Conduct supplier risk questionnaire for all critical suppliers; review annually | ISMS Owner | [date] | Open |
| | | | | | | | | | | | | | | | | | |

---

## Risk Summary Dashboard

| Rating | Count | % of Total |
|--------|-------|-----------|
| 🔴 Critical (16-25) | | |
| 🟠 High (10-15) | | |
| 🟡 Medium (5-9) | | |
| 🟢 Low (1-4) | | |
| **Total risks** | | |

---

## Risk Acceptance Log

Risks accepted without further treatment (requires ISMS Owner + CEO sign-off):

| Risk ID | Risk Summary | Residual Score | Accepted By | Date | Rationale |
|---------|-------------|---------------|------------|------|-----------|
| | | | | | |

---

## Change Log

| Date | Version | Changed By | Change |
|------|---------|-----------|--------|
| | 1.0 | | Initial risk assessment |
