# Annual ISMS Review Calendar
**ISO 27001:2022 — Post-Certification | Version 1.0**
**Owner:** ISMS Owner
**Purpose:** Rolling 12-month schedule of all mandatory and recommended ISMS activities

---

## How to Use

- Copy this template at the start of each certification year.
- Fill in specific dates for each activity.
- Use as the basis for calendar invites sent to all stakeholders.
- Mark each item complete with date when done.
- Present completion status at management review.

---

## Mandatory Activities (ISO 27001 Required)

| Activity | ISO Ref | Frequency | Q1 Date | Q2 Date | Q3 Date | Q4 Date | Owner | Done |
|----------|---------|-----------|---------|---------|---------|---------|-------|------|
| Internal audit | 9.2 | Annual (min) | or | or | [date] | or | ISMS Owner | ☐ |
| Management review | 9.3 | Annual (min) | [date] | | | [date] | ISMS Owner + CEO | ☐ ☐ |
| Risk assessment review | 6.1.2 | Annual + triggers | | | [date] | | ISMS Owner | ☐ |
| SoA review | 6.1.3 | Annual | | | [date] | | ISMS Owner | ☐ |
| Corrective action review | 10.1 | Ongoing | [monthly] | [monthly] | [monthly] | [monthly] | ISMS Owner | |
| Access review (quarterly) | A.5.18 | Quarterly | [date] | [date] | [date] | [date] | IT Lead | ☐ ☐ ☐ ☐ |
| Security awareness training | A.6.3 | Annual programme | Launch Jan | | | Close Dec | ISMS Owner | ☐ |
| AUP re-acknowledgement | A.5.10 | Annual | [date] | | | | HR | ☐ |

---

## Infrastructure and Technical Activities

| Activity | Standard Ref | Frequency | Schedule | Owner | Done |
|----------|-------------|-----------|---------|-------|------|
| Backup restore test | A.8.13 | Quarterly | Jan / Apr / Jul / Oct | DevOps Lead | ☐ ☐ ☐ ☐ |
| DR failover test | A.5.30 | Annual | [date] | DevOps Lead + CTO | ☐ |
| Vulnerability scan review | A.8.8 | Monthly | 1st week each month | DevOps Lead | ☐ × 12 |
| Patch compliance review | A.8.8 | Monthly | 1st week each month | DevOps Lead | ☐ × 12 |
| KMS key rotation check | A.8.24 | Annual | [date] | DevOps Lead | ☐ |
| SSL/TLS certificate audit | A.8.24 | Quarterly | [dates] | DevOps Lead | ☐ ☐ ☐ ☐ |
| Penetration test | A.5.35 | Annual | [date] | CTO (procure) | ☐ |
| Cloud security baseline review | A.8.9 | Annual | [date] | DevOps Lead | ☐ |
| GitHub org settings review | A.8.4 | Quarterly | [dates] | Engineering Lead | ☐ ☐ ☐ ☐ |

---

## Awareness and Training Activities

| Activity | Frequency | Target Date | Owner | Done |
|----------|-----------|------------|-------|------|
| Annual training programme launch | Annual | January | ISMS Owner | ☐ |
| M1 IS Fundamentals (all staff) | Annual | Jan–Feb | ISMS Owner | ☐ |
| M2 Phishing / Social Engineering | Annual | Jan–Feb | ISMS Owner | ☐ |
| M3 Data Protection / GDPR | Annual | Jan–Feb | ISMS Owner | ☐ |
| M4 Secure Development (devs) | Annual | March | Engineering Lead | ☐ |
| M5 Executive Briefing | Annual | September | ISMS Owner | ☐ |
| Phishing simulation Q1 | Quarterly | January | ISMS Owner | ☐ |
| Phishing simulation Q2 | Quarterly | April | ISMS Owner | ☐ |
| Phishing simulation Q3 | Quarterly | July | ISMS Owner | ☐ |
| Phishing simulation Q4 | Quarterly | October | ISMS Owner | ☐ |
| Training completion audit | Annual | May (chase non-completers) | HR | ☐ |
| Security awareness content update | Annual | December (plan next year) | ISMS Owner | ☐ |

---

## Supplier Management Activities

