# IRL Tips — Post-Certification

---

## 1. The Certificate Is the Start of the Work, Not the End

The most common mistake after certification: treating the ISO 27001 certificate as a destination. It isn't. It's entry to a 3-year maintenance cycle. The controls that got you certified need to be operated continuously — access reviews every quarter, training completion every year, internal audit every 12 months.

Clients who treat certification as "done" fail their first surveillance audit. Set expectations on engagement day: "We'll get you certified in Month X. Then I'll support you for 3 months of steady-state operation, and we'll check in before your Year 1 surveillance."

The calendar in this folder is the tool for preventing the "we forgot to do Q3 access reviews" problem. Set recurring calendar reminders on Day 1 post-certification.

---

## 2. Surveillance Audits Are Shorter But Sharper

Year 1 and Year 2 surveillance audits are typically 30–50% the duration of Stage 2. But they're not easier — they're more focused. The CB auditor knows where the weaknesses were. They'll go straight to:
- The specific finding from Stage 2 and whether the corrective action stuck
- The areas where your organization is most likely to slip (usually access reviews and training)
- Evidence that things have been running continuously — not just in the month before the audit

A common trap: "We've been doing everything correctly, we just haven't been documenting it." That's not a compliance success — that's an evidence gap. Undocumented controls don't exist for audit purposes.

---

## 3. Scope Changes Must Be Notified to the CB

If your ISMS scope changes significantly — new product line, new cloud region, major headcount growth, acquisition — the certification body needs to know. Depending on the change, they may require:
- A scope extension audit (additional day)
- Notification only (if the new area is materially similar to what's already certified)
- A new certification (if it's fundamentally different scope)

What constitutes "significant" is a CB judgement call. When in doubt, tell them. A 30-minute call with the CB to discuss a scope change is far better than a surveillance auditor discovering an unnotified scope expansion.

For clients: put "scope change? Notify ISMS Owner" in the change management policy and in the product launch checklist.

---

## 4. Control Decay Is the Silent Killer

Controls that worked on certification day gradually erode without active maintenance. Common decay patterns:
- Access review: done on time in Q1, Q2, skipped in Q3 because "busy," Q4 is rushed
- Patch compliance: enforced until the DevOps Lead goes on holiday, then manual patching stops
- Training: annual course completed, but new hires in months 6–11 haven't been trained
- Supplier assessments: done for the audit, never updated when the supplier has a major incident
- Risk register: dated the month of Stage 2, not touched since

The metrics dashboard and annual calendar are designed to make decay visible before the auditor does. If a metric starts sliding — flag it in the monthly dashboard immediately, don't wait for the quarterly management review.

---

## 5. Selling the Certificate (Without Overselling It)

ISO 27001 certification is a significant commercial differentiator for B2B SaaS, especially with enterprise and regulated-industry buyers. Help clients extract business value from it:

- **Trust page / security page:** Public-facing page listing: ISO 27001 certified, certificate number, CB name, scope, expiry date, and a PDF download link
- **Sales enablement:** Certificate PDF for procurement questionnaire responses; answers ~60% of security questionnaire questions
- **Vendor portal / RFP responses:** "We are ISO 27001:2022 certified by [CB name]; certificate number [X]; scope: [Y]" — one line that bypasses pages of a security questionnaire
- **Customer security one-pager:** 1-page document covering: certification, data handling practices, key controls, incident response SLAs, GDPR compliance position

What to avoid: overclaiming. "ISO 27001 certified" means the ISMS has been independently audited and found conformant. It does not mean the company has never had a security incident, or that its software is secure, or that customer data is 100% safe. Sophisticated security buyers know this distinction. Naive overclaiming undermines trust with the ones who matter most.

---

## 6. Year 2 Maturity vs. Year 1 Compliance

Year 1 post-certification should shift the conversation from "are we compliant?" to "is our security programme actually working?" The metrics that matter in Year 2 are outcome metrics, not process metrics:
- **Outcome:** Mean time to detect/respond to incidents is decreasing
- **Process:** We ran 4 phishing simulations (that's just a process metric)
- **Outcome:** Phishing click rate dropped from 15% to 7% year-over-year
- **Process:** We completed all 4 quarterly access reviews
- **Outcome:** 0 former employee accounts found active at any access review

Help clients transition to this framing at the Year 1 management review. It builds a genuine security culture and makes Year 2 surveillance evidence much stronger.
