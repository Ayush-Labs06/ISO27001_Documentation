# Surveillance Audit Preparation
**ISO 27001:2022 — Post-Certification | Version 1.0**
**Owner:** ISMS Owner
**When to use:** 8–12 weeks before Year 1 or Year 2 surveillance audit

---

## What Changes at Surveillance vs. Initial Certification

| Aspect | Initial Certification (Stage 2) | Surveillance Audit (Year 1/2) |
|--------|--------------------------------|-------------------------------|
| Duration | 1.5–3 days | 0.5–1 day (roughly 1/3 of Stage 2) |
| Scope | Full ISMS (all clauses + Annex A) | Focused on: changes since cert, previous findings, high-risk areas |
| Documents reviewed | All mandatory documents | Changed/updated documents; new policies |
| Evidence sampled | Full range | Higher risk areas + any new domains |
| Main question | "Does the ISMS exist and work?" | "Is the ISMS still working and improving?" |
| Outcome | Certificate granted or not | Certificate maintained or suspended |

**Key difference:** The surveillance auditor will focus on what changed since the last audit and whether previous findings were addressed. They will not re-audit everything from scratch — but they can, if they see signs of regression.

---

## What Surveillance Auditors Focus On

### 1. Status of Previous Audit Findings

First thing they check: were the nonconformities from Stage 2 (or last surveillance) closed effectively?
- [ ] All major NCs have verified corrective actions with evidence
- [ ] All minor NCs have corrective actions complete or in progress with documented plan
- [ ] Corrective action tracker is up to date

### 2. Changes Since Last Audit

They want to know what changed and whether those changes were managed properly:
- [ ] New systems or services added to scope — risk assessed?
- [ ] Scope changes documented and CB notified?
- [ ] New suppliers added — security assessed?
- [ ] Significant org changes (headcount growth, new team, acquisition)?
- [ ] New regulations or compliance requirements?
- [ ] Major security incidents — handled per IRP?

### 3. Evidence of Ongoing Operation

Controls that must show 12 months of evidence:
- [ ] Access reviews — completed quarterly (4 review records)
- [ ] Training completion — annual programme complete for all staff
- [ ] Phishing simulations — quarterly (4 simulation records with metrics)
- [ ] Internal audit — completed within the last 12 months
- [ ] Management review — at least 1 completed with CEO attendance
- [ ] Vulnerability scans — ongoing (can show recent scan + evidence of monitoring)
- [ ] Patch compliance — consistent (not just patched the week before the audit)
- [ ] Incident logging — any incidents properly recorded, even P4

### 4. Continual Improvement Evidence

They want to see that the ISMS is improving, not static:
- [ ] IS Policy reviewed and version updated (even minor revision shows it's alive)
- [ ] Risk register updated — new risks added, existing risks reviewed
- [ ] At least some closed nonconformities from the period
- [ ] Management review outputs show decisions and actions were taken

---

## Surveillance Readiness Checklist

**8 weeks before surveillance:**
- [ ] Pull previous audit report — review all findings
- [ ] Run through corrective action tracker — are all NCs closed?
- [ ] Schedule management review if not done in last 6 months
- [ ] Check training completion — is anyone outstanding?
- [ ] Run a phishing simulation if Q4 sim hasn't been done
- [ ] Review access review records — any quarter missing?

**4 weeks before surveillance:**
- [ ] Internal mini-audit: sample 5 controls from each Annex A theme
- [ ] Update risk register (even a review with no changes = evidence of review)
- [ ] Confirm IS Policy version date is within 12 months
- [ ] Check SoA — any new controls to add? Exclusions still valid?
- [ ] Update subprocessor register with any new suppliers

**2 weeks before surveillance:**
- [ ] Refresh evidence pack (same structure as Stage 2 evidence folder)
- [ ] Fresh IAM credential report
- [ ] Recent vulnerability scan results
- [ ] Updated KPI dashboard for the period
- [ ] Confirm all staff interview candidates available on audit day

**Audit day:**
- [ ] Evidence folder accessible (tested)
- [ ] CEO available for 30-minute slot
- [ ] DevOps Lead available for technical walkthrough
- [ ] Previous audit report and CB Stage 2 findings documented responses on hand

---

## Common Surveillance Audit Failures

| Failure | Root Cause | Prevention |
|---------|-----------|-----------|
| No management review in 12 months | Assumed "we'll do it before the audit" | Schedule in January for the year; calendar block |
| Training completion <80% | Training was assigned but not tracked/chased | LMS auto-reminders; HR escalation at 30 days |
| Access reviews skipped (Q2 and Q3) | No one was tracking the schedule | ISMS calendar with quarterly reminders; named owner |
| Open NCs from Stage 2 not closed | NC tracker not maintained post-cert | ISMS Owner reviews tracker monthly; include in mgmt review |
| New critical supplier not assessed | Procurement didn't loop in ISMS | Supplier onboarding checklist requires ISMS Owner sign-off |
| Scope expanded (new product) without update | Engineering launched without IS review | Change management gate: significant change = ISMS review |
| Risk register unchanged from Stage 2 | "It was fine then" | Trigger-based review process + annual review reminder |

---

## If the CB Finds a Major NC at Surveillance

**What it means:** Certificate is at risk of suspension. The CB will set a deadline (typically 90 days) for corrective action. If not resolved: certificate suspended, then withdrawn.

**What to do:**
1. Don't panic — major NCs at surveillance are rare if you maintain the ISMS properly
2. Understand exactly what the finding is — get it in writing
3. Perform root cause analysis (same as for internal NCs)
4. Implement corrective action within the timeline
5. Submit evidence to CB
6. CB may conduct a follow-up review (additional day, sometimes remote)

**Prevention:** The surveillance mini-audit checklist (above) is designed specifically to catch major issues before the CB does.

---

## Recertification (Year 3)

After 3 years, the certificate expires. Recertification is a full audit again — similar scope to the initial Stage 2, though shorter because you have 3 years of ISMS operating history.

Key difference from initial certification: auditors will review 3 years of evidence. A management review from year 1 is in scope. An incident from year 2 is in scope.

**Start recertification prep 6 months before cert expiry:**
- Book CB slot — they get busy; book early
- Conduct a pre-cert internal audit (full scope)
- Ensure 3 years of records are retained and accessible
- Update all policies that haven't been reviewed in 12+ months
