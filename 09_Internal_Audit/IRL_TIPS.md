# IRL Tips — Internal Audit

---

## 1. The Auditor Independence Problem at Startups

ISO 27001 requires the internal auditor to be independent — they cannot audit their own work. At a 10-person startup, this is structurally impossible if ISMS Owner = CTO = the person who implements everything.

**Practical solutions (in order of preference):**
1. Use the consultant (you) as internal auditor for the first cycle — document in the audit plan that the consultant serves as independent internal auditor
2. Cross-audit: Engineering Lead audits HR/legal processes; ISMS Owner audits engineering (only if they didn't implement those specific controls)
3. External internal auditor service: some ISO consultancies offer "internal audit as a service" for a day or two annually

**What to document:** Independence rationale in the audit plan — "Auditor [Name] has no responsibility for the processes being audited in this engagement." Auditors want to see you thought about it.

---

## 2. How to Write a Nonconformity Correctly

A nonconformity report fails if it's too vague or if it confuses symptom with root cause.

**Bad NC:** "The organization does not adequately manage access control."

**Good NC:** "Three of the ten IAM user accounts sampled (IAM credential report dated 2024-09-10) had not been reviewed as part of the Q3 access review. The access control policy (v1.2, Section 4.3) requires quarterly access reviews to be completed by the last day of each quarter. The Q3 review was not completed."

The good NC has:
- A specific, factual observation (not an opinion)
- A specific reference to the requirement (policy, clause, control)
- Objective evidence (what was checked, sample size, date)

This matters because the certification body auditor will read your internal NCs and judge whether your internal audit programme is credible.

---

## 3. Major vs. Minor — The Distinction That Matters for Certification

Getting the grading right matters. A major NC in your internal audit isn't automatically fatal — it shows you found a real problem and fixed it. But grading a major as a minor is worse than the NC itself, because it signals your internal audit isn't finding real issues.

**Call it Major if:**
- The requirement is completely absent (no process, no record, no evidence of ever doing it)
- Personal data is at risk right now (unencrypted DB, public S3 bucket)
- There's been a systemic failure (all of Q3 and Q4 access reviews missed, not just one)

**Call it Minor if:**
- The process exists and is mostly working but had an isolated lapse
- Documentation is out of date but practice is correct
- A required review was done but not documented properly

---

## 4. Auditors Will Walk the Entire Corrective Action Tracker

The certification body auditor doesn't just look at your current audit findings. They'll pull up your corrective action tracker and look at everything from the past cycle:
- Were NCs closed on time?
- Was root cause actually investigated (not just the symptom fixed)?
- Did the same NC come back in the next audit? (A repeated NC is a red flag)
- Are there overdue NCs with no escalation?

The worst thing to show: a list of NCs with "closed" status but no verification date and no documented evidence of closure. Closed means verified by the auditor — not "the process owner said it's fixed."

---

## 5. The Internal Audit Is Your Dress Rehearsal

Frame it this way to clients: the internal audit is the only time you can find a major nonconformity without consequences. The certification body finding a major NC means a finding, a re-audit, and potentially a delayed certificate. Your internal auditor finding the same issue gives you 60–90 days to fix it before anyone external sees it.

This means: don't sandbag the internal audit. Go looking for real problems. The most valuable internal audit reports are the ones with real findings.

---

## 6. What Gets Sampled by Certification Body Auditors

Certification body auditors typically sample:
- **Access reviews:** "Show me the last completed access review for the production environment." → They'll check it's signed, covers all users, action items were followed up.
- **Training records:** "Show me training completion for 5 random employees." → They'll check the dates match the programme plan, the pass marks are recorded.
- **Incident logs:** "Show me all incidents in the last 12 months." → They check: severity correct? Timelines followed? Post-incident review done? Lessons learned documented?
- **Supplier assessments:** "Show me your Tier 1 supplier assessments." → They'll check each one has a DPA, a risk assessment, and evidence of periodic review.
- **Change management:** "Walk me through a significant change that happened recently." → They'll check the change was reviewed, security was considered, rollback was documented.

For each of these, make sure the evidence exists and is accessible in under 5 minutes during the audit. Nothing makes an auditor nervous faster than "let me just find that..."

---

## 7. Audit Checklists vs. Audit Evidence

The checklist (`audit_checklist_clauses.md`, `audit_checklist_annex_a.md`) tells you what to look for. The audit report documents what you found. Audit evidence is the actual records proving compliance.

Don't confuse having a completed checklist with having completed the audit. The checklist is a working document — the report and the referenced evidence records are the official output.

Keep raw evidence (screenshots, log exports, sampled records) in a separate folder: `Records/Audits/IA-[YYYY]-[NNN]-Evidence/`. This is what auditors want to see, not your working notes.
