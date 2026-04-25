# Stage 1 Audit Readiness Checklist
**ISO 27001:2022 — Certification Preparation | Version 1.0**
**Owner:** ISMS Owner
**Purpose:** Ensure all required documented information is in place before the Stage 1 (document review) audit

---

## What Happens at Stage 1

The Stage 1 audit is a document review. The certification body (CB) auditor will:
1. Review your ISMS documentation to confirm it addresses all ISO 27001:2022 requirements
2. Verify that mandatory documented information exists
3. Confirm that the scope is clear and appropriate
4. Assess whether you are ready for Stage 2 (evidence audit)

**Stage 1 does NOT involve deep evidence sampling.** They will not interview all your staff or test your systems. They will read your documents and interview ISMS Owner and key personnel.

**Stage 1 output:** List of findings (gaps to address before Stage 2) and a Stage 2 audit date.

---

## Mandatory Documented Information (Clause 7.5)

All 11 mandatory documents must be present, version-controlled, and approved.

| # | Document | ISO Ref | Version | Owner | Date Approved | Location | Ready |
|---|---------|---------|---------|-------|--------------|---------|-------|
| 1 | ISMS Scope Statement | 4.3 | | | | | ☐ |
| 2 | Information Security Policy | 5.2 | | | | | ☐ |
| 3 | IS Objectives (and plan to achieve them) | 6.2 | | | | | ☐ |
| 4 | Risk Assessment Methodology | 6.1.2 | | | | | ☐ |
| 5 | Risk Register (results of risk assessment) | 6.1.2 | | | | | ☐ |
| 6 | Risk Treatment Plan | 6.1.3 | | | | | ☐ |
| 7 | Statement of Applicability (SoA) | 6.1.3(d) | | | | | ☐ |
| 8 | IS Objectives with metrics | 6.2 | | | | | ☐ |
| 9 | Competence records (training records) | 7.2 | | | | | ☐ |
| 10 | Monitoring and measurement results | 9.1 | | | | | ☐ |
| 11 | Internal audit programme + results | 9.2 | | | | | ☐ |
| 12 | Management review minutes | 9.3 | | | | | ☐ |
| 13 | Nonconformities and corrective actions | 10.1 | | | | | ☐ |

> Note: The standard requires these as "documented information." Your policies, procedures, and records collectively satisfy these. The SoA is the single most scrutinised document — ensure every control has a justification.

---

## Supporting Policies and Procedures

These are not explicitly mandatory but are expected and will be asked for:

| Document | Present | Current (reviewed within 12 months) | Approved | Notes |
|---------|---------|-------------------------------------|---------|-------|
| Acceptable Use Policy (AUP) | ☐ | ☐ | ☐ | |
| Access Control Policy | ☐ | ☐ | ☐ | |
| Cryptography Policy | ☐ | ☐ | ☐ | |
| Data Classification Policy | ☐ | ☐ | ☐ | |
| Incident Response Policy | ☐ | ☐ | ☐ | |
| Incident Response Plan (IRP) | ☐ | ☐ | ☐ | |
| Supplier Security Policy | ☐ | ☐ | ☐ | |
| Change Management Policy | ☐ | ☐ | ☐ | |
| Business Continuity Plan | ☐ | ☐ | ☐ | |
| Document Control Procedure | ☐ | ☐ | ☐ | |
| Vulnerability Management Procedure | ☐ | ☐ | ☐ | |
| Patch Management Procedure | ☐ | ☐ | ☐ | |
| Backup and Recovery Procedure | ☐ | ☐ | ☐ | |
| Onboarding/Offboarding Procedure | ☐ | ☐ | ☐ | |
| Logging and Monitoring Procedure | ☐ | ☐ | ☐ | |

---

## SoA Quality Check

The SoA is the document the auditor goes to first. Check each of the following:

