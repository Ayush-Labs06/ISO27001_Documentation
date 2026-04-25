# Information Security Incident Response Policy
**ISO 27001:2022 — A.5.24, A.5.25, A.5.26, A.5.27, A.5.28 | Version 1.0**
**Owner:** ISMS Owner
**Approved by:** [CEO] | [Date]

---

## 1. Purpose

To ensure information security incidents are detected, reported, assessed, and responded to in a timely, consistent, and effective manner — minimizing harm, preserving evidence, and enabling learning.

---

## 2. What Is an Incident?

An **information security event** is any observable occurrence that might indicate a breach of security, policy violation, or system failure.

An **information security incident** is an event (or series of events) that has resulted in, or is likely to result in:
- Unauthorized access to, or disclosure of, information
- Loss of availability of a critical system or service
- Loss of integrity of information
- A violation of this or any ISMS policy

### Examples

| Incident Type | Examples |
|--------------|---------|
| Unauthorized access | Login from unusual location, account compromise, ex-employee access |
| Data breach / disclosure | PII sent to wrong recipient, S3 bucket made public, data scraped |
| Malware / ransomware | Endpoint infected, cryptolocker detected |
| Account compromise | Phishing click leading to credential theft |
| Service disruption | DDoS, critical outage, ransomware encryption |
| Insider threat | Data exfiltration by employee, sabotage |
| Physical security | Lost/stolen device, unauthorized office access |
| Supply chain | Supplier breach affecting our data |

---

## 3. Roles and Responsibilities

| Role | Responsibilities |
|------|----------------|
| **Incident Owner** | ISMS Owner (CTO/CISO); overall accountability for response |
| **Incident Coordinator** | Leads the response; coordinates team; communicates status |
| **Technical Lead** | Executes containment, investigation, remediation |
| **HR** | Involved if insider threat; personnel actions |
| **Legal / DPO** | Breach notification decisions; regulatory engagement |
| **Communications Lead** | External communications (customers, press) — CEO-approved |
| **All Staff** | Recognize and report events promptly |

---

## 4. Incident Classification

See `07_Incident_BCP/incident_classification_matrix.md` for full severity matrix.

| Severity | Description | Initial Response Time |
|----------|-------------|----------------------|
| P1 — Critical | Data breach with customer PII; ransomware; complete service loss | 30 minutes |
| P2 — High | Suspected compromise; significant system degradation; active attack | 2 hours |
| P3 — Medium | Policy violation; suspicious activity; minor data exposure | 8 hours |
| P4 — Low | Security events with no confirmed impact; user-reported phishing (pre-click) | 24 hours |

---

## 5. Incident Response Process

### Phase 1 — Identify and Report
- Any person who detects or suspects an incident reports to: **`security@[company.com]`** or via Slack **`#security-incidents`**
- Include: what you observed, when, affected systems, any actions already taken
- Do not investigate on your own — report first

### Phase 2 — Triage and Classify
- ISMS Owner / Incident Coordinator reviews the report within response time
- Classify severity (P1–P4)
- Assemble response team (size based on severity)
- Open incident log (Jira / Notion / dedicated incident template)

### Phase 3 — Contain
- Immediate containment actions to prevent further damage:
  - Isolate affected systems (AWS Security Group, network isolation, EC2 stop)
  - Suspend compromised accounts (IdP disable)
  - Block malicious IPs at WAF/firewall
  - Revoke compromised credentials immediately
- **Do not take any action that destroys evidence** before forensic preservation

### Phase 4 — Investigate
- Preserve evidence: CloudTrail logs, application logs, memory dumps, disk images (if needed)
- Establish timeline: what happened, when, how
- Identify: root cause, attacker's entry point, what data was accessed/exfiltrated
- Document findings in the incident log

### Phase 5 — Notify
- **Internal:** Notify CEO within 1 hour of P1/P2 incidents
- **Regulatory (GDPR):** If personal data breach — notify supervisory authority within 72 hours of becoming aware (Article 33); notify affected individuals if high risk (Article 34)
- **Customers:** If their data is affected — notification per DPA/contract obligations
- **Suppliers:** If supplier-related — notify per supplier agreement

### Phase 6 — Remediate and Recover
- Remove malware, patch vulnerabilities, reset credentials, fix misconfiguration
- Restore systems from clean backup (with restore verification)
- Confirm clean state before returning to production

### Phase 7 — Post-Incident Review (A.5.27)
- Post-incident review within 5 business days of resolution
- Document: root cause, timeline, response effectiveness, lessons learned
- Update: risk register, controls, procedures, runbooks as needed
- Share key learnings with engineering team; update threat model

---

## 6. Evidence Preservation (A.5.28)

To preserve evidence for potential legal or regulatory proceedings:
- Take timestamped screenshots before any changes
- Export and archive logs before systems are cleaned or shut down
- Use S3 Object Lock for preserved evidence (COMPLIANCE mode if legal proceedings expected)
- Maintain strict chain of custody: who accessed evidence, when, why
- Do not delete or overwrite anything until Legal/ISMS Owner authorizes

AWS log preservation:
```bash
# Create evidence bucket with Object Lock
aws s3api create-bucket --bucket incident-evidence-[incident-id] --region eu-west-1
aws s3api put-object-lock-configuration --bucket incident-evidence-[incident-id] \
  --object-lock-configuration '{"ObjectLockEnabled":"Enabled","Rule":{"DefaultRetention":{"Mode":"COMPLIANCE","Days":365}}}'

# Copy CloudTrail logs to evidence bucket
aws s3 sync s3://[cloudtrail-bucket]/ s3://incident-evidence-[incident-id]/cloudtrail/
```

---

## 7. GDPR Breach Notification (72-Hour Clock)

| Step | Action | Timeline |
|------|--------|---------|
| Breach discovered | Clock starts | T=0 |
| Triage / initial assessment | Determine if personal data involved; estimate scope | T+2h |
| Legal / DPO engaged | Assess notification obligation | T+4h |
| Supervisory authority notified | ICO/DPA notification (if required) | T+72h max |
| Affected individuals notified | If high risk to rights and freedoms | ASAP after authority notification |

**Notification not required if:**
- No personal data was involved
- Data was encrypted and keys are uncompromised (rendering data unintelligible)
- Breach is unlikely to result in risk to individuals' rights and freedoms

---

## 8. Contact List

| Role | Name | Contact |
|------|------|---------|
| Incident Owner (ISMS) | [CTO name] | [phone/email] |
| Backup Incident Owner | [name] | [phone/email] |
| Legal Counsel / DPO | [name / external firm] | [phone/email] |
| CEO | [name] | [phone/email] |
| Cyber insurance broker | [firm] | [phone] — policy # [xxx] |
| Forensics retainer | [firm if any] | [phone] |
| Supervisory authority | [ICO / national DPA] | [reporting URL] |

---

## 9. Incident Log Retention

All incident records retained for minimum 3 years. P1 incidents: permanent record. Retained in [ISMS document store / Jira].

| Field | Detail |
|-------|--------|
| Document ID | ISMS-IRP-001 |
| Owner | ISMS Owner |
| Review cycle | Annual + after any P1/P2 incident |
| Next review | [Date] |
