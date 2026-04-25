# ISO 27001:2022 Gap Analysis Report
**Client:** [Organization Name]
**Prepared by:** [Your name]
**Date:** YYYY-MM-DD
**Version:** 1.0 — Confidential

---

## Executive Summary

[Organization Name] underwent an ISO 27001:2022 gap analysis assessment conducted on [date range]. The assessment covered [scope summary] and evaluated compliance against:
- ISO 27001:2022 mandatory clauses (Clauses 4–10)
- All 93 Annex A controls (4 themes: Organizational, People, Physical, Technological)

### Overall Readiness Score

| Domain | Score | Status |
|--------|-------|--------|
| Clause 4–10 Compliance | __% | 🔴 / 🟡 / 🟢 |
| Annex A Control Coverage | __% | 🔴 / 🟡 / 🟢 |
| **Overall** | **__%** | **🔴 / 🟡 / 🟢** |

### Heat Map — Clause Readiness

```
Clause 4  Context         [████░░░░░░] 40%  🟡
Clause 5  Leadership      [██░░░░░░░░] 20%  🔴
Clause 6  Planning        [█░░░░░░░░░] 10%  🔴
Clause 7  Support         [███░░░░░░░] 30%  🔴
Clause 8  Operation       [████░░░░░░] 40%  🟡
Clause 9  Performance     [██░░░░░░░░] 20%  🔴
Clause 10 Improvement     [██░░░░░░░░] 20%  🔴
```
*(Replace with actual scores)*

### Heat Map — Annex A Themes

```
A.5 Organizational  [████░░░░░░] 40%  🟡
A.6 People          [████████░░] 80%  🟢
A.7 Physical        [██████░░░░] 60%  🟡
A.8 Technological   [███░░░░░░░] 30%  🔴
```

### Key Findings

**Critical Gaps (must resolve before certification):**
1. [e.g., No audit logging enabled in cloud environments — A.8.15]
2. [e.g., MFA not enforced for privileged accounts — A.8.5]
3. [e.g., No formal risk assessment conducted — Clause 6.1.2]
4. [e.g., No incident response procedure — A.5.24]
5. [e.g., No Statement of Applicability — Clause 6.1.3]

**Quick Wins (can close within 2 weeks):**
1. [e.g., Enable CloudTrail in all regions — 1 day effort]
2. [e.g., Enable GuardDuty — 2 hours effort]
3. [e.g., Enforce MFA on AWS root and admin accounts — 1 day]
4. [e.g., Document the IS Policy (template provided) — 2 days]

---

## Methodology

This assessment was conducted using:
- Interviews with: [list roles e.g., CTO, DevOps Lead, HR Manager]
- Document review: [list docs reviewed e.g., org chart, existing policies, CI/CD pipeline configs]
- Technical observation: [e.g., AWS console review with read-only access, GitHub org settings]
- Scoring scale: 0 (not done) → 1 (partial) → 2 (implemented) → 3 (managed) → 4 (optimized)

Assessment period: [date] to [date]
Assessment location: [on-site / remote]

---

## Clause 4–10 Findings

### Clause 4 — Context of the Organization (Score: __/32)

**Findings:**
- [e.g., No documented analysis of internal/external context issues (Clause 4.1). The organization operates in a regulated sector but has not formally documented the relevant legal/regulatory requirements that affect the ISMS.]
- [e.g., Interested parties have not been formally identified or documented (Clause 4.2). Customer security requirements are understood informally but not captured.]
- [e.g., An ISMS scope exists informally but has not been documented or approved (Clause 4.3).]

**Recommended Actions:**
- [ ] Document internal and external context (can be integrated into ISMS Manual)
- [ ] Produce interested parties register with their requirements
- [ ] Draft, review, and approve formal ISMS scope statement

---

### Clause 5 — Leadership (Score: __/72)

**Findings:**
- [e.g., No formal Information Security Policy exists (Clause 5.2). The CTO informally communicates security expectations, but there is no signed, approved policy.]
- [e.g., ISMS roles and responsibilities have not been formally assigned or communicated (Clause 5.3).]

**Recommended Actions:**
- [ ] Draft and obtain CEO approval for Information Security Policy
- [ ] Define and document ISMS roles (RACI)

---

### Clause 6 — Planning (Score: __/80)

**Findings:**
- [e.g., No formal risk assessment has been conducted (Clause 6.1.2). The team has awareness of risks but no documented methodology, asset register, or risk register.]
- [e.g., No Statement of Applicability exists (Clause 6.1.3). This is a mandatory document and is not present.]
- [e.g., No IS objectives have been defined (Clause 6.2).]

**Recommended Actions:**
- [ ] Define risk assessment methodology and acceptance criteria
- [ ] Conduct asset inventory and risk assessment
- [ ] Produce Statement of Applicability
- [ ] Define measurable IS objectives

---

### Clause 7 — Support (Score: __/60)

**Findings:**
- [e.g., No formal security awareness training programme (Clause 7.2/7.3). New employees receive informal security guidance but there are no training records.]
- [e.g., No document control procedure (Clause 7.5). Documents are stored ad-hoc with no version control or review cycle.]

**Recommended Actions:**
- [ ] Establish security training programme with attendance records
- [ ] Implement document control procedure (versioning, review dates, owners)

---

### Clause 8 — Operation (Score: __/32)

