# Incident Response Plan (IRP)
**ISO 27001:2022 — A.5.24, A.5.25, A.5.26 | Version 1.0**
**Owner:** ISMS Owner
**Approved by:** [CEO] | [Date]

---

## 1. Purpose and Scope

This plan defines how [Organization Name] detects, responds to, and recovers from information security incidents. It applies to all systems and data within the ISMS scope.

---

## 2. Incident Response Team

| Role | Primary | Backup | Contact |
|------|---------|--------|---------|
| Incident Commander (ISMS Owner) | [CTO Name] | [Senior Eng Name] | [phone] |
| Technical Lead | [DevOps Lead] | [Senior Eng] | [phone] |
| Communications (External) | [CEO] | [CTO] | [phone] |
| HR / People | [HR Manager] | [CEO] | [phone] |
| Legal / DPO | [Legal Counsel / external] | [CEO] | [phone] |
| On-call Engineer | [Rotation via PagerDuty] | — | [PagerDuty] |

**24/7 Security Contact:** security@[company.com] | Slack: `#security-incidents`
**Out-of-hours escalation:** PagerDuty policy "[Security-Critical]"

---

## 3. Incident Severity and Response Times

| Severity | Definition | Initial Response | Examples |
|----------|-----------|-----------------|---------|
| **P1 — Critical** | Active breach; data exfiltration; ransomware; full service outage | **30 minutes** | Ransomware active; PII exfiltrated; root account compromised |
| **P2 — High** | Suspected compromise; significant data exposure; major service degradation | **2 hours** | Suspicious admin access; possible credential compromise; partial outage |
| **P3 — Medium** | Policy violation; minor data exposure; suspicious activity | **8 hours** | AUP violation; unsanctioned data access; phishing click (contained) |
| **P4 — Low** | No current impact; near-miss; awareness event | **24 hours** | Phishing received (not clicked); weak password discovered; failed brute force |

---

## 4. How to Report an Incident

**Any employee:**
1. Email: `security@[company.com]`
2. Slack: `#security-incidents` (tag @isms-owner)
3. Phone: [Incident Commander phone] (P1 only)
4. If you cannot access company systems: [personal mobile of ISMS Owner]

**What to include:**
- What you observed (what, when, where)
- Systems or data involved (if known)
- Any actions already taken
- Your contact details

**Do not:**
- Investigate on your own
- Delete anything — preserve evidence
- Inform others beyond those managing the incident until Communications Lead approves

---

## 5. Incident Response Phases

### Phase 1 — Identify and Triage (0–2 hours)

```
Incident reported
      │
      ▼
Incident Commander alerted
      │
      ▼
Initial triage: Is this a real incident or a false positive?
      │
      ├── False positive → Document in incident log; close
      └── Real incident → Assign severity (P1-P4)
                              │
                              ▼
                    Activate response team (size based on severity)
                              │
                              ▼
                    Open incident log (Jira / Notion ticket)
                              │
                              ▼
                    CEO/exec notified (P1/P2)
```

### Phase 2 — Contain (Immediate Priority)

Stop the bleeding before investigating:

| Action | Method | Owner |
|--------|--------|-------|
| Isolate compromised EC2 instance | Quarantine security group (remove all inbound/outbound except logging) | DevOps |
| Disable compromised user account | IdP (Entra ID / Okta) → Disable user; revoke all sessions | IT Lead |
| Revoke compromised IAM key | `aws iam delete-access-key --access-key-id [key]` | DevOps |
| Block malicious IP | WAF rule + Security Group deny | DevOps |
| Isolate EKS pod | `kubectl delete pod [pod] -n [namespace]` or NetworkPolicy | DevOps |
| Suspend payment processing | Stripe dashboard → Emergency stop | Finance |
| Disable compromised GitHub Actions secret | GitHub Settings → Secrets → Delete | Engineering |

**AWS containment commands:**
```bash
# Quarantine EC2 instance (deny all traffic except SSM)
aws ec2 modify-instance-attribute \
  --instance-id i-[compromised] \
  --groups sg-[quarantine-sg-id]  # pre-created security group with no ingress

# Revoke all active IAM credentials
aws iam delete-access-key \
  --user-name [compromised-user] \
  --access-key-id AKIA[key]

# Disable IAM user login
aws iam update-login-profile \
  --user-name [compromised-user] \
  --password-reset-required  # or delete login profile entirely

# Force IdP session revocation (Entra ID)
# az ad user revoke-sign-in-sessions --id [user-object-id]
```

