# Quarterly Access Review
**ISO 27001:2022 — A.5.18 | Template — Complete each quarter**

**Review Period:** Q[X] [Year] ([Month] [Year] – [Month] [Year])
**Review Date:** YYYY-MM-DD
**Reviewer:** [IT Lead Name]
**ISMS Owner Sign-off:** [Name] | [Date]

---

## Review Process

1. Export user lists from IdP and each major system
2. Cross-reference against HR active employee list
3. Ask each manager to confirm their team's access is still appropriate
4. Revoke access where no longer needed (within 5 business days)
5. Document all decisions (retain, modify, revoke) in this log
6. Sign off and file in ISMS document store

---

## Section 1 — Identity Provider (Entra ID / Okta)

| User | Role/Title | Groups Assigned | Last Login | Review Decision | Action Taken | Manager Confirmed |
|------|-----------|----------------|-----------|----------------|-------------|-----------------|
| | | | | ✅ Retain / ⚠️ Modify / ❌ Revoke | | Y/N |
| | | | | | | |

**Dormant accounts (no login > 30 days):**

| User | Last Login | Account Status | Decision |
|------|-----------|---------------|---------|
| | | Active / Disabled | Disable if inactive > 60 days |

---

## Section 2 — AWS Production Account

Export from IAM credential report (`aws iam get-credential-report`):

| IAM Entity | Type | Last Console | Last API | MFA | Access Keys Active | Decision |
|-----------|------|-------------|---------|-----|-------------------|---------|
| | User/Role | | | Y/N | Y/N | Retain / Modify / Remove |

**Privileged roles review (AdministratorAccess / PowerUser):**

| Role Name | Principals (who can assume) | Last assumed | JIT? | Decision |
|-----------|---------------------------|-------------|------|---------|
| | | | Y/N | |

---

## Section 3 — GitHub Organization

Export from: GitHub Org → People + Teams

| User | Role | Teams | Last Activity | Decision |
|------|------|-------|---------------|---------|
| | Owner / Member | | | |

Outside collaborators (contractors / external contributors):

| Collaborator | Repo Access | Granted Date | Contract End | Decision |
|-------------|-------------|-------------|-------------|---------|
| | | | | |

---

## Section 4 — Production Database Access

| User/Role | Access Level | Last Access | Granted For | Decision |
|-----------|-------------|------------|-------------|---------|
| | SELECT / ALL PRIVILEGES | | | |

---

## Section 5 — Critical SaaS Tools

| Tool | Users Reviewed | Changes Made |
|------|---------------|-------------|
| Slack | | |
| Google Workspace | | |
| Zendesk | | |
| Stripe | | |
| Notion / Confluence | | |
| DataDog / Grafana | | |
| [Other] | | |

---

## Section 6 — Access Changes This Quarter

| User | System | Old Access | New Access | Reason | Date Applied |
|------|--------|-----------|-----------|--------|-------------|
| | | | | Promotion / Role change / Left | |
| | | | | | |

---

## Section 7 — Exceptions to Least-Privilege

Any access that exceeds the standard access for the role must be documented:

| User | System | Excess Access | Business Justification | Approved By | Expiry Date |
|------|--------|--------------|----------------------|------------|------------|
| | | | | | |

---

## Section 8 — Findings and Actions

| Finding | Priority | Owner | Due Date | Status |
|---------|---------|-------|---------|--------|
| | H/M/L | | | Open / Closed |

---

## Metrics

| Metric | Count |
|--------|-------|
| Total active users reviewed | |
| Dormant accounts disabled | |
| Access revocations | |
| Access modifications | |
| Exceptions documented | |
| New findings raised | |

---

## Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| IT Lead (reviewer) | | | |
| ISMS Owner | | | |

*Retain this record for 3 years. File in ISMS document store: `Records/Access Reviews/Q[X]-[Year].md`*
