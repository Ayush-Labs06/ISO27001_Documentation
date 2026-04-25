# Employee Onboarding & Offboarding Security Procedure
**ISO 27001:2022 — A.6.1, A.6.2, A.6.5, A.5.18 | Version 1.0**
**Owner:** HR Manager + IT Lead

---

## Onboarding Checklist

**Complete before/on first day. Sign off each item.**

### Day -5 to Day -1 (Before First Day)

| # | Task | Owner | Done |
|---|------|-------|------|
| 1 | Create account in IdP (Entra ID / Okta / Google Workspace) | IT Lead | ☐ |
| 2 | Assign to correct groups/roles per job role | IT Lead | ☐ |
| 3 | Configure SSO access for approved SaaS tools | IT Lead | ☐ |
| 4 | Prepare and ship company laptop (MDM-enrolled, encrypted) | IT Lead | ☐ |
| 5 | Background check completed (if required for role) | HR | ☐ |
| 6 | NDA / confidentiality agreement signed | HR | ☐ |
| 7 | Employment contract with IS obligations signed | HR | ☐ |
| 8 | Add to HR system; set offboarding date reminder | HR | ☐ |

### Day 1

| # | Task | Owner | Done |
|---|------|-------|------|
| 9 | IT orientation: device setup, password manager, MFA enrollment | IT Lead | ☐ |
| 10 | Security awareness: present or send AUP; get signed acknowledgement | HR / ISMS Owner | ☐ |
| 11 | IS policy acknowledgement signed and filed | HR | ☐ |
| 12 | Security training assigned (complete within 30 days) | HR | ☐ |
| 13 | Slack / Teams invite; `#security-incidents` channel joined | IT Lead | ☐ |
| 14 | Confirm: MFA enabled on IdP, email, and GitHub | IT Lead | ☐ |
| 15 | Confirm: password manager account set up | IT Lead | ☐ |
| 16 | Production access: NOT granted by default — requires separate approval | IT Lead | ☐ |

### First 30 Days

| # | Task | Owner | Done |
|---|------|-------|------|
| 17 | Security training completed and recorded | HR | ☐ |
| 18 | Role-specific training (developer: secure coding; finance: data handling) | Manager | ☐ |
| 19 | Access review: confirm provisioned access matches job requirements | IT Lead | ☐ |

### Access Provisioning by Role

| Role | Systems | Access Level |
|------|---------|-------------|
| Software Engineer | GitHub (write), AWS Dev account (developer role), Jira, Slack, Google Workspace | No prod admin by default |
| Senior Engineer | + staging environment, code review access | No prod admin by default |
| DevOps/Platform | AWS prod (limited), EKS read, monitoring dashboards | Prod read + JIT admin |
| Engineering Manager | All Engineering systems + HR data view | No prod admin |
| HR | Google Workspace, HR system, Slack | No technical system access |
| Finance | Accounting system, Stripe read, Google Workspace | No technical system access |
| Support | Zendesk, CRM (customer data scoped), Slack | Read-only prod data |
| ISMS Owner / CTO | All systems | Full access (audited) |

---

## Offboarding Checklist

**Critical: Begin this process ON the last working day (or immediately for disciplinary termination).**

### Day of Departure — Within 4 Hours

| # | Task | Owner | Done |
|---|------|-------|------|
| 1 | **DISABLE IdP account** (Entra ID / Okta / Google) — this cascades to all SSO-federated apps | IT Lead | ☐ |
| 2 | Revoke all active SSO sessions (force sign-out) | IT Lead | ☐ |
| 3 | Disable GitHub account from org (or remove from org) | IT Lead | ☐ |
| 4 | Revoke any AWS IAM keys / disable IAM user if any | IT Lead | ☐ |
| 5 | Revoke VPN access | IT Lead | ☐ |
| 6 | Notify manager to change shared account credentials (if any used — document as gap) | IT Lead | ☐ |

### Within 24 Hours

| # | Task | Owner | Done |
|---|------|-------|------|
| 7 | Review list of all systems NOT federated via SSO; revoke manually | IT Lead | ☐ |
| 8 | Remove from all Slack channels and guest shares | IT Lead | ☐ |
| 9 | Transfer device: initiate remote wipe OR collect laptop in person | IT Lead | ☐ |
| 10 | Change passwords for any service accounts the person had access to | IT Lead | ☐ |
| 11 | Archive email / transfer calendar (per HR policy) | IT Lead + HR | ☐ |
| 12 | Retrieve company assets: laptop, access cards, hardware MFA keys | HR + IT Lead | ☐ |

### Within 5 Business Days

| # | Task | Owner | Done |
|---|------|-------|------|
| 13 | Verify non-SSO systems revoked (manual list check) | IT Lead | ☐ |
| 14 | Confirm laptop wiped or in IT possession | IT Lead | ☐ |
| 15 | Update asset register: devices reassigned or decommissioned | IT Lead | ☐ |
| 16 | Update supplier contacts if person was a named supplier contact | ISMS Owner | ☐ |
| 17 | Confirm final payslip includes any outstanding NDA/data return obligations | HR | ☐ |
| 18 | Log offboarding completion in access review log | IT Lead | ☐ |

### Non-SSO Systems — Manual Revocation List

Maintain this list for all systems not federated via SSO (require manual offboarding):

| System | Access Type | Revocation Step | Owner |
|--------|------------|-----------------|-------|
| [Tool not on SSO] | Direct login | Disable account in [tool] admin panel | IT Lead |
| AWS root account | N/A | Not applicable (no personal access) | — |
| Physical office access | Keycard | Deactivate card in access control system | Office Manager |
| | | | |

---

## Handling Involuntary / Emergency Termination

For disciplinary terminations or suspected insider threat:

1. **IT Lead is notified in advance** by HR (at minimum 30 min before employee is informed)
2. **IdP account disabled IMMEDIATELY** — before or at the same moment the employee is informed
3. **Manager is alerted** to monitor for any anomalous access attempts in the last 24-48 hours
4. **Preserve the user's email and file access** (do not delete — needed for investigation)
5. **ISMS Owner reviews** CloudTrail and application logs for the previous 30 days for the user's accounts
6. **If data exfiltration suspected**: raise security incident, preserve evidence, contact legal counsel

---

## Contractor Access Management

| Event | Action | Timeline |
|-------|--------|---------|
| Contractor engagement starts | Same as employee onboarding (scoped) | Day 1 |
| Contract extension | Verify access still appropriate | At renewal |
| Contract end | Same as employee offboarding | On last day |
| No response from contractor manager | IT Lead proactively disables after contract end date | Day 0 |

IT Lead maintains a contractor register with contract end dates and sets calendar reminders 5 days before each end date.

---

## Evidence for Auditors

The auditor will ask about A.6.5 (post-employment) and A.5.18 (access rights). Show:
- Completed offboarding checklists with dates and sign-off
- IdP account status (disabled accounts for departed staff)
- Access review log showing reviews completed quarterly
- Example: "Show me what happened to [departed employee's] accounts"
