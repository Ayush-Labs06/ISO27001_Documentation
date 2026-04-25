# Access Control Policy
**ISO 27001:2022 — A.5.15, A.5.16, A.5.17, A.5.18, A.8.2, A.8.3, A.8.5 | Version 1.0**
**Owner:** IT Lead / ISMS Owner
**Approved by:** [CTO/CEO] | [Date]

---

## 1. Purpose

To ensure information and systems are accessed only by authorized individuals, on the basis of legitimate business need, using the principle of least privilege.

---

## 2. Access Control Principles

| Principle | Definition |
|-----------|-----------|
| **Least privilege** | Users receive the minimum access required to perform their job function — nothing more |
| **Need-to-know** | Access to sensitive information only if the role requires it |
| **Individual accounts** | No shared accounts. Every person has a unique identity |
| **Default deny** | Access is denied unless explicitly granted |
| **Separation of duties** | Critical actions require more than one person where feasible |
| **Time-limited access** | Temporary or elevated access is time-bounded |

---

## 3. Identity and Access Management

### 3.1 Identity Provider

[Organization Name] uses **[Azure Entra ID / Okta / Google Workspace]** as the central Identity Provider (IdP). SSO is the preferred authentication mechanism for all business applications.

### 3.2 Account Types

| Type | Definition | Controls |
|------|-----------|---------|
| Standard user | Day-to-day business access | SSO + MFA; least privilege |
| Developer | Code, cloud dev access | SSO + MFA; no prod admin by default |
| Privileged / admin | Admin access to production systems | SSO + MFA + JIT access; PAM controls; reviewed monthly |
| Service account | Machine-to-machine; no human login | No console access; scoped to minimum permissions; rotated credentials |
| Contractor | External party with time-limited access | SSO + MFA; access expires at contract end |
| Guest | Temporary access for specific purpose | Minimum access; time-limited; sponsor required |

### 3.3 MFA Requirements

MFA is mandatory for:
- [ ] All SSO / IdP authentication
- [ ] All cloud provider consoles (AWS, Azure)
- [ ] Source code management (GitHub)
- [ ] Email (Google Workspace / Microsoft 365)
- [ ] VPN access
- [ ] Any admin or privileged access

Acceptable MFA methods (in order of preference):
1. Hardware security key (FIDO2/WebAuthn — YubiKey, etc.)
2. Authenticator app (TOTP — Google Authenticator, Authy, Microsoft Authenticator)
3. Push notification (Okta/Duo push) — acceptable; phishing-resistant preferred
4. SMS OTP — **not acceptable for privileged accounts**; deprecated

---

## 4. Access Request and Provisioning

### 4.1 New Access Request

1. Employee/manager submits access request to IT Lead via [ticketing system / email]
2. Request must specify: system, access level, business justification, duration (if temporary)
3. IT Lead validates request against policy and approves or escalates to ISMS Owner
4. Access provisioned within [2] business days of approval
5. Access grant logged in access review record

### 4.2 Provisioning Standards

- All accounts created in IdP first; SSO then federated to downstream apps (SCIM preferred)
- Role-based access: users assigned to groups/roles, not granted individual permissions
- Cloud access: use IAM roles, not IAM users with long-lived credentials
- Production access: requires separate approval from CTO/Tech Lead; time-limited by default

---

## 5. Access Review (A.5.18)

| Review Type | Frequency | Scope | Owner |
|------------|-----------|-------|-------|
| Full access review | Quarterly | All users across all systems | IT Lead |
| Privileged access review | Monthly | Admin/root accounts; cloud consoles | IT Lead |
| Contractor access review | On contract renewal or quarterly | All contractors | Manager + IT Lead |
| Dormant account review | Monthly | Accounts inactive >30 days | IT Lead |

**Access review process:**
1. IT Lead exports user access list from IdP and key systems
2. Each asset/system owner confirms: "Does [name] still need [access level]?"
3. Revoke confirmed unnecessary access within 5 business days
4. Document review completion in access review log

---

## 6. Access Revocation

| Trigger | Revocation Timeline |
|---------|-------------------|
| Employee termination | Within 24 hours of last working day |
| Role change (lateral) | Within 5 business days; old access removed, new access granted |
| Contractor end of engagement | On last day; IT Lead informed at least 3 business days in advance |
| Disciplinary suspension | Immediate — IT Lead notified by HR |
| Suspected account compromise | Immediate — IT Lead suspends account; incident raised |

> **IRL Note:** SSO revocation (IdP account disabled) cascades to all federated apps immediately. But some apps with local accounts (not SSO-federated) require manual revocation. Maintain a list of non-SSO-federated systems and check these during offboarding.

---

## 7. Privileged Access Management (A.8.2)

Full PAM procedure: see `05_Access_Control_Identity/privileged_access_management.md`

Summary requirements:
- No standing admin access to production for day-to-day work
- Production access via: VPN + bastion / AWS SSM Session Manager / break-glass account
- All privileged sessions logged (CloudTrail, SSM Session logs)
- Break-glass account usage triggers immediate security review
- Admin credentials stored in approved password manager / secrets manager; not in personal vaults

---

## 8. Service Account Management

- Service accounts created with minimum necessary permissions (scoped IAM roles, not AdministratorAccess)
- No interactive login; no console access
- API keys and credentials rotated at minimum annually; immediately on suspected compromise
- Service account credentials stored in Secrets Manager / Vault (not in environment variables or code)
- Each service account has a named owner (person responsible for it)
- Unused service accounts disabled within 30 days

---

## 9. Authentication Standards (A.8.5)

| System | Minimum Auth Standard |
|--------|-----------------------|
| IdP (master identity) | Strong password + FIDO2/TOTP MFA |
| AWS Console | IAM role via SSO + MFA |
| AWS API | IAM role with OIDC (CI/CD) or temporary credentials; no long-lived access keys |
| Azure | Entra ID + Conditional Access + MFA |
| GitHub | SSO + MFA enforced at org level |
| Production SSH/SSM | SSM Session Manager preferred; key-based SSH if needed; no password SSH |
| VPN | Certificate + MFA |
| SaaS tools | SSO where available; MFA required |

**Password policy (where passwords are required):**
- Minimum length: 12 characters (16+ for privileged accounts)
- Complexity: no requirement to use symbols — length is the priority
- No password expiry for non-privileged accounts (NCSC guidance) — unless breached
- Privileged accounts: reviewed and rotated every 90 days
- Password manager mandatory for all staff

---

## 10. Remote Access (A.6.7)

- Corporate resources accessible remotely via: [VPN / Zero-trust / ZTNA solution]
- VPN requires: MFA + device compliance check (MDM enrolled)
- SSH access to servers via SSM Session Manager preferred; bastion host if required
- No direct RDP or SSH exposure on public internet

---

## Policy Compliance

Non-compliance with this policy must be reported. Violations may result in immediate access revocation pending investigation.

| Field | Detail |
|-------|--------|
| Document ID | ISMS-AC-001 |
| Owner | IT Lead |
| Review cycle | Annual |
| Next review | [Date] |
