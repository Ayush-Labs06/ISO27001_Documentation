# New Employee Security Onboarding
**ISO 27001:2022 — A.6.1, A.6.2, A.6.3 | Version 1.0**
**Owner:** HR + IT Lead + ISMS Owner
**Applies to:** All new employees and long-term contractors (>1 month)

---

## Before Day 1 (HR + IT Lead)

- [ ] Create account in IdP (Entra ID / Okta) with temporary password requiring reset on first login
- [ ] Assign to correct role-based access group (see access control policy — principle of least privilege)
- [ ] Provision device (laptop) with:
  - [ ] MDM enrolled (Intune / Jamf)
  - [ ] Full disk encryption enabled (BitLocker / FileVault)
  - [ ] EDR agent installed (CrowdStrike / Defender)
  - [ ] Screen lock policy enforced (5 min timeout)
  - [ ] OS updated to latest stable version
- [ ] Send welcome email with Day 1 instructions: SSO login, Slack invite, IT support contact
- [ ] Assign training in LMS before Day 1 starts

---

## Day 1 — First 2 Hours (IT Lead walkthrough)

### Account Setup

- [ ] Log in to SSO / IdP for the first time — reset password immediately
  - Password requirements: ≥16 characters, passphrase encouraged, no reuse
  - Set up 1Password (or approved password manager) — provide licence
- [ ] Enrol MFA: authenticator app (Microsoft Authenticator / Authy) — **not SMS**
  - App-based TOTP is minimum; FIDO2/Passkey preferred if supported
- [ ] Verify SSO access to all assigned tools (Slack, GitHub, Google Workspace, etc.)
- [ ] Confirm you cannot access systems you should not have access to (spot-check)
- [ ] Configure company email signature (templates in Notion)

### Device Verification

- [ ] Confirm MDM is enrolled — IT Lead to verify from console
- [ ] Screen lock working (lock screen manually; verify 5 min auto-lock)
- [ ] Verify disk encryption active:
  - macOS: `fdesetup status` → "FileVault is On."
  - Windows: `manage-bde -status C:` → Protection Status: Protection On
- [ ] VPN configured and tested (connect + confirm IP changes)
- [ ] Do NOT use personal USB drives, personal cloud storage (Dropbox, personal Google Drive), or personal email for company data — ever

### Communication Channels

- [ ] Slack: added to relevant channels including `#security-incidents` and `#announcements`
- [ ] Email: configured in approved email client only (not personal Gmail)
- [ ] PagerDuty (if on-call rotation): enrolled and test alert received

---

## Day 1 — Security Policy Read-Through (with ISMS Owner or HR, 30 min)

Walk through these policies verbally. Employee reads and signs acknowledgement at end.

### 1. Acceptable Use Policy (AUP)
Key points to cover:
- Company devices are for work use; limited personal use permitted but monitored
- No installing unlicensed software or circumventing security controls (VPN, EDR, MDM)
- No accessing inappropriate, illegal, or offensive content on company networks or devices
- Remote work: same rules apply at home/coffee shop as in office

### 2. Information Security Policy
Key points:
- Security is everyone's responsibility — not just IT's
- Report incidents immediately — no shame, no blame for reporting; blame for not reporting
- Contact: `security@[company.com]` or Slack `#security-incidents`

### 3. Data Classification
Key points:
- Restricted and Confidential data stays in approved company systems — no personal storage
- Customer PII is Restricted — treat it accordingly
- If unsure about classification: ask before sharing

### 4. Password and Authentication
Key points:
- Use the password manager — no reusing passwords, no sticky notes
- MFA is mandatory — no exceptions
- Never share credentials — not even with IT (we'll never ask for your password)

### 5. Incident Reporting
Key points:
- If you click a phishing link: report it immediately, don't try to fix it yourself
- If you lose a device: report within 1 hour so we can remote wipe
- If you see something unusual: report it — false positives are welcome

### Policy Acknowledgement

> I, [Employee Name], confirm that I have read and understood the following policies:
> - Acceptable Use Policy
> - Information Security Policy
> - Data Classification Policy
> - Password and Authentication Policy
> - Incident Reporting Procedure
>
> I agree to comply with these policies as a condition of my employment/engagement.
>
> **Signature:** ___________________ **Date:** ___________________
> **Full Name:** ___________________ **Role:** ___________________

*(Retain signed copy in personnel file and ISMS evidence store)*

---

## Day 1 — Mandatory Training Assignments

Assign in LMS on Day 1. Must be completed within first 5 business days:

- [ ] Module 1: Information Security Fundamentals (30 min)
- [ ] Module 2: Phishing and Social Engineering (20 min)
- [ ] Module 3: Data Protection and GDPR (20 min)
- [ ] Module 4: Secure Development Practices (if engineering role) (45 min)

**LMS link:** [URL] | **Login:** Use SSO credentials

---

## First Week Checklist (Employee self-completes)

- [ ] Password manager set up with at least 3 accounts stored
- [ ] MFA enrolled on all mandatory systems (see `mfa_enforcement_checklist.md`)
- [ ] GitHub 2FA enabled (mandatory — GitHub will enforce if org policy is set)
- [ ] Completed all mandatory training modules with ≥80% pass mark
- [ ] Know how to report a security incident (Slack `#security-incidents` or `security@[company.com]`)
- [ ] Know who the ISMS Owner is and how to contact them
- [ ] No company data stored on personal devices or personal cloud accounts

---

## Role-Specific Supplements

### Engineering / DevOps

- [ ] Added to GitHub organization with correct team permissions
- [ ] Signed [GitHub username] — recorded in access tracker
- [ ] Pre-commit hooks installed (gitleaks, detect-secrets) — confirm via `pre-commit run --all-files`
- [ ] AWS access: SSO account assigned (no long-lived IAM keys issued unless exceptional case)
- [ ] AWS CLI configured with SSO: `aws configure sso`
- [ ] Kubernetes access configured (if applicable): `kubectl` context set; RBAC role confirmed
- [ ] Understood: never commit secrets to git — use Secrets Manager / environment variables
- [ ] Understood: production access is break-glass — request via SSM Session Manager, not SSH key

### Finance / Accounts

- [ ] Bank transfer requests: always verify by phone call to known number — no email-only approval
- [ ] Wire transfer process: requires 2-person authorization — no exceptions
- [ ] Phishing focus: BEC (Business Email Compromise) — CFO impersonation is a top attack vector
- [ ] Access to financial systems: reviewed and role-scoped; no admin unless required

### HR / People

- [ ] Access to employee data is Restricted — treat accordingly
- [ ] No sharing employee personal data via unencrypted email or personal devices
- [ ] Offboarding access revocation: HR triggers IT access termination same day

---

## IT Lead Sign-Off

| Item | Confirmed By | Date |
|------|-------------|------|
| Account created and MFA enrolled | IT Lead | |
| Device issued, MDM enrolled, FDE verified | IT Lead | |
| Correct access groups assigned | IT Lead | |
| Training assigned in LMS | ISMS Owner | |
| Policy acknowledgement signed | HR | |
| Onboarding complete | IT Lead | |

**Employee name:** ___________________ **Start date:** ___________________ **Role:** ___________________
