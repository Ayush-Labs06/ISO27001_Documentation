# MFA Enforcement Checklist
**ISO 27001:2022 — A.8.5 | Version 1.0**
**Owner:** IT Lead

MFA enforcement status across all systems. Run this audit quarterly.

---

## MFA Requirements by Account Type

| Account Type | MFA Required | Acceptable Methods | Notes |
|-------------|-------------|-------------------|-------|
| All employees — IdP | Yes | FIDO2, TOTP app | No SMS |
| Admins / privileged accounts | Yes | FIDO2 only (preferred) | Hardware key required |
| Contractors | Yes | TOTP app minimum | |
| Service accounts | N/A | No MFA — no console access | Compensating: no console access |
| Break-glass accounts | Yes | FIDO2 hardware key | Physical key in safe |

---

## MFA Status Audit — System by System

### Identity Provider (Entra ID / Okta)

| # | Check | Method | Status | Date Checked |
|---|-------|--------|--------|-------------|
| 1 | Conditional Access policy: Require MFA for all users | CA policy active | | |
| 2 | Legacy authentication blocked (no bypass) | CA policy blocks legacy auth | | |
| 3 | All users enrolled in MFA | Entra ID auth methods report | | |
| 4 | Authenticator app (not SMS) as default for all | Auth method report | | |
| 5 | FIDO2 keys issued to privileged users | Key inventory | | |
| 6 | Sign-in risk → MFA challenge configured | Identity Protection | | |

```powershell
# Check users WITHOUT MFA registered
Get-MgUser -All | ForEach-Object {
  $methods = Get-MgUserAuthenticationMethod -UserId $_.Id
  if ($methods.Count -le 1) {  # Only password
    [PSCustomObject]@{
      Name = $_.DisplayName
      UPN = $_.UserPrincipalName
      MFAMethods = $methods.Count
    }
  }
} | Format-Table
```

### AWS IAM

| # | Check | Command | Status |
|---|-------|---------|--------|
| 1 | All IAM users (if any) have MFA | Credential report: `mfa_active=TRUE` | |
| 2 | Root account MFA enabled | `aws iam get-account-summary` → `AccountMFAEnabled=1` | |
| 3 | AWS Identity Center requires MFA | Identity Center settings | |
| 4 | SCP blocks API calls without MFA condition (for critical actions) | SCP audit | |

```bash
# Check MFA status for all IAM users
aws iam generate-credential-report
sleep 5
aws iam get-credential-report --output text --query Content | base64 --decode | \
  awk -F',' 'NR>1 && $8=="false" {print "NO MFA: " $1}' # field 8 = mfa_active
```

### GitHub Organization

| # | Check | Location | Status |
|---|-------|---------|--------|
| 1 | Organization-level 2FA enforcement enabled | Org Settings → Authentication security | |
| 2 | All members have 2FA | Org Settings → People (filter by 2FA) | |
| 3 | SAML SSO enabled for org | Org Settings → Security | |
| 4 | Personal access tokens (PATs): MFA required to create | Org Settings | |

```bash
# Using GitHub CLI — check 2FA status for org members
gh api /orgs/[org-name]/members?filter=2fa_disabled --paginate \
  --jq '.[].login'
# Any output = member without 2FA
```

### Google Workspace / Microsoft 365

| # | Check | Location | Status |
|---|-------|---------|--------|
| 1 | 2-Step Verification enforced for all users | Admin Console → Security | |
| 2 | Enrollment period set to 0 (immediate) | Admin Console | |
| 3 | Less secure app access disabled | Admin Console | |
| 4 | TOTP or Google Prompt; no SMS for admin accounts | User security settings | |

### Critical SaaS Tools

| Tool | MFA Enforced? | Method | How to Enforce | Status |
|------|--------------|--------|----------------|--------|
| GitHub | SSO + Org 2FA policy | TOTP/FIDO2 | Org → Security → Require 2FA | |
| Slack | SSO via IdP | Via Entra ID/Okta CA | Configure SSO | |
| Notion/Confluence | SSO via IdP | Via Entra ID/Okta CA | Configure SSO | |
| Jira/Linear | SSO via IdP | Via Entra ID/Okta CA | Configure SSO | |
| AWS Console | SSO + MFA | IAM + AWS Identity Center | CA policy | |
| Vercel/Netlify | GitHub/Google SSO | Via IdP | Enable SSO | |
| Stripe | Per-account MFA | TOTP | Dashboard → Team → Require MFA | |
| Zendesk | SSO via IdP | Via Entra ID/Okta CA | Configure SSO | |
| DataDog/Grafana | SSO via IdP | Via Entra ID/Okta CA | Configure SSO | |

---

## MFA Exceptions and Compensating Controls

If MFA cannot be enforced for a specific account, document the exception:

| Account | System | Reason MFA Not Feasible | Compensating Control | Approved By | Expiry |
|---------|--------|------------------------|---------------------|------------|--------|
| ci-service-account | GitHub (PAT) | Automation; no interactive login | Scoped to read-only; IP allowlisted; token rotated 30 days | IT Lead | |

---

## MFA Incident Response

If MFA is suspected compromised (e.g., SIM swap, authenticator app compromise):

1. Immediately terminate all active sessions (IdP → Revoke all sessions)
2. Disable MFA methods associated with compromised device
3. Re-enroll MFA with a trusted device (in person if possible)
4. Review sign-in logs for last 30 days for suspicious activity
5. Raise security incident

```powershell
# Revoke all sessions for a user (Entra ID)
Revoke-MgUserSignInSession -UserId [user-id]

# Force MFA re-registration
Update-MgUser -UserId [user-id] -StrongAuthenticationRequirements @()
# User will be prompted to register MFA on next login
```

---

## Quarterly MFA Audit Report

| Metric | Target | Current |
|--------|--------|---------|
| % employees with MFA enrolled | 100% | |
| % privileged accounts with FIDO2 | 100% | |
| Non-SSO tools with MFA enforced | 100% | |
| MFA bypass exceptions | 0 (or documented + time-limited) | |
| IAM users with MFA (if any exist) | 100% | |

*Report to ISMS Owner for management review.*
