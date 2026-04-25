# Incident Log — [INC-YYYY-NNN]
**Use one copy of this file per incident. File in: `Records/Incidents/INC-[YYYY]-[NNN].md`**

---

## Header

| Field | Value |
|-------|-------|
| Incident ID | INC-[YYYY]-[NNN] |
| Date/Time Opened | YYYY-MM-DD HH:MM UTC |
| Date/Time Closed | YYYY-MM-DD HH:MM UTC |
| Total Resolution Time | [X hours] |
| Severity | P1 / P2 / P3 / P4 |
| Incident Type | Ransomware / Data breach / Unauthorized access / Policy violation / Other |
| Incident Commander | [Name] |
| Technical Lead | [Name] |
| Status | Open / Contained / Remediated / Closed |

---

## 1. Initial Report

**Reported by:** [Name / automated system]
**Reported via:** Email / Slack / Automated alert / Other
**Date/Time of report:** YYYY-MM-DD HH:MM UTC

**Initial description:**
> [What was reported. Copy verbatim if possible.]

---

## 2. Triage Assessment

**Is this a real incident or false positive?** Real / False positive / Under investigation

**Severity assigned:** P[1-4]
**Basis for severity:**
> [Why this severity? What data/systems are affected?]

**Response team assembled:**
- [ ] Incident Commander: [name]
- [ ] Technical Lead: [name]
- [ ] HR: [name] (if insider threat or personnel implications)
- [ ] Legal/DPO: [name] (if personal data involved)
- [ ] CEO: [name] (if P1/P2)

---

## 3. Incident Timeline

| Date/Time (UTC) | Actor | Action / Observation |
|-----------------|-------|---------------------|
| | Attacker / System | Initial event (based on log analysis) |
| | [Name] | Incident detected / reported |
| | [Name] | Incident Commander notified |
| | [Name] | Severity P[X] assigned |
| | [Name] | [Containment action] |
| | [Name] | CEO/Legal notified |
| | [Name] | Root cause identified |
| | [Name] | Remediation complete |
| | [Name] | Service restored |
| | [Name] | Incident closed |

---

## 4. Affected Systems and Data

| System | Impact | Data Affected | Evidence Location |
|--------|--------|--------------|------------------|
| | | | |

**Estimated number of individuals affected:** [number or "not yet determined"]
**Data types involved:** PII / Financial / Health / Source code / Internal / Other

---

## 5. Containment Actions

| Action | Time | Taken By | Outcome |
|--------|------|---------|--------|
| | | | |

---

## 6. Investigation Findings

**Attack vector / entry point:**
> [How did the attacker gain access? Or: what caused the incident?]

**Actions taken by attacker / what happened:**
> [Detailed description of what occurred]

**Data accessed or exfiltrated:**
> [Specific data, volumes, timeframe]

**Evidence reviewed:**
- [ ] CloudTrail logs
- [ ] VPC Flow Logs
- [ ] Application logs
- [ ] Endpoint logs (MDM/EDR)
- [ ] Email headers (if phishing)
- [ ] Database access logs
- [ ] Other: _______________

**Evidence preserved in:** s3://incident-evidence-[incident-id]/

---

## 7. Root Cause

**Root cause:**
> [The fundamental reason this incident occurred — not the symptom]

**5-Why Analysis:**
1. Why did the incident occur? ___
2. Why did [answer 1] happen? ___
3. Why did [answer 2] happen? ___
4. Why did [answer 3] happen? ___
5. Why did [answer 4] happen? ___
**Root cause conclusion:** ___

---

## 8. Regulatory Notifications

### GDPR Assessment
- **Personal data involved?** Y / N
- **Number of individuals affected:** ___
- **Categories of personal data:** ___
- **Risk assessment (high risk to individuals?):** Y / N
- **Notification to supervisory authority required?** Y / N
- **ICO / DPA notification sent:** YYYY-MM-DD HH:MM UTC (within 72h of awareness: [date/time of awareness])
- **Individual notification required?** Y / N
- **Customer notification sent:** YYYY-MM-DD

### Other Notifications
- [ ] Customer notification: [date]
- [ ] Cyber insurance notified: [date] | Policy #: ___
- [ ] Law enforcement: [date] (if criminal activity suspected)

---

## 9. Remediation Actions

| Action | Owner | Completed | Evidence |
|--------|-------|-----------|---------|
| | | YYYY-MM-DD | |

---

## 10. Post-Incident Review

**Review meeting date:** YYYY-MM-DD
**Attendees:** [list]

### What worked well:
-
-

### What didn't work:
-
-

### Lessons learned:
-
-

### Follow-up actions from review:

| Action | Owner | Due Date | Status |
|--------|-------|---------|--------|
| Update IRP: [specific section] | ISMS Owner | | |
| Implement control: [control name] | IT Lead | | |
| Update risk register | ISMS Owner | | |

---

## 11. Closure Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Incident Commander | | | |
| ISMS Owner | | | |
| CEO (P1/P2 only) | | | |

**Retain this record for:** 3 years minimum (longer if regulatory action pending)
