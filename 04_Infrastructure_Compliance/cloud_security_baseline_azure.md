# Azure Security Baseline — Entra ID & Defender
**ISO 27001:2022 — A.5.15, A.5.16, A.8.2, A.8.5, A.8.7, A.8.16 | Version 1.0**
**Owner:** IT Lead
**Scope:** Azure Entra ID (identity) + Microsoft Defender for Cloud + Microsoft 365/Google Workspace endpoints

---

## 1. Entra ID — Identity Security

### Authentication & MFA
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 1.1 | MFA enforced for all users (Conditional Access policy, not per-user MFA) | | CA policy list | A.8.5 |
| 1.2 | Microsoft Authenticator push or FIDO2 key as MFA method (not SMS) | | Authentication methods | A.8.5 |
| 1.3 | Security defaults disabled (using Conditional Access instead) | | Entra ID Properties | A.8.5 |
| 1.4 | Legacy authentication protocols blocked (SMTP AUTH, basic auth) | | CA policy: block legacy auth | A.8.5 |
| 1.5 | Sign-in risk policy: require MFA or block for medium/high risk | | Identity Protection | A.8.5 |
| 1.6 | User risk policy: require password change for high-risk users | | Identity Protection | A.8.5 |
| 1.7 | Password protection enabled (banned passwords list) | | Entra ID Password Protection | A.5.17 |
| 1.8 | Self-service password reset (SSPR) requires MFA verification | | SSPR settings | A.5.17 |

```powershell
# Check Conditional Access policies
Get-MgIdentityConditionalAccessPolicy | Select-Object DisplayName, State, Conditions, GrantControls

# Check legacy authentication sign-ins (should be 0)
Get-MgAuditLogSignIn -Filter "clientAppUsed ne 'Browser' and clientAppUsed ne 'Mobile Apps and Desktop clients'" `
  -Top 20 | Select-Object UserDisplayName, ClientAppUsed, CreatedDateTime

# Block legacy authentication - create CA policy
# (use Entra ID portal: Security > Conditional Access > New Policy)
# Conditions: Client apps = Exchange ActiveSync clients + Other clients
# Grant: Block access
```

### Conditional Access Policies (Required Set)
| Policy | Condition | Action | Status |
|--------|-----------|--------|--------|
| Require MFA for all users | All users, all apps | Require MFA | |
| Block legacy authentication | Legacy auth clients | Block | |
| Require compliant device for sensitive apps | All users, sensitive apps | Require compliant device | |
| Block sign-in from risky locations | High-risk locations / anonymous IPs | Block | |
| Require MFA for Azure management | Azure Management app | Require MFA | |
| Block guest access to sensitive data | Guests, restricted SharePoint sites | Block | |

### Privileged Identity Management (PIM)
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 2.1 | PIM enabled for all Global Administrator, Application Administrator roles | | PIM > Roles | A.8.2 |
| 2.2 | Global Admins ≤ 5 accounts; all cloud-only (no hybrid) | | Entra ID > Roles | A.8.2 |
| 2.3 | Privileged roles require activation (just-in-time) | | PIM role settings | A.8.2 |
| 2.4 | PIM activation requires MFA + justification | | PIM settings | A.8.2 |
| 2.5 | PIM access reviews scheduled quarterly | | PIM > Access Reviews | A.5.18 |
| 2.6 | Emergency access (break-glass) accounts exist — cloud-only, long random password, excluded from CA | | | A.8.2 |
| 2.7 | Break-glass accounts have monitoring alert on use | | Sentinel / Log Analytics alert | A.8.16 |

```powershell
# List all Global Admins
Get-MgDirectoryRoleMember -DirectoryRoleId (Get-MgDirectoryRole -Filter "DisplayName eq 'Global Administrator'").Id `
  | Select-Object -ExpandProperty AdditionalProperties | Select-Object displayName, userPrincipalName

# Check PIM role settings
Get-MgIdentityGovernancePrivilegedAccessGroupEligibilitySchedule
```

### Guest and External Access
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 3.1 | Guest access restricted to specific domains (allowlist) | | External Collaboration Settings | A.5.19 |
| 3.2 | Guest accounts reviewed quarterly | | Access Reviews | A.5.18 |
| 3.3 | Guests cannot invite other guests | | External Collaboration Settings | A.5.19 |

---

## 2. Microsoft Defender for Cloud

