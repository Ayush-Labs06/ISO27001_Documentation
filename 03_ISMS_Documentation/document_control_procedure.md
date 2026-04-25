# Document Control Procedure
**ISO 27001:2022 — Clause 7.5 | Version 1.0**
**Owner:** ISMS Owner

---

## 1. Purpose

To ensure all ISMS documentation is controlled, current, accessible to those who need it, and protected from unauthorized modification.

---

## 2. ISMS Document Store

All ISMS documents are stored in: **[Notion / Confluence / Google Drive / SharePoint — specify]**

- Location: `[ISMS > Documents]` folder / space
- Access: All employees (read); ISMS Owner (write); HR documents restricted to HR + ISMS Owner
- Backup: [Cloud sync / Git-backed]

---

## 3. Document Identification

Every controlled ISMS document must have:

| Field | Example |
|-------|---------|
| Document ID | `ISMS-POL-001` (policies), `ISMS-PROC-001` (procedures), `ISMS-TMPL-001` (templates) |
| Title | Information Security Policy |
| Version | 1.0, 1.1, 2.0 |
| Status | Draft / Under Review / Approved / Retired |
| Owner | Name and role |
| Approved by | Name and role |
| Approval date | YYYY-MM-DD |
| Review date | YYYY-MM-DD |
| Classification | Confidential / Internal |

### ID Naming Convention
```
ISMS-[TYPE]-[NNN]

Types:
  POL  = Policy
  PROC = Procedure
  TMPL = Template
  REG  = Register
  RPT  = Report
  PLAN = Plan
  CHKL = Checklist
```

---

## 4. Version Control

| Version | Meaning |
|---------|---------|
| 1.0 | First approved version |
| 1.1 | Minor update (no change to scope or intent) |
| 1.2 | Further minor update |
| 2.0 | Major revision (significant scope or intent change) |

**Git-based version control (recommended):**
- Store all ISMS documents in a private Git repository
- PRs required for all changes
- Review + approval documented in PR history
- Tags for approved versions (e.g., `v1.0-approved`)

**Non-Git (document store):**
- Version history enabled in Notion/Drive/Confluence
- Previous versions archived (not deleted)
- Version number in document filename: `information_security_policy_v1.0.md`

---

## 5. Document Lifecycle

```
DRAFT → REVIEW → APPROVED → [IN USE] → REVIEW CYCLE → APPROVED v1.1
                                              │
                                              └→ RETIRED (if superseded)
```

### Creating a New Document
1. Use approved template (`ISMS-TMPL-001`)
2. Assign Document ID and fill metadata table
3. Draft content
4. Submit for review to relevant stakeholders
5. Incorporate feedback
6. Submit for approval (sign-off by approver)
7. Publish to ISMS document store
8. Communicate to affected parties (training / email / Slack)
9. Add to ISMS document register

### Updating an Existing Document
1. Check out / copy current version
2. Increment version number (minor or major)
3. Make changes; update "Changed sections" in change log
4. Submit for review (full review if major change; ISMS Owner only for minor)
5. Obtain approval
6. Archive previous version
7. Publish new version and communicate changes

### Retiring a Document
1. Mark as "Retired" in document store
2. Note superseding document (if applicable)
3. Retain archived copy per retention policy (minimum 3 years)
4. Remove from active ISMS document index

---

## 6. Review Schedule

| Document Type | Review Cycle | Trigger Events |
|--------------|-------------|----------------|
| Top-level IS Policy | Annual | Org change; legal change; incident |
| Topic policies | Annual | Org/tech change; incident; audit finding |
| Procedures | Annual | Process change; incident; audit finding |
| Risk register | Annual + on change | New assets; incidents; major changes |
| SoA | Annual + on scope change | New Annex A controls; exclusion changes |
| Templates | Bi-annual | Process improvements |

---

## 7. ISMS Document Register

Master list of all controlled ISMS documents:

| Doc ID | Title | Version | Owner | Approved | Next Review | Location |
|--------|-------|---------|-------|----------|-------------|---------|
| ISMS-001 | ISMS Scope and Manual | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-001 | Information Security Policy | 1.0 | CEO | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-002 | Acceptable Use Policy | 1.0 | HR | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-003 | Access Control Policy | 1.0 | IT Lead | [CTO] [Date] | [Date] | [Link] |
| ISMS-POL-004 | Cryptography Policy | 1.0 | IT Lead | [CTO] [Date] | [Date] | [Link] |
| ISMS-POL-005 | Data Classification Policy | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-006 | Incident Response Policy | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-007 | Supplier Security Policy | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-POL-008 | Change Management Policy | 1.0 | Eng Lead | [CTO] [Date] | [Date] | [Link] |
| ISMS-PROC-001 | Document Control Procedure | 1.0 | ISMS Owner | [ISMS Owner] [Date] | [Date] | [Link] |
| ISMS-PROC-002 | Risk Assessment Methodology | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-REG-001 | Asset Register | 1.0 | IT Lead | [ISMS Owner] [Date] | [Date] | [Link] |
| ISMS-REG-002 | Risk Register | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-REG-003 | Supplier Register | 1.0 | ISMS Owner | [ISMS Owner] [Date] | [Date] | [Link] |
| ISMS-REG-004 | Statement of Applicability | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-PLAN-001 | Risk Treatment Plan | 1.0 | ISMS Owner | [CEO] [Date] | [Date] | [Link] |
| ISMS-PLAN-002 | Internal Audit Programme | 1.0 | ISMS Owner | [ISMS Owner] [Date] | [Date] | [Link] |
| ISMS-RPT-001 | Internal Audit Report [Year] | 1.0 | Internal Auditor | [ISMS Owner] [Date] | Post-next-audit | [Link] |

---

## 8. Document Distribution and Communication

When a new or updated document is approved:
- Post announcement in `#security` or `#all-company` Slack channel
- For policy updates: require re-acknowledgement from all affected staff (tracked in training records)
- For procedure updates: notify affected teams directly

---

## 9. External Documents

Standards, regulations, and external references (ISO 27001 standard, GDPR text, AWS documentation) are not version-controlled by [Organization Name] but must be referenced with the version/date used.
