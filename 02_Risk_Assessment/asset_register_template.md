# Information Asset Register
**ISO 27001:2022 — A.5.9 | Version 1.0**
**Owner:** [IT Lead / ISMS Owner]
**Review Cycle:** Annual (or when significant change occurs)

---

## Asset Classification Guide

### CIA Impact Ratings (per asset)
- **C** = Confidentiality: 1 (Public) / 2 (Internal) / 3 (Confidential) / 4 (Restricted)
- **I** = Integrity: 1 (Low) / 2 (Medium) / 3 (High) / 4 (Critical)
- **A** = Availability: 1 (Low) / 2 (Medium) / 3 (High) / 4 (Critical)

### Asset Classification (overall)
Take the maximum of C, I, A ratings:

| Max Score | Classification | Label |
|-----------|---------------|-------|
| 4 | Restricted | Customer PII, credentials, encryption keys, IP |
| 3 | Confidential | Business strategy, financial data, source code, contracts |
| 2 | Internal | Internal docs, employee data, system configs |
| 1 | Public | Marketing materials, public documentation |

---

## Primary Assets — Information

| ID | Asset Name | Description | Owner | Classification | C | I | A | Location | Notes |
|----|-----------|-------------|-------|---------------|---|---|---|----------|-------|
| INF-001 | Customer PII database | Names, emails, addresses, usage data | Engineering Lead | Restricted | 4 | 3 | 4 | AWS RDS (eu-west-1) | GDPR applies |
| INF-002 | Authentication credentials | User passwords (hashed), session tokens | Engineering Lead | Restricted | 4 | 4 | 4 | AWS RDS + Redis | Must be hashed at rest |
| INF-003 | Encryption keys | KMS keys, TLS certificates | DevOps Lead | Restricted | 4 | 4 | 3 | AWS KMS | Annual rotation |
| INF-004 | Source code | Application source code, IaC | Engineering Lead | Confidential | 3 | 4 | 3 | GitHub (private org) | Branch protection required |
| INF-005 | Financial data | Invoice data, revenue figures, bank details | Finance Lead | Restricted | 4 | 4 | 2 | Google Sheets + Stripe | Finance lead access only |
| INF-006 | Employee personal data | Names, salaries, bank details, HR records | HR Manager | Restricted | 4 | 3 | 2 | Google Workspace (HR folder) | Employment law obligations |
| INF-007 | Business contracts | Customer contracts, supplier agreements, NDAs | Legal/CEO | Confidential | 3 | 4 | 2 | Google Drive (Legal folder) | Signed copies only |
| INF-008 | Security documentation | Policies, risk register, audit reports | ISMS Owner | Confidential | 3 | 4 | 2 | [ISMS document store] | Version controlled |
| INF-009 | Backup data | Database snapshots, backup archives | DevOps Lead | Confidential | 3 | 4 | 3 | AWS S3 (versioned) | Encrypted at rest |
| INF-010 | Audit logs | CloudTrail, application logs, security events | DevOps Lead | Internal | 2 | 4 | 3 | CloudWatch Logs / S3 | 1-year retention minimum |
| | | | | | | | | | |

## Primary Assets — Software & Applications

| ID | Asset Name | Description | Owner | Classification | C | I | A | Location | Notes |
|----|-----------|-------------|-------|---------------|---|---|---|----------|-------|
| APP-001 | Production application | Customer-facing SaaS platform | Engineering Lead | Confidential | 3 | 4 | 4 | AWS (ECS/EKS) | Revenue-generating |
| APP-002 | Internal admin dashboard | Internal admin panel with elevated access | Engineering Lead | Confidential | 3 | 4 | 3 | AWS (private) | MFA + VPN required |
| APP-003 | CI/CD pipeline | GitHub Actions / deployment pipeline | DevOps Lead | Confidential | 3 | 4 | 3 | GitHub Actions | OIDC auth preferred |
| APP-004 | Identity Provider (IdP) | Okta / Azure Entra ID / Google Workspace | IT Lead | Restricted | 4 | 4 | 4 | SaaS (cloud) | SSO master — protect carefully |
| APP-005 | Source code management | GitHub org | Engineering Lead | Confidential | 3 | 4 | 3 | GitHub (SaaS) | Supplier assessment required |
| APP-006 | Customer support platform | Zendesk / Intercom | Support Lead | Confidential | 3 | 3 | 3 | SaaS | Contains customer PII in tickets |
| | | | | | | | | | |

## Supporting Assets — Hardware

| ID | Asset | Description | Owner | Serial/Tag | Location | Classification | Notes |
|----|-------|-------------|-------|-----------|----------|---------------|-------|
| HW-001 | Employee laptops (×N) | MacBook Pro / ThinkPad | IT Lead | See MDM | Remote / Office | Confidential | Encrypted, MDM enrolled |
| HW-002 | Office network equipment | Router, switches | IT Lead | | HQ | Internal | |
| HW-003 | Office access control | Door entry system | IT Lead | | HQ | Internal | |
| | | | | | | | |

## Supporting Assets — Network & Cloud Infrastructure

| ID | Asset | Description | Owner | Account/ID | Region | Classification | Notes |
|----|-------|-------------|-------|-----------|--------|---------------|-------|
| NET-001 | AWS Production Account | Primary cloud account | DevOps Lead | [Account ID] | eu-west-1 | Confidential | MFA enforced on all users |
| NET-002 | AWS Dev Account | Development environment | Engineering Lead | [Account ID] | eu-west-1 | Internal | No production data |
| NET-003 | Azure Subscription | Entra ID + Defender services | IT Lead | [Sub ID] | EU | Internal | Used for identity |
| NET-004 | Production VPC | Private cloud network | DevOps Lead | vpc-xxxxx | eu-west-1 | Confidential | |
| NET-005 | GitHub Organization | Source code + CI/CD | Engineering Lead | [org name] | Global (SaaS) | Confidential | |
| | | | | | | | |

## Supporting Assets — People

| ID | Category | Description | Count | Owner |
|----|---------|-------------|-------|-------|
| PPL-001 | Full-time employees | Access to all in-scope systems | [N] | HR Manager |
| PPL-002 | Contractors | Limited system access; under NDA | [N] | [Manager] |
| PPL-003 | Privileged users | Admin-level access | [N] | ISMS Owner |

---

## Asset Register Maintenance

| Activity | Frequency | Owner |
|----------|-----------|-------|
| Full asset register review | Annual | IT Lead |
| Add new asset | On provisioning | IT Lead / Dept Lead |
| Update classification | When data type changes | Asset owner |
| Remove decommissioned assets | On decommission | IT Lead |
| Report to management review | Annual | ISMS Owner |

---

## Change Log

| Date | Version | Changed By | Change |
|------|---------|-----------|--------|
| | 1.0 | | Initial creation |
