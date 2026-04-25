# ISO 27001 Implementation Project Charter
**Version:** 1.0 | **Status:** Draft / Approved | **Date:** YYYY-MM-DD

---

## 1. Project Overview

| Field | Detail |
|-------|--------|
| Client Organization | [Legal name] |
| Implementor / Consultant | [Your name / firm] |
| Project Name | ISO 27001:2022 ISMS Implementation |
| Target Certification Body | [BSI / Bureau Veritas / LRQA / DNV / other — TBD] |
| Target Certification Date | YYYY-MM-DD |
| ISMS Scope (summary) | [1-2 sentence summary — full scope in scope doc] |
| Project Start Date | YYYY-MM-DD |
| Project Manager (client side) | [Name, role] |
| Lead Implementor | [Your name] |

---

## 2. Project Objectives

1. Design and implement an ISO 27001:2022-conformant ISMS covering [scope].
2. Achieve Stage 1 and Stage 2 certification by [target date].
3. Remediate all critical and high-priority gaps identified in the gap analysis.
4. Build internal capability so the client can sustain the ISMS post-certification without full reliance on external consultants.
5. Produce all mandatory documentation required by the standard (Clause 7.5).

---

## 3. Deliverables

| # | Deliverable | Owner | Due Date |
|---|------------|-------|----------|
| 1 | Gap Analysis Report | Consultant | Week 3 |
| 2 | ISMS Scope Statement | Consultant + Client | Week 4 |
| 3 | Risk Assessment & Risk Register | Consultant + Client | Week 6 |
| 4 | Statement of Applicability (SoA) | Consultant | Week 7 |
| 5 | Risk Treatment Plan | Consultant + Client | Week 8 |
| 6 | Full Policy Suite (10+ policies) | Consultant | Week 10 |
| 7 | Infrastructure Remediation Report | Consultant | Week 12 |
| 8 | Supplier Register + assessments | Client (supported) | Week 12 |
| 9 | Training programme + completion records | Client | Week 14 |
| 10 | Internal Audit Report | Consultant | Week 18 |
| 11 | Corrective Action closure | Client + Consultant | Week 20 |
| 12 | Management Review Minutes | Client | Week 22 |
| 13 | Stage 1 Audit support | Consultant | Week 24 |
| 14 | Stage 2 Audit support | Consultant | Week 28+ |

---

## 4. Project Timeline (High Level)

```
Month 1: Scoping, Gap Analysis, Risk Methodology
Month 2: Risk Assessment, Asset Register, Threat Modelling
Month 3: SoA, Risk Treatment Plan, Policy Writing (Phase 1)
Month 4: Policy Writing (Phase 2), Infrastructure Remediation begins
Month 5: Training, Supplier Management, Access Control remediation
Month 6: Internal Audit, Corrective Actions
Month 7: Management Review, Stage 1 Prep
Month 8: Stage 1 Audit
Month 9: Stage 1 findings closed, Stage 2 prep
Month 10+: Stage 2 Audit → Certification
```

---

## 5. Roles & Responsibilities

| Role | Person | Responsibilities |
|------|--------|-----------------|
| Sponsor / Top Management | [CEO/MD name] | Approve policy, attend management review, sign off objectives |
| Project Lead (client) | [Name] | Day-to-day coordination, chase internal stakeholders, evidence collection |
| Lead Implementor | [Consultant name] | Gap analysis, risk assessment, policy writing, audit prep, advisory |
| IT / Infrastructure Lead | [Name] | Infrastructure remediation, log management, backup testing |
| HR Lead | [Name] | Training records, onboarding/offboarding procedures |
| Legal / Compliance | [Name or "N/A — escalate to CEO"] | Contract reviews, DPA, regulatory queries |
| Information Asset Owners | [Various dept leads] | Own their assets in the asset register, review access |

---

## 6. In Scope vs. Out of Scope (for this Project)

**In scope for this engagement:**
- Full ISMS implementation through to certification
- All mandatory ISO 27001:2022 documentation
- Infrastructure gap assessment and remediation guidance (not hands-on infra work unless agreed separately)
- Internal audit (first cycle)
- Stage 1 and Stage 2 audit support

**Out of scope (requires separate agreement):**
- Penetration testing
- Hands-on infrastructure configuration / DevOps work
- Legal review of contracts
- GDPR / data protection gap analysis (though we'll flag crossovers)
- Post-certification surveillance audits (separate retainer)

---

## 7. Assumptions

1. Client will assign a named project lead with at least 0.25 FTE availability for this project.
2. Senior management will actively participate, particularly for policy approval and management review.
3. Client has authority to implement technical controls on their own infrastructure.
4. No major platform migrations or organizational restructures during the project (flag immediately if this changes).
5. Client will procure penetration testing separately if not already done within the last 12 months.
6. Certification body will be selected and booked by Month 6.

---

## 8. Risks & Issues

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| Key person dependency — project lead leaves | M | H | Cross-train backup; document all decisions |
| Management engagement drops off after kickoff | H | H | Regular steering meetings; build it into the charter |
| Technical remediation takes longer than expected | H | M | Identify top 10 infra gaps in Month 1; start remediation early |
| Certification body slots unavailable | M | H | Book CB by Month 4 — slots fill fast |
| Scope creep — "can we add X product?" | M | M | Formal change control; scope changes require written agreement |
| No pentest completed | L | H | Flag in kickoff; it's not a hard requirement but Stage 2 auditors love to ask about it |

---

## 9. Communication Plan

| Meeting | Frequency | Attendees | Purpose |
|---------|-----------|-----------|---------|
| Project Steering | Bi-weekly | Sponsor + Project Lead + Consultant | Status, decisions, blockers |
| Technical Working Session | Weekly (Months 1-3) | IT Lead + Consultant | Infrastructure and technical controls |
| Policy Review | As needed | Dept heads + Consultant | Review and approve policies |
| Internal Audit Debrief | Once | All stakeholders | Present findings, agree corrective actions |
| Management Review | Once (pre-Stage 2) | Top management | Mandatory clause 9.3 activity |

---

## 10. Sign-Off

By signing below, the parties agree to the scope, deliverables, and responsibilities described in this charter.

| Name | Role | Signature | Date |
|------|------|-----------|------|
| | Client Sponsor | | |
| | Client Project Lead | | |
| | Lead Implementor | | |
