# Internal Audit Plan
**ISO 27001:2022 — Clause 9.2 | Version 1.0**
**Owner:** ISMS Owner (Internal Audit Programme Manager)
**Approved by:** [CEO] | [Date]

---

## 1. Purpose

The internal audit programme provides independent assurance that the ISMS:
- Conforms to ISO 27001:2022 requirements (Clauses 4–10 + Annex A)
- Conforms to [Organization Name]'s own ISMS requirements (policies, procedures, standards)
- Is effectively implemented and maintained

---

## 2. Audit Programme Overview

**Programme period:** [Year] — [Year+1]
**Audit scope coverage:** Full ISMS scope — all ISO 27001:2022 clauses and applicable Annex A controls
**Minimum frequency:** Annual (full scope must be covered within each certification cycle)
**Auditor:** [Name] — Internal auditor; independent of activities being audited

> **Independence requirement:** The internal auditor must not audit their own work. For small teams, this may require engaging an external consultant for specific areas (e.g., DevOps Lead should not audit their own infrastructure controls). Document how independence is maintained.

---

## 3. Annual Audit Schedule

| Audit | Scope | Auditor | Planned Date | Actual Date | Status |
|-------|-------|---------|-------------|-------------|--------|
| Audit 1 | Clauses 4–6 (Context, Leadership, Planning) + ISMS documentation review | [Name] | [Date] | | Planned |
| Audit 2 | Clause 7 (Support: docs, resources, awareness) + Annex A.6 (People controls) | [Name] | [Date] | | Planned |
| Audit 3 | Clause 8 (Operations) + Annex A.8 (Technological controls — infrastructure) | [Name] | [Date] | | Planned |
| Audit 4 | Clause 9–10 (Performance, Improvement) + Annex A.5 (Organizational controls) | [Name] | [Date] | | Planned |
| Audit 5 | Annex A.7 (Physical controls) + supplier/access control review | [Name] | [Date] | | Planned |

**Alternatively:** Single annual audit covering full scope (common for small organizations).

---

## 4. Audit Criteria

Each audit is evaluated against:
1. ISO 27001:2022 requirements (the standard itself)
2. [Organization Name] ISMS policies and procedures
3. Annex A controls as documented in the Statement of Applicability (SoA)

---

## 5. Audit Methodology

### Evidence Collection Methods

| Method | Used For |
|--------|---------|
| Document review | Policies, procedures, records — check version, owner, review date |
| Staff interviews | Verify understanding and actual practice vs. documented procedure |
| Technical observation | Walk-through of system configurations, tool demonstrations |
| Log/record sampling | Access reviews, training records, change logs, incident logs |
| Process walkthrough | Follow a process end-to-end (e.g., new employee onboarding) |

### Sampling Approach

For large record populations (training completion, access reviews, change logs):
- Population >50: sample minimum 10% or 10 records (whichever is larger)
- Population <50: sample minimum 5 records or all, whichever is smaller
- High-risk areas: increase sample size

### Nonconformity Classification

| Grade | Definition | Response Required |
|-------|-----------|------------------|
| **Major NC** | Failure to meet a requirement of ISO 27001; system breakdown; complete absence of required control | Immediate corrective action; must be closed before certification |
| **Minor NC** | Partial failure; control exists but not consistently applied; isolated lapse | Corrective action within agreed timeframe (typically 30–90 days) |
| **Observation** | Not a nonconformity but a risk or improvement opportunity | Optional to act on; documented for management awareness |
| **Positive Finding** | Control working well — noteworthy good practice | Document; no action required |

---

## 6. Audit Programme Inputs

Before each audit, collect:
- [ ] Previous audit report + open nonconformities + status of corrective actions
- [ ] Risk register (current version)
- [ ] Statement of Applicability (current version)
- [ ] Incident log since last audit (any incidents that warrant focus)
- [ ] Significant changes since last audit (new systems, org changes, new suppliers)
- [ ] Management review outputs (any decisions affecting audit focus)
- [ ] Previous certification body findings (if applicable)

---

## 7. Audit Report Requirements

Each completed audit must produce:
- [ ] Audit report (use `audit_report_template.md`)
- [ ] Nonconformity reports for each NC identified (use `nonconformity_report_template.md`)
- [ ] Updated corrective action tracker
- [ ] List of evidence reviewed (sampling records)

**Distribution:** Audit report to ISMS Owner, CEO, relevant function heads.
**Retention:** 3 years minimum.

---

## 8. Corrective Action Follow-Up

| Step | Owner | Timing |
|------|-------|--------|
| NC raised in report | Internal Auditor | At time of audit |
| Root cause analysis completed | Auditee (process owner) | Within 10 business days |
| Corrective action plan agreed | ISMS Owner + Auditee | Within 15 business days |
| Corrective action implemented | Auditee | Per agreed plan (major: <30 days, minor: <90 days) |
| Verification of effectiveness | Internal Auditor | On completion of action |
| NC closed | ISMS Owner | After verification |

Track all NCs in `corrective_action_tracker.md`.

---

## 9. Audit Programme Review

The audit programme itself is reviewed annually by the ISMS Owner and presented at management review. Inputs to programme review:
- Number of audits completed vs. planned
- NC trends (which areas generate most findings)
- Auditor competence and independence
- Changes to organization scope or risk profile that warrant focus area changes

**Audit programme record retained as evidence for Clause 9.2.**