### Phase 3 — Investigate

**Preserve evidence first:**
```bash
# Export CloudTrail logs for the incident window
aws cloudtrail lookup-events \
  --start-time [incident-start-minus-1h] \
  --end-time [incident-end-plus-1h] \
  --output json > cloudtrail_incident_[date].json

# Copy to evidence bucket (with Object Lock)
aws s3 cp cloudtrail_incident_[date].json \
  s3://incident-evidence-[incident-id]/cloudtrail/ \
  --sse aws:kms

# Get VPC Flow Logs for compromised instance
aws ec2 describe-flow-logs --filter "Name=resource-id,Values=[vpc-id]"
# Download relevant logs from CloudWatch or S3
```

**Investigate:**
1. Build a timeline: first event → last known malicious action
2. Identify: entry point, attacker actions, data accessed/exfiltrated, lateral movement
3. Determine scope: what systems, what data, how many individuals affected
4. CloudWatch Insights queries for log analysis:

```
# Find all API calls from a specific IP in the incident window
fields @timestamp, eventName, sourceIPAddress, userIdentity.arn, errorCode
| filter sourceIPAddress = "x.x.x.x"
| sort @timestamp asc
| limit 1000
```

### Phase 4 — Notify

**Internal notifications:**
- P1/P2: CEO notified within 1 hour
- Board/investors: CEO discretion (usually for P1 data breach)
- All-staff: only if operational impact requires it

**GDPR notification (if personal data breach):**
- 72-hour clock starts when you become "aware" of the breach (i.e., when you have enough information to determine personal data was involved)
- Notification to supervisory authority: [ICO / national DPA] — see contact in Section 2
- Affected individuals: if "likely to result in a high risk to the rights and freedoms of natural persons"

```
GDPR notification template to supervisory authority:
1. Nature of the breach (type, categories of data, approximate number of individuals)
2. Name and contact of DPO
3. Likely consequences of the breach
4. Measures taken or proposed to address the breach, including mitigation measures
```

**Customer notification (if their data affected):**
- Legal counsel to determine obligation and timing
- Communications Lead drafts and CEO approves before sending
- No speculation — only confirmed facts

### Phase 5 — Remediate and Recover

1. Remove malware / malicious persistence (backdoors, scheduled tasks, cron jobs, Lambda functions)
2. Patch exploited vulnerability
3. Reset ALL credentials that may have been exposed (even suspected)
4. Restore from known-clean backup (if data was corrupted/encrypted)
5. Verify clean state (run security scans, check logs for persistence)
6. Gradual traffic restoration (not all at once)
7. Enhanced monitoring for 7-14 days post-incident

### Phase 6 — Post-Incident Review (A.5.27)

**Within 5 business days of resolution:**

1. Review meeting with incident team (30-60 min)
2. Document:
   - Complete timeline
   - Root cause (5-whys or fishbone)
   - What worked in the response
   - What didn't work
   - Lessons learned
3. Update:
   - Risk register (if new risk identified)
   - Control gaps (if a missing control was the root cause)
   - This IRP (if procedure gaps found)
   - Technical runbooks
4. Share summary with engineering team (blameless post-mortem culture)

---

## 6. Incident Communication Templates

### Internal P1 Alert (Slack / Email)
> **SECURITY INCIDENT — P1**
> A potential security incident has been detected affecting [SYSTEM/DATA].
> Incident response is now active.
> All actions on affected systems must be coordinated with [ISMS Owner].
> **Do not discuss this on external channels.**

### GDPR Regulatory Notification (Draft)
> [Organization Name] is notifying the [Supervisory Authority] of a personal data breach in accordance with Article 33 GDPR.
> Date/time incident discovered: [datetime]
> Nature of breach: [description]
> Categories of data: [list]
> Approximate number of data subjects: [number]
> Contact: [DPO / ISMS Owner name, contact]
> Likely consequences: [assessment]
> Measures taken: [containment actions]

---

## 7. Evidence Handling (A.5.28)

- All logs preserved in locked evidence bucket before any remediation
- Chain of custody documented: who accessed evidence, when, why
- Evidence retained for minimum 3 years; longer if legal proceedings expected
- If criminal referral considered: contact legal counsel before any additional investigation steps
