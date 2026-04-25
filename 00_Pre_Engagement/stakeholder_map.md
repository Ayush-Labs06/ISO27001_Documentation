# ISMS Stakeholder Map & RACI
**ISO 27001:2022 — Clause 5.1 (Leadership), 5.3 (Roles)**

---

## Roles Reference

| Role | ISO 27001 Requirement | Typical Person at a Startup |
|------|----------------------|----------------------------|
| Top Management | Must demonstrate leadership (Clause 5.1); approve policy; lead management review | CEO / MD / Founder |
| ISMS Owner / CISO | Overall accountability for ISMS | CTO, Head of Engineering, or you (consultant interim) |
| Risk Owner | Accept/treat risks in their domain | Dept leads, CTO |
| Internal Auditor | Must be independent from audited area | Can be consultant, or trained internal person from a different team |
| Information Asset Owners | Responsible for their assets in the register | Engineering leads, HR, Finance |
| DPO | Required under GDPR if processing personal data at scale | Legal counsel, external DPO service |
| All Staff | Comply with ISMS policies | Everyone |

---

## RACI Matrix — ISMS Activities

**R** = Responsible (does the work) | **A** = Accountable (owns the outcome) | **C** = Consulted | **I** = Informed

| Activity | CEO | CISO/CTO | IT/Infra Lead | HR | Legal | Dept Leads | All Staff | Consultant |
|----------|-----|----------|--------------|-----|-------|------------|-----------|------------|
| Approve ISMS scope | A | R | C | I | C | C | - | C |
| Approve IS policy | A | R | I | I | C | I | I | C |
| Risk assessment | I | A | R | C | C | C | - | R |
| Approve risk treatment | A | R | C | I | I | R | - | C |
| Approve SoA | A | R | I | I | I | I | - | C |
| Write policies | I | A | C | C | C | C | - | R |
| Approve policies | A | R | I | I | C | C | - | C |
| Infrastructure remediation | I | A | R | I | I | I | - | C |
| Maintain asset register | I | A | R | C | I | R | - | C |
| Security training | I | A | C | R | I | C | R | C |
| Access reviews | I | A | R | C | I | R | - | C |
| Supplier assessments | I | A | I | I | C | R | - | C |
| Incident response | I | A | R | C | C | C | R | I |
| Internal audit | A | C | C | C | C | C | C | R |
| Corrective actions | I | A | R | R | I | R | - | C |
| Management review | A | R | C | C | C | C | - | C |
| Certification audit | A | R | R | C | C | C | - | R |

---

## Stakeholder Register

Fill this in during kickoff:

| Name | Role/Title | ISMS Role | Contact | Availability for project |
|------|-----------|-----------|---------|--------------------------|
| | CEO/Founder | Top Management, Sponsor | | Bi-weekly steering |
| | CTO / Head of Eng | ISMS Owner / CISO | | Weekly working sessions |
| | Senior Engineer | IT / Infra Lead | | Daily (project) |
| | HR Manager | People & Training | | As needed |
| | Finance Lead | Asset Owner (Finance) | | Monthly |
| | Product Lead | Asset Owner (Product) | | Monthly |
| | Legal Counsel | DPA, contracts | | As needed |
| [Your name] | Consultant | Lead Implementor | | Per schedule |

---

## Common Startup Org Patterns

### Pattern A: Tiny startup (< 20 people)
- CEO = Top Management + Risk Owner
- CTO = ISMS Owner + IT Lead + Asset Owner for everything technical
- Consultant = Internal Auditor + Risk Assessor + Policy writer
- Risk: single points of failure everywhere. Mitigate with documentation.

### Pattern B: Seed/Series A (20-80 people)
- CEO = Sponsor (limited involvement)
- Head of Engineering = ISMS Owner
- DevOps/Platform Engineer = IT Lead
- HR = Training + Onboarding/Offboarding
- Consultant = Auditor + advisor, policy writing, risk methodology

### Pattern C: Series B+ (80+ people)
- CISO or Security Lead hired = ISMS Owner
- IT Manager = Asset management, infrastructure
- DPO in place (GDPR)
- Consultant = supplementary expertise, internal audit
- Risk: politics between security team and engineering. Navigate carefully.

---

## Engagement Tips

- **Get top management in the room for kickoff** — not just the CTO. The CEO needs to understand they have personal accountability for the ISMS under ISO 27001 Clause 5.1. If they're absent from the project, you'll hit blockers when you need policy approvals.
- **Name every asset owner explicitly** — "IT Department owns it" is not acceptable to an auditor. There must be a named human.
- **Clarify the internal auditor early** — you as consultant can run the first internal audit, but document that you're independent of the areas being audited. If you wrote the policies, don't audit the policies in isolation — have someone else review those specific areas.
