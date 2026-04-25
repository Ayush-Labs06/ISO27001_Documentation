# Corrective Action Tracker
**ISO 27001:2022 — Clause 10.1 | Version 1.0**
**Owner:** ISMS Owner
**Updated:** [Date]

---

## How to Use

- Add a row for every nonconformity (major and minor) as it is identified.
- Update status column as actions progress.
- Present open NCs at every management review.
- Closed NCs are retained but marked Closed — do not delete rows.
- This tracker is evidence for Clause 10.1 and auditors will review it in full.

---

## Status Definitions

| Status | Meaning |
|--------|---------|
| **Open** | NC identified; root cause analysis not yet complete |
| **In Progress — Root Cause** | Root cause being investigated |
| **In Progress — Action** | Corrective action plan agreed and underway |
| **Pending Verification** | Actions complete; awaiting auditor sign-off |
| **Closed** | Verified as effective; NC formally closed |
| **Overdue** | Past agreed completion date; escalation required |

---

## Active Corrective Actions

| NCR # | Date Raised | Grade | Area | Clause | Description (brief) | Root Cause | Corrective Action | Owner | Due Date | Status | Closed Date |
|-------|------------|-------|------|--------|---------------------|-----------|------------------|-------|---------|--------|------------|
| NCR-[YYYY]-001 | | Major / Minor | | | | | | | | Open | |
| NCR-[YYYY]-002 | | | | | | | | | | | |
| NCR-[YYYY]-003 | | | | | | | | | | | |

---

## Sample Populated Entries (for reference)

| NCR # | Date Raised | Grade | Area | Clause | Description | Root Cause | Corrective Action | Owner | Due Date | Status | Closed |
|-------|------------|-------|------|--------|-------------|-----------|------------------|-------|---------|--------|--------|
| NCR-2024-001 | 2024-03-15 | Major | Cryptography | A.8.24 | RDS prod DB storage encryption disabled; contains PII | Encryption not enforced on DB creation; no automated policy check | 1. Enable encryption (snapshot + restore encrypted) 2. Add AWS Config rule: encrypted-volumes 3. Add Terraform policy check (Checkov) | DevOps Lead | 2024-04-15 | Closed | 2024-04-10 |
| NCR-2024-002 | 2024-03-15 | Minor | Access Control | A.5.18 | Q3 access review not completed on time | No reminder process in place; ISMS Owner assumed IT Lead had scheduled it | 1. Add quarterly access review to ISMS calendar with 2-week reminder 2. Document named owner for each review | IT Lead | 2024-04-30 | Closed | 2024-04-25 |
| NCR-2024-003 | 2024-06-20 | Minor | Awareness | A.6.3 | 3 employees (22%) training completion outstanding after 45 days | Reminder email not sent; LMS not configured for auto-escalation | 1. Configure LMS auto-reminder at 14 and 30 days 2. Escalate outstanding to HR at 30 days 3. Complete outstanding training | ISMS Owner / HR | 2024-07-20 | Closed | 2024-07-18 |
| NCR-2024-004 | 2024-09-10 | Major | Supplier Mgmt | A.5.19 | 6 Tier-1 suppliers in subprocessor register have no completed assessment | Supplier review process not resourced; assumed existing contract was sufficient | 1. Complete assessments for all 6 suppliers by deadline 2. Implement quarterly supplier review in ISMS calendar | ISMS Owner | 2024-11-01 | In Progress | |
| NCR-2024-005 | 2024-09-10 | Obs | Incident Resp | A.5.25 | IRP has no named backup Incident Commander | Observation only — no mandatory requirement | 1. Update IRP section 2 with backup IC role 2. Confirm backup in writing | ISMS Owner | 2024-10-01 | Closed | 2024-09-30 |

---

## NC Trend Summary

Update quarterly. Used as input to management review.

| Quarter | Major NCs Opened | Major NCs Closed | Minor NCs Opened | Minor NCs Closed | Observations | Overdue |
|---------|-----------------|-----------------|-----------------|-----------------|-------------|---------|
| Q1 [Year] | | | | | | |
| Q2 [Year] | | | | | | |
| Q3 [Year] | | | | | | |
| Q4 [Year] | | | | | | |
| **Total [Year]** | | | | | | |

---

## Overdue NC Escalation

Any NC past its due date without closure is escalated:

| NCR # | Original Due Date | Days Overdue | Reason for Delay | New Due Date | Escalated To | Date Escalated |
|-------|------------------|-------------|-----------------|-------------|-------------|---------------|
| | | | | | | |

**Escalation policy:**
- Overdue by >14 days: ISMS Owner notified
- Overdue by >30 days: CEO notified; added to management review agenda
- Overdue by >60 days: Risk register entry updated; may affect certification

---

## Closed NC Archive

Retain records of all closed NCs. Do not delete rows — move to this section when closed.

| NCR # | Date Raised | Grade | Description (brief) | Closed Date | Closed By | Effectiveness Confirmed |
|-------|------------|-------|---------------------|------------|----------|------------------------|
| | | | | | | |
