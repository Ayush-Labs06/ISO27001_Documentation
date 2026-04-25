# Acceptable Use Policy (AUP)
**ISO 27001:2022 — A.5.10, A.6.7 | Version 1.0**
**Owner:** HR Manager / ISMS Owner
**Approved by:** [CEO] | [Date]

---

## 1. Purpose

This policy defines acceptable use of [Organization Name]'s information assets, systems, and technology. It protects both the organization and its employees by establishing clear expectations.

All employees, contractors, and third parties with access to [Organization Name] systems must read, acknowledge, and comply with this policy.

---

## 2. Scope

Applies to all:
- Company-issued devices (laptops, mobile phones, tablets)
- BYOD devices used to access company resources
- Cloud services, SaaS tools, email, and internal systems
- Physical documents and removable media
- Remote working environments (home office, café, hotel, etc.)

---

## 3. Company-Issued Devices

### 3.1 Permitted Use
- Work-related tasks, communication, and collaboration
- Limited personal use that does not interfere with work or compromise security

### 3.2 Prohibited Use
- Accessing illegal content (copyright infringement, CSAM, illegal downloads)
- Installing unauthorized software (only approved software via MDM)
- Disabling security controls (MDM agent, antivirus/EDR, disk encryption)
- Using company devices for personal business or freelance work without approval
- Leaving devices unattended and unlocked in public places

### 3.3 Device Security Requirements
- [ ] Full-disk encryption enabled (FileVault / BitLocker)
- [ ] MDM agent enrolled and active
- [ ] Automatic screen lock: ≤ 5 minutes idle
- [ ] Password/PIN protected (minimum 12 characters or biometric)
- [ ] Operating system and security patches applied within 7 days of critical release

---

## 4. Remote Working (A.6.7)

### 4.1 Home Office Requirements
- Use a private, secure Wi-Fi network (not public café/hotel Wi-Fi without VPN)
- Enable VPN when accessing internal systems or sensitive data from outside the office
- Do not allow family members or visitors to use company devices
- Apply clear screen policy — sensitive information not visible to others
- Lock screen whenever stepping away from device

### 4.2 Public Locations
- Use VPN at all times on public or untrusted networks
- Use a privacy screen filter when working with sensitive information in public
- Do not verbally discuss sensitive customer or business information in public spaces

### 4.3 Travel
- Report international travel involving company devices to IT Lead before departure
- Exercise increased caution in high-risk countries (additional guidance available on request)
- Report immediately if a device is lost or stolen

---

## 5. Email and Communication

- Do not send sensitive or classified information (Confidential / Restricted) via unencrypted email
- Do not open attachments or click links from unexpected or suspicious senders — report phishing to `security@[company.com]`
- Do not use personal email accounts to conduct company business
- Do not use SMS for sharing credentials, tokens, or sensitive business data
- Company email accounts must use MFA at all times

---

## 6. Cloud Services and SaaS

- Only use company-approved SaaS tools for business data (approved list in IT wiki)
- Do not store company data in personal cloud accounts (personal Google Drive, personal Dropbox, iCloud)
- Do not share company accounts or credentials with colleagues — each person has individual accounts
- When a SaaS tool is no longer needed, notify IT to offboard your access

---

## 7. Password and Authentication (A.5.17)

- Use a password manager (company-provided: [e.g., 1Password / Bitwarden])
- Passwords must be: minimum 12 characters, unique per service, not reused from personal accounts
- MFA must be enabled on all accounts where available — no exceptions for company-critical systems
- Never share passwords, tokens, or API keys with anyone — not even IT support (IT will never ask)
- If you suspect a credential is compromised, change it immediately and notify IT

---

## 8. Data Handling (A.5.12, A.5.13, A.5.14)

- Handle information according to its classification (see Data Classification Policy)
- **Restricted** data (customer PII, credentials, keys): stored only in approved encrypted storage; never emailed in clear text; never in Slack DMs
- **Confidential** data: internal systems only; do not share externally without approval
- Do not take copies of customer data to personal devices or unapproved storage
- When in doubt about handling, ask your manager or the ISMS Owner before acting

---

## 9. Source Code and Development

- Never commit credentials, API keys, or secrets to version control — use secrets management (AWS Secrets Manager, HashiCorp Vault)
- Code must go through peer review (PR process) before merging to main/production
- Production access is restricted — do not access production directly without following the privileged access procedure
- Do not use production data in development or testing environments

---

## 10. Physical Security (A.7.7)

- Clear desk: sensitive documents must not be left unattended on desks; lock away at end of day
- Clear screen: lock your screen when away from your desk (Windows: Win+L; Mac: Cmd+Ctrl+Q)
- Shred sensitive printed documents rather than discarding in waste paper
- Visitor access to the office must be logged; visitors must not be left unattended in secure areas

---

## 11. Incident Reporting (A.6.8)

Report the following **immediately** to `security@[company.com]` or via [incident reporting channel]:
- Lost or stolen devices
- Suspected phishing or social engineering attempts
- Accidental disclosure of sensitive information
- Any unexpected access to systems you don't normally use
- Any suspicious activity on your accounts

**There is no blame culture for good-faith reporting. The only wrong thing to do is not report.**

---

## 12. Monitoring

[Organization Name] reserves the right to monitor use of company systems and devices for security purposes. Monitoring may include:
- MDM compliance reporting (device health, encryption status)
- Email security scanning (anti-phishing, DLP)
- Cloud access logs (IdP, AWS CloudTrail, SaaS audit logs)

Monitoring is conducted for security purposes only and is proportionate. Personal communications on personal devices are not monitored.

---

## 13. Acknowledgement

By signing (or completing onboarding confirmation), I confirm that I have read, understood, and agree to comply with this Acceptable Use Policy.

| Name | Role | Date | Signature |
|------|------|------|-----------|
| | | | |
