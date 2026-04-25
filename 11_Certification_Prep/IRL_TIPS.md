# IRL Tips — Certification Preparation

---

## 1. Choose the Right Certification Body Before Stage 1

Not all certification bodies are equal. For startups, the main considerations:

**Price:** Varies significantly. Get 3 quotes. BSI is usually most expensive; smaller accredited CBs can be 40–60% cheaper with the same UKAS/IAF accreditation.

**Reputation with customers:** Some enterprise customers (especially US enterprise) prefer UKAS-accredited CBs with name recognition (BSI, SGS, Bureau Veritas, LRQA). If your customers care which CB issued the cert, factor that into selection.

**Availability:** Stage 2 auditors who understand SaaS and cloud are in demand. Book 6–8 weeks ahead. Don't book Stage 2 until Stage 1 findings are resolved.

**Remote vs. on-site:** Most CBs now do remote Stage 2 audits for cloud-native companies. This is fine — the audit quality is the same and it saves travel costs.

**Key check:** Is the CB accredited by a recognized IAF member body (UKAS in UK, ANAB in US, DAkkS in Germany)? Accreditation is what makes the certificate globally recognized. Non-accredited certificates are essentially worthless for procurement purposes.

---

## 2. The SoA Is the Single Most Important Document — Treat It That Way

If you only polish one document before Stage 1, polish the SoA. It tells the auditor:
- Which controls apply to your organization
- Why you excluded what you excluded
- How well-implemented your included controls are

Common SoA failures:
- All 93 controls "included" with no exclusions — looks like a template was filled in without thought
- Exclusions with justifications like "not applicable" or "N/A" (these are not justifications)
- Status column blank or everything showing as "Planned" (shows nothing is actually implemented)
- Version is "1.0" but no review date — suggests it was created once and never touched

Good SoA exclusions with real justifications: "A.7.11 (Supporting utilities) — Excluded. Organization operates exclusively on AWS managed infrastructure. Physical supporting utilities (power, cooling) are managed by AWS under the shared responsibility model and evidenced by AWS's ISO 27001 certification."

---

## 3. Stage 1 Gaps Are Your Friend — Don't Hide Them

Stage 1 is specifically designed to find problems before Stage 2. Some consultants try to present a perfect picture at Stage 1 to avoid Stage 1 findings. This is a mistake.

A Stage 1 finding means the CB gives you 4–8 weeks to fix it before Stage 2. A Stage 2 finding on the same issue means a nonconformity against a live audit, a corrective action deadline, and potentially a delayed certificate.

Use Stage 1 as a quality check. If the auditor finds something at Stage 1 that you should have caught: good — better now than later.

---

## 4. Evidence Must Be Accessible in Under 5 Minutes

Nothing unsettles an auditor faster than "let me find that... it's somewhere in our Notion... give me a minute..." 20 minutes into an evidence request. Auditors take note — it suggests the organization doesn't actually operate its ISMS day-to-day, they just scrambled to create documents before the audit.

Build the evidence folder structure (see `evidence_collection_guide.md`) 4 weeks before Stage 2. Run a mock audit internally: "Auditor asks to see the last access review. How long does it take me to pull it up?" If the answer is >2 minutes, reorganise.

For remote audits: have your screen share ready with the evidence folder open. Don't rely on search finding the right document — have it pre-opened in tabs.

---

## 5. Surveillance Audits Are Where Clients Fail After Getting Certified

Many clients treat certification as the finish line. It isn't — it's year 1 of a 3-year cycle. The Year 1 surveillance audit (typically 12 months after certification) catches clients who:
- Didn't complete the management review
- Let training completion slip
- Forgot to do access reviews for 2 quarters
- Added a major new supplier without a security assessment
- Had a significant organizational change without updating the scope

Brief clients on this immediately after they get the certificate. The surveillance prep folder (`12_Post_Certification/surveillance_audit_prep.md`) is specifically for this.

---

## 6. The Certification Body Auditor Is Not the Enemy

This seems obvious but some clients treat the audit as adversarial. The best outcomes come from treating the auditor as a knowledgeable peer whose job is to help verify that your controls work. If something isn't clear, explain it. If a control is new and not yet fully tested, say so — and show the plan.

Auditors find major NCs when they sense something is being hidden or when evidence appears freshly manufactured. They're experienced enough to tell the difference between a mature ISMS and a last-minute document sprint. What builds confidence: genuine conversation, real records with real dates, staff who actually know the policies without being coached on specific answers.

---

## 7. Don't Overbuild — Certification Is a Baseline, Not a Gold Standard

A common mistake: adding controls that go far beyond what's needed for certification, driven by the consultant's thoroughness or the client's anxiety. This creates maintenance burden. Controls that exist on paper but aren't operated are evidence of a gap, not a strength.

ISO 27001 auditors are looking for: "Does this control exist and is it working?" Not: "Is this control state of the art?" A simple, consistently-operated access review is better evidence than an elaborate PAM system that nobody uses because it's too complex.

Scope, then build. Certify what you have. Expand the scope and add controls over time as the organization matures.
