# Continuous Improvement Plan
**ISO 27001:2022 — Clause 10.2 | Version 1.0**
**Owner:** ISMS Owner
**Updated:** [Date]

---

## Purpose

Clause 10.2 requires the organization to continually improve the suitability, adequacy, and effectiveness of the ISMS. This backlog is the operational tool for tracking improvement items from all sources — audit findings, incident lessons, management review decisions, and ISMS Owner observations.

---

## Improvement Sources

| Source | How Items Enter the Backlog |
|--------|--------------------------|
| Internal audit findings (observations) | Auditor raises in audit report; ISMS Owner adds to backlog |
| Management review decisions | Actions from review minutes with "improvement" category |
| Post-incident review lessons | Lessons learned section of incident log |
| Phishing simulation trends | ISMS Owner analysis of declining/plateau metrics |
| Vulnerability management gaps | DevOps Lead flags control gaps |
| New threat intelligence | ISMS Owner review of threat landscape |
| Staff suggestions | Via `security@[company.com]` or `#security-improvements` Slack channel |
| CB audit recommendations | Observations raised at surveillance or recertification |

---

## Improvement Backlog

| # | Item | Source | Priority | Owner | Target Quarter | Status | Completed |
|---|------|--------|---------|-------|---------------|--------|-----------|
| 1 | Automate Route 53 health check failover for DR | DR test finding | High | DevOps Lead | Q1 [Year] | In Progress | |
| 2 | Implement Secrets Manager auto-rotation for all Tier 1 secrets | Audit observation | High | DevOps Lead | Q1 [Year] | Not started | |
| 3 | Add DLP policy for sensitive data in M365 (Purview) | Risk register R-015 | Medium | IT Lead | Q2 [Year] | Not started | |
| 4 | Automate access review workflow (reduce manual effort) | Management review | Medium | IT Lead | Q2 [Year] | Not started | |
| 5 | Enable S3 Object Lock on all evidence and audit log buckets | Audit observation | High | DevOps Lead | Q1 [Year] | Not started | |
| 6 | Implement SBOM generation in CI/CD (Syft + Grype) | A.5.21 supply chain | Medium | Engineering Lead | Q2 [Year] | Not started | |
| 7 | Data masking implementation for staging database | Tabletop exercise finding | High | DevOps Lead | Q1 [Year] | In Progress | |
| 8 | Formalise threat intelligence process (weekly GuardDuty + RSS review) | Audit observation | Low | ISMS Owner | Q3 [Year] | Not started | |
| 9 | Create pre-prepared customer security one-pager | Supplier tip / sales enablement | Low | ISMS Owner | Q3 [Year] | Not started | |
| 10 | Annual third-party penetration test (schedule and procure) | A.5.35 | High | CTO | Q2 [Year] | Not started | |

---

## Priority Definitions

| Priority | Definition | Target Timeline |
|---------|-----------|----------------|
| **Critical** | Control gap that could directly enable a security incident | Immediate (<30 days) |
| **High** | Significant gap; in scope of next surveillance audit | Within 1 quarter |
| **Medium** | Meaningful improvement; not immediately exploitable | Within 2 quarters |
| **Low** | Nice-to-have; future maturity milestone | Within 12 months |

---

## Completed Improvements (Archive)

| # | Item | Completed Date | Evidence |
|---|------|--------------|---------|
| | | | |

---

## Improvement Metrics (Report at Management Review)

| Metric | Q1 | Q2 | Q3 | Q4 | Target |
|--------|----|----|----|----|--------|
| Items in backlog | | | | | ≤20 (well-managed backlog) |
| High priority items open | | | | | 0 beyond 1 quarter |
| Items completed this quarter | | | | | Positive trend |
| Average age of open items (days) | | | | | <90 days average |

---

## ISMS Maturity Roadmap

Beyond certifying individual controls, track ISMS maturity over time:

### Year 1 (Certification)
- All mandatory controls implemented
- Core processes operational
- Documentation in place
- Certificate achieved

### Year 2 (Embedding)
- Processes running consistently (evidence accumulating)
- Metrics trending positively
- Staff genuinely security-aware (not just trained once)
- Supplier management mature
- First surveillance passed cleanly

### Year 3 (Optimising)
- Automation reducing manual compliance overhead
- Advanced controls implemented (threat intelligence, DLP, UEBA)
- DevSecOps fully integrated
- Continuous control monitoring (AWS Config rules, Azure Policy)
- Recertification straightforward

### Year 4+ (Excellence)
- ISMS self-sustaining without heavy consultant involvement
- Security culture embedded in engineering decisions
- Risk register drives roadmap decisions
- Potential: SOC 2 Type II, ISO 27701 (privacy), FedRAMP (if US govt)
