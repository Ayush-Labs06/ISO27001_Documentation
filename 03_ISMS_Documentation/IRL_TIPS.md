# IRL Tips — ISMS Documentation & Policies

---

## 1. Policies Must Be Tailored — Auditors Spot Copy-Paste Immediately

Every auditor has seen the same 10 policy templates. If your policy says "the IT department" but the company has no IT department, or references "data centres" when they're 100% cloud, the auditor will ask pointed questions. Customize:
- Company name throughout (not just in the header)
- Specific tool names where relevant ("Azure Entra ID", "GitHub", "AWS KMS")
- Role names that match the org chart (not "CISO" if there is no CISO)
- Exception processes that are actually workable for the client's size

---

## 2. "Approved" Means Signed by Top Management — Not Just the CTO

Clause 5.2 requires the Information Security Policy to be "approved by top management." For a startup, top management = CEO. The CEO must physically sign or digitally approve the IS Policy. A CTO approval is not sufficient. This comes up in Stage 2 audits.

Get the CEO signature as a PDF and store it with the policy. For everything else, CTO/ISMS Owner approval is fine.

---

## 3. The 11 Mandatory Documents — Know Them Cold

Auditors start Stage 1 by checking for the 11 mandatory documented items (Clause 7.5). If any are missing, it's a Major Nonconformity and Stage 2 will be delayed:

1. ISMS Scope
2. IS Policy
3. Risk Assessment Methodology
4. Risk Register
5. Risk Treatment Plan
6. Statement of Applicability
7. IS Objectives
8. Evidence of Competence (training records)
9. Internal Audit Programme + Results
10. Management Review Results
11. NC + Corrective Action records

Have these ready as a dossier before Stage 1. You want to be able to hand the auditor a folder (physical or shared link) with all 11 documents in under 60 seconds.

---

## 4. Document Control Is Where Many Startups Fall Down

The most common minor NC at Stage 2: documents without proper version numbers, review dates, or owner names. The fix is 30 minutes of admin. Do it before the audit.

Practical minimum: Every document must have in its header:
- Version (1.0, 1.1...)
- Date approved
- Owner name
- Next review date
- Approved by

That's it. Even a Notion page with these 5 fields in a table at the top satisfies clause 7.5.

---

## 5. Policy Tone for Startups — Be Directive, Not Bureaucratic

Startup culture rebels against corporate-sounding policies. Write policies that:
- Are direct and readable (no "the incumbent shall ensure...")
- Match the company's actual communication style
- Have practical examples where helpful ("e.g., don't install Zoom on your company laptop without IT approval")
- Are short — 2-3 pages max per policy; auditors don't read 20-page policies carefully anyway

The goal is policies that employees actually read and follow, not policies that only exist to satisfy auditors.

---

## 6. Change Management Policy ≠ Bureaucracy

Clients will resist a change management policy because they think it means a change advisory board reviewing every PR. It doesn't. Frame it as:
- "Your PR process IS your change control" — just document it
- The policy formalizes what you already do: PR, CI/CD, review, deploy
- The additional step: a checkbox in the PR template for security-relevant changes

Security-relevant changes (IAM, network config, encryption) get 1 extra reviewer. That's the overhead. Everything else is unchanged.

---

## 7. Policies Need Review Cycles — Build a Calendar

The most common finding at Year 1 surveillance: "Policies were approved 13 months ago with no evidence of review." The fix is a calendar entry. Literally. Add a recurring calendar event for each policy's review anniversary and assign it to the owner.

Better: a single annual ISMS document review calendar (see `12_Post_Certification/annual_review_calendar.md`) that bundles all reviews into Q1 of each year. One management review + all policies reviewed in the same sitting.

---

## 8. Confidentiality Agreements — Often Missing for Contractors

A.6.6 requires confidentiality/NDA agreements for all employees and contractors who handle sensitive information. Most startups have NDAs in employment contracts but forget about:
- Freelancers engaged ad-hoc (Upwork, Toptal)
- Agency contractors
- Pen testers and auditors (your own NDA with them)
- Consulting firms with access to your systems

Check every contractor in the access review and cross-reference against signed NDA records. Any without an NDA = gap.