| Activity | Frequency | Schedule | Owner | Done |
|----------|-----------|---------|-------|------|
| Tier 1 supplier annual assessment | Annual | [month per supplier] | ISMS Owner | ☐ |
| Subprocessor register review | Annual | [date] | ISMS Owner | ☐ |
| DPA expiry check | Annual | [date] | Legal / ISMS Owner | ☐ |
| Supplier certificate validity check | Annual | [date] | ISMS Owner | ☐ |
| New supplier security review | As needed | Before contract signature | ISMS Owner | — |

---

## Policy and Document Review

| Document | Review Cycle | Last Reviewed | Next Review | Owner | Done |
|---------|-------------|--------------|------------|-------|------|
| Information Security Policy | Annual | | [date] | ISMS Owner / CEO | ☐ |
| Acceptable Use Policy | Annual | | | ISMS Owner | ☐ |
| Access Control Policy | Annual | | | ISMS Owner | ☐ |
| Cryptography Policy | Annual | | | ISMS Owner | ☐ |
| Data Classification Policy | Annual | | | ISMS Owner | ☐ |
| Incident Response Plan | Annual + after incident | | | ISMS Owner | ☐ |
| Supplier Security Policy | Annual | | | ISMS Owner | ☐ |
| Change Management Policy | Annual | | | ISMS Owner | ☐ |
| Business Continuity Plan | Annual + after BCP test | | | ISMS Owner | ☐ |
| Disaster Recovery Runbook | Annual + after DR test | | | DevOps Lead | ☐ |
| All other procedures | Annual | | | ISMS Owner | ☐ |

---

## Incident Response Activities

| Activity | Frequency | Schedule | Owner | Done |
|----------|-----------|---------|-------|------|
| IRP review and update | Annual + after incident | [date] | ISMS Owner | ☐ |
| Tabletop exercise | Annual (min); semi-annual recommended | [date] | ISMS Owner (facilitator) | ☐ |
| Contact list verification (IRT, legal, insurance) | Quarterly | [dates] | ISMS Owner | ☐ ☐ ☐ ☐ |
| Evidence S3 bucket review (access control, Object Lock) | Annual | [date] | DevOps Lead | ☐ |
| Cyber insurance renewal check | Annual | [date] | CEO | ☐ |

---

## Monthly Checklist (ISMS Owner)

Do these at the start of each month:

- [ ] Update metrics dashboard (incidents, vulns, training, access)
- [ ] Review open NCs in corrective action tracker
- [ ] Check upcoming access review due dates
- [ ] Review GuardDuty finding trends (weekly is better)
- [ ] Check vulnerability scan results — any SLA breaches?
- [ ] Check patch compliance report
- [ ] Update improvement backlog with any new items

---

## Certification Timeline (3-Year Cycle)

| Year | Month | Activity |
|------|-------|---------|
| Year 1 | Jan | Certification year begins |
| Year 1 | Ongoing | Monthly/quarterly ISMS activities (this calendar) |
| Year 1 | Nov–Dec | Year 1 surveillance audit prep (see `surveillance_audit_prep.md`) |
| Year 2 | Jan | Year 1 surveillance audit |
| Year 2 | Ongoing | Monthly/quarterly ISMS activities |
| Year 2 | Nov–Dec | Year 2 surveillance audit prep |
| Year 3 | Jan | Year 2 surveillance audit |
| Year 3 | Q1–Q2 | Begin recertification preparation |
| Year 3 | Q3 | Stage 1 recertification audit |
| Year 3 | Q4 | Stage 2 recertification audit (before cert expiry) |
| Year 4 | Jan | New 3-year cycle begins |

---

## Annual Calendar Summary — Key Dates

Fill in at start of each year:

| Month | Key ISMS Activities |
|-------|-------------------|
| January | Training launch; Q1 phishing sim; Q1 access review; backup test |
| February | Training completion push; risk assessment kick-off |
| March | Developer training; risk assessment complete; management review #1 |
| April | Q2 phishing sim; Q2 access review; backup test; patch review |
| May | Training completion audit; NC tracker review |
| June | Certificate validity check if due; supplier review cycle |
| July | Q3 phishing sim; Q3 access review; backup test; DR test |
| August | Internal audit planning; executive briefing prep |
| September | Executive briefing; internal audit execution |
| October | Cybersecurity month; Q4 phishing sim; patch review |
| November | Management review #2; surveillance audit prep if due |
| December | Year-end metrics review; plan next year's programme |
