# Gap Analysis Methodology
**ISO 27001:2022 — Pre-implementation baseline assessment**

The gap analysis is the foundation of the entire ISMS project. It tells you where the client is today, what they need to build, and in what order. Done well, it sets the risk treatment priorities and scopes the implementation work. Done poorly, you'll miss critical gaps and get surprised in the internal audit.

---

## Assessment Framework

We use a three-dimensional assessment:

1. **Clause compliance** (Clauses 4–10) — Are the mandatory ISMS processes in place?
2. **Annex A control coverage** (93 controls, 4 themes) — Are the required security controls implemented?
3. **Maturity level** — Not just "exists/doesn't exist" but how effectively it's implemented

---

## Scoring Scale

Use a consistent scale across all items:

| Score | Label | Definition |
|-------|-------|------------|
| 0 | Not implemented | No evidence of this control or process existing |
| 1 | Partially implemented | Exists informally, inconsistently, or undocumented |
| 2 | Implemented | In place but not formally reviewed or measured |
| 3 | Managed | Documented, implemented, and periodically reviewed |
| 4 | Optimized | Measured, continuously improved, with evidence trail |

**For gap analysis purposes, score 0-2 = gap (needs work), 3+ = satisfactory for pre-certification.**

---

## Evidence Quality Tiers

Not all evidence is equal. When scoring, note the evidence quality:

| Tier | Type | Auditor weight |
|------|------|---------------|
| A | Policy + documented procedure + implementation + records | Strong |
| B | Policy + some implementation evidence, no consistent records | Moderate |
| C | Informal/verbal practice, no documentation | Weak — gap |
| D | Awareness of requirement only, nothing done | Gap |

---

## Data Collection Methods

Use all three — never rely on self-assessment alone:

### 1. Interviews (primary method)
Structured conversations with: CEO/CTO (management commitment, risk tolerance), IT/Infra lead (technical controls), HR (training, onboarding/offboarding), a sample of developers (security awareness, practices)

**Interview guide questions:**
- "Walk me through what happens when a new employee joins from a systems access perspective."
- "What happens when someone leaves? How quickly does their access get revoked?"
- "If there was a security incident tonight, who would you call and what would you do?"
- "How do you know when a system has a critical vulnerability?"
- "How are changes to production deployed?"
- "Where is customer data stored? Who can access it?"

### 2. Document Review
Request and review:
- [ ] Org chart
- [ ] Any existing security policies
- [ ] Cloud account architecture diagram (or create one)
- [ ] AWS/Azure console screenshots (or export of IAM users, MFA status, logging config)
- [ ] Any previous audit reports or pentest reports
- [ ] Employee training records
- [ ] Incident log (even if informal)
- [ ] Supplier/vendor list
- [ ] Backup job configs and last restore test date

### 3. Technical Observation
- Log into the AWS console (read-only) and check: IAM users, MFA, CloudTrail, S3 buckets, Security Hub
- Run `aws iam generate-credential-report` to see all IAM users and MFA status
- Check if GuardDuty is enabled
- Review a sample of CI/CD pipelines for secret scanning, SAST, image scanning
- Check if employee laptops have MDM enrolled (ask to see Jamf/Intune console)

---

## Gap Analysis Workflow

```
Step 1: Schedule 3-4 hours of client time
         │
Step 2: Conduct interview sessions
         │
Step 3: Collect and review documents
         │
Step 4: Perform technical observation (with read-only access)
         │
Step 5: Score each clause and control
         │
Step 6: Map scores to implementation effort
         │
Step 7: Produce gap analysis report with:
         ├── Executive summary (RAG heat map)
         ├── Clause findings
         ├── Annex A findings
         ├── Quick wins (can close in <2 weeks)
         ├── Medium-term remediation (1-3 months)
         └── Long-term / complex (3-6 months)
```

---

## Prioritization Framework

After scoring, prioritize gaps by:

**Priority 1 — Must Fix Before Stage 2** (Auditors will look for these)
- No audit logging / CloudTrail not enabled
- No MFA on privileged accounts
- Shared credentials / no individual accounts
- No incident response procedure
- No formal risk assessment
- Critical vulnerabilities known but not patched

**Priority 2 — Must Fix Before Stage 2 (but can be later in project)**
- Missing policies (they can be written)
- No training records
- Supplier register incomplete
- No access review evidence

**Priority 3 — Observations (auditor may raise as minor)**
- Policies not reviewed on schedule
- Informal processes that work but aren't documented
- KPIs not yet defined

---

## Output: Gap Analysis Report Sections

1. Executive Summary with RAG dashboard
2. Methodology
3. Clause 4-10 findings table
4. Annex A findings table
5. Priority remediation roadmap
6. Quick wins list
7. Resource and effort estimate
8. Appendix: evidence reviewed

See `gap_analysis_report_template.md` for the full report format.