- [ ] All 93 controls listed (not 114 — that is the 2013 count)
- [ ] Each control is marked "Included" or "Excluded"
- [ ] Every excluded control has a documented justification
- [ ] Every included control has an implementation status (Implemented / Partial / Planned)
- [ ] Controls marked "Included" are actually implemented (not just listed as "planned" across the board)
- [ ] SoA is signed off by ISMS Owner and CEO
- [ ] SoA version and date is current
- [ ] SoA cross-references to risk treatment plan (which controls address which risks)

**Red flag for auditors:** SoA where every control is "Included" but with no implementation evidence — looks like a paper exercise. Exclusions with real justifications demonstrate genuine thought.

---

## Scope Document Check

- [ ] Scope statement clearly defines what is IN scope (systems, processes, locations)
- [ ] Scope explicitly names what is OUT of scope, with justification
- [ ] Interfaces with out-of-scope elements are documented (e.g., AWS-managed services, third-party SaaS)
- [ ] Scope matches what was agreed with the CB at contract stage
- [ ] Scope does not unreasonably exclude high-risk areas just to simplify the audit

---

## Risk Assessment Quality Check

- [ ] Risk methodology is documented (likelihood × impact scales defined)
- [ ] Risk acceptance criteria explicitly stated (e.g., "residual risks scoring >15 must be treated")
- [ ] Risk register covers all major asset types (data, applications, infrastructure, people, physical)
- [ ] At least 20–30 risks in register for a typical startup (fewer suggests incomplete assessment)
- [ ] Each risk has: asset, threat, vulnerability, inherent score, controls, residual score, owner
- [ ] Residual risk scores reflect actual controls in place (not aspirational controls not yet implemented)
- [ ] Risk treatment plan covers all risks above acceptance threshold
- [ ] Risk register signed off by ISMS Owner
- [ ] Risk assessment has been performed in the last 12 months (or since last significant change)

---

## Document Control Check

For each mandatory document and key policy, verify:
- [ ] Document title
- [ ] Version number (e.g., v1.0, v1.1)
- [ ] Date (approved/last reviewed)
- [ ] Owner/approved by name
- [ ] Review cycle documented
- [ ] Stored in version-controlled location (not emailed as attachment)

Use the Document Control Review (`document_control_review.md`) to formally verify all documents.

---

## Stage 1 Interview Preparation

The auditor will interview ISMS Owner (and likely CEO, CTO). Common questions:

1. "Describe your ISMS and how it works day to day."
2. "How was the scope of your ISMS determined?"
3. "Walk me through your risk assessment process."
4. "Who is responsible for information security in this organisation?"
5. "What are your IS objectives and how are they measured?"
6. "What was your last internal audit finding?"
7. "How does management demonstrate commitment to IS?"

Prepare 2-sentence answers to each. Practise them with the client before the audit.

---

## Stage 1 — Common Findings (Fix Before Stage 2)

| Finding | Typical Grade | Fix |
|---------|--------------|-----|
| SoA not version-controlled or not signed | Minor NC | Add version/date; get CEO signature |
| Risk register doesn't cover all asset types | Minor NC | Add missing assets/risks |
| Excluded controls in SoA have no justification | Minor NC | Write justification for each exclusion |
| Objectives not measurable or not tracked | Minor NC | Add metrics and targets to IS Policy |
| Management review minutes don't address all 9.3.2 inputs | Minor NC | Update template; redo minutes |
| No internal audit completed | Major NC | Complete internal audit before Stage 2 |
| Corrective action tracker empty or no NCs recorded | Minor NC | Evidence at least some NC process |
| IS Policy not signed by CEO / top management | Minor NC | Get signature |
| Document version control absent (no dates/owners) | Minor NC | Add metadata to all documents |

---

## Stage 1 Complete — Ready for Stage 2?

The CB will issue a Stage 1 report. This typically includes:
- Finding level: Ready / Conditionally ready / Not ready
- List of gaps to address before Stage 2

**Your response:** Address every finding before Stage 2. For each finding, write a 1-paragraph response explaining what you did and providing evidence. Send this to the CB before Stage 2 date.

**Stage 2 date:** Typically 4–12 weeks after Stage 1. Don't rush Stage 2 if Stage 1 found significant gaps.
