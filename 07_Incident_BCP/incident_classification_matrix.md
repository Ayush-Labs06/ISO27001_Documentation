# Incident Classification Matrix
**ISO 27001:2022 — A.5.25 | Version 1.0**

---

## Severity Definitions

| Severity | Score | Criteria | Response SLA | Escalation |
|----------|-------|---------|-------------|-----------|
| **P1 — Critical** | 4 | Active exploitation; confirmed data breach; full service outage; ransomware active | 30 minutes | CEO + Legal within 1h |
| **P2 — High** | 3 | Suspected compromise; significant data exposure risk; major service degradation | 2 hours | CEO within 4h |
| **P3 — Medium** | 2 | Policy violation; minor unauthorized access; suspicious activity (unconfirmed) | 8 hours | ISMS Owner |
| **P4 — Low** | 1 | Near-miss; failed attack (blocked); user-reported phishing (no click) | 24 hours | ISMS Owner weekly review |

---

## Classification Matrix

Combine **Scope** × **Impact** to determine severity:

| | **Impact: Critical** (PII breach, key compromise, ransomware) | **Impact: High** (significant data exposure, auth bypass) | **Impact: Medium** (policy violation, suspicious access) | **Impact: Low** (attempt blocked, no impact) |
|--|--|--|--|--|
| **Scope: External attack (confirmed)** | P1 | P1 | P2 | P3 |
| **Scope: Internal/Insider** | P1 | P2 | P2 | P3 |
| **Scope: Supplier breach (our data)** | P1 | P2 | P3 | P4 |
| **Scope: Accidental disclosure** | P1 | P2 | P3 | P4 |
| **Scope: Physical/device** | P1 | P2 | P3 | P4 |
| **Scope: System/technical failure** | P2 | P2 | P3 | P4 |

---

## Incident Type Reference

| Incident Type | Typical Severity | Notes |
|--------------|-----------------|-------|
| Ransomware — active encryption | P1 | Immediate containment; involve legal |
| Confirmed PII exfiltration | P1 | GDPR 72h clock starts |
| AWS root account access (unauthorized) | P1 | Full account compromise risk |
| Credential stuffing — accounts breached | P1/P2 | Depends on data accessed |
| S3 bucket publicly exposed (confirmed) | P1 | Assess data exposed; notify if PII |
| Malware on endpoint (uncontained) | P2 | Isolate device immediately |
| Phishing click + credential entered | P2 | Treat as account compromise |
| Accidental email to wrong recipient (PII) | P2 | GDPR assessment required |
| Lost/stolen laptop (unencrypted) | P2 | FDE enforced? Check MDM |
| Lost/stolen laptop (encrypted + MDM) | P3 | Document; remote wipe; lower risk |
| Brute force (blocked by rate limiting) | P3 | Monitor; no immediate action |
| Unauthorized access attempt (failed) | P4 | Log; review pattern |
| Phishing email received (not clicked) | P4 | Report to email provider |
| Policy violation (AUP) | P3 | HR involvement |
| Third-party supplier breach (not our data) | P3 | Monitor for updates |
| Third-party supplier breach (our data) | P1/P2 | GDPR assessment; notification |

---

## Incident Log Template

Copy this block for each incident:

```
INCIDENT LOG
============
Incident ID: INC-[YYYY]-[NNN]
Date/Time Opened: YYYY-MM-DD HH:MM UTC
Severity: P[1-4]
Incident Type: [ransomware / data breach / unauthorized access / etc.]
Incident Commander: [name]
Technical Lead: [name]

DESCRIPTION
-----------
[What was observed; when; how discovered]

AFFECTED SYSTEMS / DATA
-----------------------
[Systems, data types, estimated individuals affected]

TIMELINE
--------
[HH:MM] [Action / observation]
[HH:MM] [Action / observation]

CONTAINMENT ACTIONS
-------------------
[What was done to contain the incident]

ROOT CAUSE
----------
[To be completed post-investigation]

REGULATORY NOTIFICATION
-----------------------
[ ] GDPR notification required? Y/N
    [ ] ICO notified? Date/Time:
    [ ] Customer notification required? Date sent:

EVIDENCE PRESERVED
------------------
[ ] CloudTrail logs exported to: [bucket/path]
[ ] Application logs archived
[ ] Screenshots taken

STATUS
------
[ ] Open — Active response
[ ] Contained — Under investigation
[ ] Remediated — Awaiting verification
[ ] Closed — Post-incident review complete

Date/Time Closed: YYYY-MM-DD HH:MM UTC
Total Resolution Time: [X hours]
Post-Incident Review Date: YYYY-MM-DD
```
