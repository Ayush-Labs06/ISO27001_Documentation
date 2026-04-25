# Document Control Review
**ISO 27001:2022 — Clause 7.5 | Version 1.0**
**Owner:** ISMS Owner
**Perform:** Before Stage 1 and annually thereafter

---

## Purpose

Verify that all ISMS documents meet document control requirements: version number, date, named owner, review cycle, and approval. A document without proper metadata is a document control nonconformity.

---

## Document Control Requirements (Clause 7.5.2)

Every controlled document must have:
- [ ] Document title
- [ ] Version number (e.g., 1.0, 1.1, 2.0)
- [ ] Date (creation date and last reviewed/approved date)
- [ ] Author / document owner (named person, not a team)
- [ ] Approved by (for policies: CEO or designated authority)
- [ ] Review cycle (e.g., "Review annually or after significant change")
- [ ] Classification (e.g., Internal, Confidential)
- [ ] Document ID (if using a numbering scheme, e.g., ISMS-POL-001)

---

## Document Control Review — Master Register

| Document Name | Doc ID | Version | Owner | Approved By | Approval Date | Next Review | Status |
|--------------|--------|---------|-------|------------|--------------|-------------|--------|
| **Mandatory Documents** | | | | | | | |
| ISMS Scope and Manual | ISMS-MAN-001 | | | | | | ☐ Pass ☐ Fail |
| Information Security Policy | ISMS-POL-001 | | | | | | ☐ Pass ☐ Fail |
| Risk Assessment Methodology | ISMS-METH-001 | | | | | | ☐ Pass ☐ Fail |
| Risk Register | ISMS-REG-001 | | | | | | ☐ Pass ☐ Fail |
| Risk Treatment Plan | ISMS-PLAN-001 | | | | | | ☐ Pass ☐ Fail |
| Statement of Applicability | ISMS-SOA-001 | | | | | | ☐ Pass ☐ Fail |
| IS Objectives (within IS Policy) | ISMS-POL-001 | | | | | | ☐ Pass ☐ Fail |
| Document Control Procedure | ISMS-PROC-001 | | | | | | ☐ Pass ☐ Fail |
| Internal Audit Plan | ISMS-AUD-001 | | | | | | ☐ Pass ☐ Fail |
| Management Review Procedure | ISMS-PROC-002 | | | | | | ☐ Pass ☐ Fail |
| Corrective Action Procedure (within NCR process) | ISMS-PROC-003 | | | | | | ☐ Pass ☐ Fail |
| **Policies** | | | | | | | |
| Acceptable Use Policy | ISMS-POL-002 | | | | | | ☐ Pass ☐ Fail |
| Access Control Policy | ISMS-POL-003 | | | | | | ☐ Pass ☐ Fail |
| Cryptography Policy | ISMS-POL-004 | | | | | | ☐ Pass ☐ Fail |
| Data Classification Policy | ISMS-POL-005 | | | | | | ☐ Pass ☐ Fail |
| Incident Response Policy | ISMS-POL-006 | | | | | | ☐ Pass ☐ Fail |
| Supplier Security Policy | ISMS-POL-007 | | | | | | ☐ Pass ☐ Fail |
| Change Management Policy | ISMS-POL-008 | | | | | | ☐ Pass ☐ Fail |
| **Procedures and Plans** | | | | | | | |
| Incident Response Plan | ISMS-PROC-004 | | | | | | ☐ Pass ☐ Fail |
| Incident Classification Matrix | ISMS-PROC-005 | | | | | | ☐ Pass ☐ Fail |
| Business Continuity Plan | ISMS-PROC-006 | | | | | | ☐ Pass ☐ Fail |
| Disaster Recovery Runbook | ISMS-PROC-007 | | | | | | ☐ Pass ☐ Fail |
| Vulnerability Management Procedure | ISMS-PROC-008 | | | | | | ☐ Pass ☐ Fail |
| Patch Management Procedure | ISMS-PROC-009 | | | | | | ☐ Pass ☐ Fail |
| Backup and Recovery Procedure | ISMS-PROC-010 | | | | | | ☐ Pass ☐ Fail |
| Onboarding/Offboarding Procedure | ISMS-PROC-011 | | | | | | ☐ Pass ☐ Fail |
| Privileged Access Management Procedure | ISMS-PROC-012 | | | | | | ☐ Pass ☐ Fail |
| Security Awareness Programme Plan | ISMS-PROC-013 | | | | | | ☐ Pass ☐ Fail |
| **Templates and Registers** | | | | | | | |
| Asset Register | ISMS-REG-002 | | | | | | ☐ Pass ☐ Fail |
| Subprocessor Register | ISMS-REG-003 | | | | | | ☐ Pass ☐ Fail |
| Incident Log Template | ISMS-TMPL-001 | | | | | | ☐ Pass ☐ Fail |
| RTO/RPO Worksheet | ISMS-TMPL-002 | | | | | | ☐ Pass ☐ Fail |
| Access Review Template | ISMS-TMPL-003 | | | | | | ☐ Pass ☐ Fail |

---

## Common Document Control Failures

| Failure | Example | Fix |
|---------|---------|-----|
| No version number | Document has no "v1.0" or "Version: 1.0" | Add version to header |
| No date | Policy from 2022 with no date stamp | Add "Approved: YYYY-MM-DD" and "Last Reviewed: YYYY-MM-DD" |
| No named owner | "Owner: ISMS Team" | Name a specific person: "Owner: [Jane Smith, ISMS Owner]" |
| Not approved by right level | IS Policy approved by IT Lead, not CEO | Get CEO signature/approval |
| Review date in the past | "Next review: 2023-01" with no evidence of review | Review and update the document; update date |
| Different versions in circulation | Slack has v1.0, intranet has v1.2 | Establish single source of truth; remove old versions |
| Records have no date | Training tracker rows missing dates | Require date column; backfill with best estimate + note |

---

## Review Summary

| Category | Total Docs | Pass | Fail | Actions Required |
|----------|-----------|------|------|-----------------|
| Mandatory documents | 13 | | | |
| Policies | 8 | | | |
| Procedures | 13 | | | |
| Templates/registers | 5 | | | |
| **Total** | **39** | | | |

**Review completed by:** ___________________ **Date:** ___________________
**Findings actioned by:** ___________________ **By date:** ___________________