**Findings:**
- [e.g., Security controls are being implemented but without a formal plan tied to risk treatment decisions (Clause 8.1).]
- [e.g., No periodic risk assessments are scheduled (Clause 8.2). Risk assessment will be conducted for the first time as part of this engagement.]

---

### Clause 9 — Performance Evaluation (Score: __/60)

**Findings:**
- [e.g., No internal audit has been conducted (Clause 9.2). This is a mandatory activity before certification.]
- [e.g., No management review of the ISMS has taken place (Clause 9.3). This is mandatory.]
- [e.g., No IS metrics or KPIs are being tracked (Clause 9.1).]

---

### Clause 10 — Improvement (Score: __/28)

**Findings:**
- [e.g., No nonconformity or corrective action process exists (Clause 10.2). Security issues are addressed ad-hoc with no formal recording or root cause analysis.]

---

## Annex A Findings — Top Gaps by Theme

### A.5 Organizational Controls — Top Gaps

| Control | Finding | Priority |
|---------|---------|---------|
| A.5.9 Asset inventory | No asset register exists | Critical |
| A.5.12 Data classification | No classification scheme defined | High |
| A.5.19 Supplier relationships | Vendor security not formally assessed | High |
| A.5.23 Cloud services | No cloud security policy or baseline | Critical |
| A.5.24 Incident management | No IRP documented | Critical |

### A.6 People Controls — Top Gaps

| Control | Finding | Priority |
|---------|---------|---------|
| A.6.3 Security training | No formal programme, no records | High |
| A.6.7 Remote working | No remote working security policy/guidance | Medium |
| A.6.8 Event reporting | No clear channel for staff to report incidents | High |

### A.7 Physical Controls — Top Gaps

| Control | Finding | Priority |
|---------|---------|---------|
| A.7.2 Physical entry | No visitor log at office | Medium |
| A.7.7 Clear desk/screen | Policy not communicated; screen lock not enforced | Medium |
| A.7.14 Secure disposal | No procedure for device disposal | Medium |

### A.8 Technological Controls — Top Gaps

| Control | Finding | Priority |
|---------|---------|---------|
| A.8.2 Privileged access | Root/admin accounts used for daily work | Critical |
| A.8.5 Authentication | MFA not enforced on all critical systems | Critical |
| A.8.8 Vulnerability mgmt | No vulnerability scanning programme | Critical |
| A.8.9 Configuration mgmt | No baseline configurations or IaC state management | High |
| A.8.15 Logging | CloudTrail not enabled in all regions; no central log store | Critical |
| A.8.16 Monitoring | No alerting on security events | High |
| A.8.25 Secure SDLC | No SAST/SCA in CI/CD pipeline | High |

---

## Remediation Roadmap

### Phase 1 — Foundation (Months 1-3)
*Required before risk treatment decisions can be made*

| Action | Owner | Effort | Month |
|--------|-------|--------|-------|
| Enable CloudTrail + GuardDuty + Security Hub (AWS) | IT Lead | 1 day | 1 |
| Enable MFA on all AWS accounts, GitHub, Google Workspace | IT Lead | 2 days | 1 |
| Create asset register (start with critical assets) | IT Lead + Dept Leads | 1 week | 1 |
| Define ISMS scope statement | Consultant + CTO | 1 day | 1 |
| Draft IS Policy (top-level) + get CEO approval | Consultant | 2 days | 1 |
| Define risk methodology | Consultant | 2 days | 2 |
| Conduct risk assessment | Consultant + IT Lead | 2 weeks | 2-3 |
| Produce SoA | Consultant | 1 week | 3 |

### Phase 2 — Documentation (Months 3-5)
| Action | Owner | Effort | Month |
|--------|-------|--------|-------|
| Write full policy suite (8-10 policies) | Consultant | 3 weeks | 3-4 |
| Implement document control | Client | 1 week | 4 |
| Supplier register + critical vendor assessments | Client | 2 weeks | 4-5 |
| Training programme launch + first cycle | HR + Consultant | 2 weeks | 5 |
| Onboarding/offboarding procedure | HR + IT | 1 week | 5 |

### Phase 3 — Technical Controls (Months 4-6)
| Action | Owner | Effort | Month |
|--------|-------|--------|-------|
| Cloud security baseline (AWS CIS) | IT Lead | 3 weeks | 4-5 |
| Vulnerability management programme | IT Lead | 2 weeks | 5 |
| Patch management procedure | IT Lead | 1 week | 5 |
| Backup testing | IT Lead | 1 week | 5 |
| DevSecOps integration (SAST, secret scan) | Dev Lead | 2 weeks | 5-6 |
| Incident response procedure + tabletop | Consultant + CTO | 1 week | 6 |

### Phase 4 — Audit & Certification (Months 7-10)
| Action | Owner | Effort | Month |
|--------|-------|--------|-------|
| Internal audit | Consultant | 2 weeks | 7 |
| Close NCs | Client + Consultant | 4 weeks | 8 |
| Management review | CTO/CEO | 2 hours | 8 |
| Stage 1 audit | CB + Consultant | 1-2 days | 9 |
| Stage 1 findings closed | Client | 4 weeks | 9-10 |
| Stage 2 audit | CB + Consultant | 2-3 days | 10-11 |

---

## Appendix: Documents Reviewed

| Document | Provided By | Date | Notes |
|----------|------------|------|-------|
| | | | |

## Appendix: Interviewees

| Name | Role | Date | Duration |
|------|------|------|---------|
| | | | |