### Defender for Cloud Setup
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 4.1 | Defender for Cloud enabled on all subscriptions | | Defender for Cloud > Overview | A.8.16 |
| 4.2 | Secure Score reviewed monthly; target ≥ 70% | | Secure Score dashboard | A.8.16 |
| 4.3 | Defender for Servers Plan 2 enabled (if VMs in use) | | | A.8.7, A.8.8 |
| 4.4 | Defender for Containers enabled (if AKS in use) | | | A.8.8 |
| 4.5 | Defender for Storage enabled | | | A.8.3 |
| 4.6 | Defender for Key Vault enabled | | | A.8.24 |
| 4.7 | Email alerts configured for high-severity alerts | | Alert notifications | A.8.16 |
| 4.8 | Microsoft Sentinel connected to Defender for Cloud | | Sentinel data connectors | A.8.16 |

```powershell
# Check Defender plans for a subscription
Get-AzSecurityPricing | Select-Object Name, PricingTier

# Get Secure Score
Get-AzSecuritySecureScore | Select-Object DisplayName, Score, MaxScore, Percentage

# List high severity recommendations
Get-AzSecurityTask | Where-Object {$_.State -eq 'Active'} | Select-Object RecommendationType, State, ResourceId
```

### Security Center Policies
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 5.1 | Azure Security Benchmark policy initiative assigned | | Policy > Assignments | A.5.36 |
| 5.2 | Audit logs enabled for all resource types | | Diagnostic settings | A.8.15 |
| 5.3 | Activity Log retention ≥ 12 months | | Monitor > Activity Log | A.8.15 |
| 5.4 | Diagnostic settings configured for Key Vault, NSGs, Storage | | Diagnostic settings per resource | A.8.15 |

```bash
# Azure CLI: Check activity log retention
az monitor log-profiles list --query '[].{Name:name,Retention:retentionPolicy.days}'

# Check diagnostic settings on a resource
az monitor diagnostic-settings list --resource [resource-id]
```

---

## 3. Endpoint Security (Microsoft Defender for Endpoint / Intune)

### Device Compliance
| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 6.1 | All corporate devices enrolled in Intune | | Intune > Devices | A.8.1 |
| 6.2 | Compliance policy requires: encrypted, OS up-to-date, EDR running | | Intune > Compliance Policies | A.8.1 |
| 6.3 | Non-compliant devices blocked from corporate resources (CA policy) | | CA policy: require compliant device | A.8.1 |
| 6.4 | Defender for Endpoint deployed on all Windows/macOS | | Intune > Security Baselines | A.8.7 |
| 6.5 | BitLocker enforced on Windows; FileVault on macOS | | Intune > Endpoint Security > Disk Encryption | A.8.24 |
| 6.6 | Windows security baselines applied via Intune | | Intune > Security Baselines | A.8.9 |
| 6.7 | Screen auto-lock: ≤ 5 minutes | | Intune > Device Configuration | A.7.7 |
| 6.8 | Remote wipe capability enabled for all enrolled devices | | Intune device action | A.7.9 |

```powershell
# Export device compliance status
Get-MgDeviceManagementManagedDevice | Select-Object DeviceName, ComplianceState, LastSyncDateTime, OperatingSystem
```

---

## 4. Microsoft 365 / Email Security

| # | Control | Status | Evidence | Annex A |
|---|---------|--------|----------|---------|
| 7.1 | SPF, DKIM, DMARC configured for all sending domains | | DNS records; DMARC policy | A.8.20 |
| 7.2 | Defender for Office 365 Safe Links + Safe Attachments enabled | | M365 Security Center | A.8.7 |
| 7.3 | Anti-phishing policy: impersonation protection for CEO/CTO/ISMS Owner | | Defender for Office > Anti-phishing | A.8.7 |
| 7.4 | Mailbox audit logging enabled | | Admin Center > Compliance | A.8.15 |
| 7.5 | DLP policy for PII patterns (credit cards, SSN, email addresses) | | Purview > DLP | A.8.12 |
| 7.6 | External email warning banner enabled | | Mail flow rules | A.8.7 |

```powershell
# Check DMARC for domain
Resolve-DnsName -Type TXT -Name "_dmarc.yourdomain.com"

# Enable mailbox auditing for all users
Get-Mailbox -ResultSize Unlimited | Set-Mailbox -AuditEnabled $true -AuditOwner MailboxLogin,HardDelete,SoftDelete
```

---

## Baseline Score Summary

| Domain | Controls | Passing | % |
|--------|---------|---------|---|
| Entra ID — Auth & MFA | 8 | | |
| Entra ID — PIM | 7 | | |
| Defender for Cloud | 8 | | |
| Azure Security Policies | 4 | | |
| Endpoint Security | 8 | | |
| Email Security | 6 | | |
| **Total** | **41** | | |
