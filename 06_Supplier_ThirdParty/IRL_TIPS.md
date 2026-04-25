# IRL Tips — Supplier & Third-Party Management

---

## 1. AWS/Azure/GitHub Are Suppliers — Don't Skip Them

Every audit: "Do you have AWS in your supplier register?" The answer must be yes. These are your most critical suppliers. They're easy to document though:
- Download their ISO 27001 certificate (AWS: aws.amazon.com/compliance/iso-27001-faqs; Azure: microsoft.com/en-us/trust-center)
- Document the shared responsibility model
- No questionnaire needed — certificate + SRM is sufficient
- DPA: use AWS's Data Processing Addendum (online click-through); same for Azure

---

## 2. SaaS Stack Sprawl Is a Supplier Management Nightmare

The average startup with 30 employees uses 50+ SaaS tools. You cannot questionnaire all of them. Practical tiering:
- **Critical (top 5-10):** Full questionnaire + DPA + contract clauses
- **Important (next 10-15):** Accept ISO/SOC 2 cert + abbreviated questionnaire
- **Standard (everything else):** Document name, service, data type, and whether a DPA was clicked through

The subprocessor register is your evidence for A.5.19 and A.5.22. It doesn't need to be a detailed report — a spreadsheet with the right fields and review dates is sufficient.

---

## 3. The 24-Hour Incident Notification Is Negotiable in Contracts

Many vendors won't agree to 24-hour breach notification — their standard is 72 hours or "as soon as reasonably practical." Don't hold up a deal over this. The key requirement from ISO 27001 A.5.24 is that YOUR 72-hour GDPR clock can start: you need enough notice to make your own notification decision.

Accept "72 hours" for standard suppliers; push for "24 hours" for Critical suppliers who have direct access to production customer data.

---

## 4. Contractor Questionnaire Fatigue

Big enterprise customers will sometimes send you a 200-question security questionnaire. Your startup clients will get the same from their customers. Teach them:
- Their ISO 27001 certificate answers most questions
- SOC 2 Type II report (if they have one) answers the rest
- For specific questions: write a 1-page security summary document they can share

As the implementor, you might also pre-prepare a "customer security one-pager" for the client — it's a selling tool and a questionnaire shortcut.

---

## 5. Verify That Certifications Are Current

Don't just check if a supplier has ISO 27001 — check the expiry date and the scope. Common traps:
- Certificate expired (6-month validity after audit; annual surveillance required)
- Certificate scope doesn't cover the service you're buying (e.g., ISO cert for HQ but not the SaaS platform)
- SOC 2 Type I (point in time, not operational) vs. Type II (6-month period — much stronger)

Check certificate registries: BSI's public registry, Bureau Veritas, LRQA. Or request the actual certificate PDF and check the issuing body's logo + stamp.

---

## 6. DPA Click-Through vs. Negotiated DPA

For major SaaS (AWS, Google, Salesforce), you click through their standard DPA — you won't negotiate it. That's fine and compliant for GDPR. Document that you accepted it.

For suppliers where you have negotiating power (smaller vendors, custom contracts), use your own DPA template. It gives you more control over data deletion timelines and sub-processor notification requirements.
