# Phishing Simulation Plan
**ISO 27001:2022 — A.6.3 | Version 1.0**
**Owner:** ISMS Owner
**Frequency:** Quarterly (minimum)

---

## 1. Purpose

Regular phishing simulations measure the effectiveness of security awareness training and identify employees who need additional support. Simulation data feeds into the management review and demonstrates a functioning awareness program to auditors.

**Auditor evidence this produces:** Phishing click rate trend over time + remedial training completion records.

---

## 2. Simulation Platform

**Recommended:** KnowBe4, Hoxhunt, Proofpoint Security Awareness, or Cofense.
**Budget alternative:** GoPhish (open source, self-hosted) — requires setup but is free.

**Platform requirements:**
- Integrates with Entra ID / Google Workspace for recipient list sync
- Tracks: who received, who opened, who clicked, who entered credentials, who reported
- Provides landing page explaining it was a simulation (post-click)
- Supports scheduling and template library

---

## 3. Simulation Design Principles

### What makes a good simulation

- **Realistic but not cruel:** Simulate real-world phishing tactics, not impossible-to-detect attacks. The goal is learning, not gotcha.
- **Graduated difficulty:** Start with obvious phishing (generic sender, generic content). Increase sophistication over time as baseline improves.
- **Timely themes:** Use topical content — e.g., HR policy update emails in January, IT password reset in Q3, "new benefits enrollment" in open enrollment periods.
- **No punishment culture:** Employees who click should receive training, not shame. The metric is organizational improvement, not individual failure.

### What to avoid

- Using genuinely distressing content (fake termination notices, fake family emergencies)
- Running simulations immediately before or during major company events (product launch, layoffs)
- Making simulations so obvious they teach nothing (real attackers don't use Comic Sans)

---

## 4. Simulation Template Library

### Template Tier 1 — Easy (for baseline / new employees)

| Template Name | Pretext | Red Flags Visible | Credential Capture |
|--------------|---------|------------------|-------------------|
| Generic IT Password Reset | "IT Department" password expiry notice | Generic greeting; sender domain mismatch | Link to fake login |
| Package Delivery | "FedEx" delivery notification with tracking link | No package expected; link URL is random domain | Link only |
| Prize Winner | "Congratulations — you've been selected" | Implausible; unsolicited | Link to fake form |

### Template Tier 2 — Medium

| Template Name | Pretext | Red Flags | Credential Capture |
|--------------|---------|-----------|-------------------|
| DocuSign Request | "Please review and sign this document" | Sender is docusign-notifications@[random].com | Fake DocuSign login |
| Slack Security Alert | "Unusual sign-in detected — verify your account" | Sender not slack.com; urgency language | Fake Slack login |
| GitHub Org Invite | "You've been invited to [random-org]" | Link goes to github-security-[random].com | Link only |
| IT Helpdesk Survey | "Quick 2-minute IT satisfaction survey" | From IT-helpdesk@[company.co] (note: .co not .com) | Form with data collection |

### Template Tier 3 — Hard (for experienced employees / engineers)

| Template Name | Pretext | Red Flags | Credential Capture |
|--------------|---------|-----------|-------------------|
| Spear phish: AWS billing alert | "Your AWS bill has exceeded threshold" | Sender: billing@aws-notifications[.]org | Link to fake AWS console |
| CEO request (BEC simulation) | CEO name: "Can you process this payment urgently?" | Sent from [ceo.name]@gmail.com | Reply capture |
| Security researcher contact | "I found a vulnerability in your app" | Attachment (.docm file) | Attachment open tracking |
| Vendor invoice | Existing vendor name: "Updated bank details for next payment" | Reply-to is different from sender | Reply capture |

---

## 5. Quarterly Simulation Schedule

### Q1 — Baseline Measurement

**Template:** Tier 1 (easy) — all staff
**Objective:** Establish click rate baseline for the year
**Timing:** First 3 weeks of January (after holiday, when defences are down)
**Reporting landing page:** "This was a phishing simulation. Here's what to look for: [explain red flags]. Please report future suspicious emails to security@[company.com]."

### Q2 — Post-Training Check

**Template:** Tier 2 (medium) — all staff
**Objective:** Measure improvement after annual training
**Timing:** 4–6 weeks after annual training completion
**Focus:** Do Q1 clickers do better after training?

### Q3 — Technical Audience Focus

**Template:** Tier 3 (hard) — engineering and finance teams
**Objective:** Test high-value targets against more sophisticated attacks
**Timing:** Mid-year
**Note:** Notify HR before running CEO-impersonation BEC template

### Q4 — Year-End Measurement

**Template:** Tier 2 (medium) — all staff
**Objective:** Year-end benchmark; compare to Q1
**Timing:** October–November (Cybersecurity Awareness Month)
**Note:** Good time to combine with phishing report to all-staff showing improvement trend

---

## 6. Metrics to Track Per Simulation

| Metric | Formula | Target |
|--------|---------|--------|
| Delivery rate | Delivered / Sent | >98% |
| Open rate | Opened / Delivered | Informational only |
| Click rate | Clicked / Delivered | <10% (<5% is excellent) |
| Credential submission rate | Creds submitted / Clicked | <5% |
| Report rate | Reported suspicious / Delivered | >20% |
| Remedial completion rate | Completed remedial / Clicked | 100% within 5 days |

---

## 7. Post-Click Response Procedure

When an employee clicks:

1. **Immediate:** They see the educational landing page (platform delivers this automatically)
2. **Within 1 hour:** LMS automatically assigns 15-minute phishing remedial module
3. **Within 24 hours:** Their manager is notified (in aggregate — not singling out if first offence)
4. **Within 5 days:** Remedial module must be completed
5. **Repeated clicker (≥3 times):** HR notified; mandatory in-person review

**ISMS Owner to review:** Click patterns. If the same department clicks repeatedly, consider targeted training for that team. If engineering is clicking at high rates, that's a priority gap.

---

## 8. Reporting Template

Post each simulation, produce a 1-page report:

```
PHISHING SIMULATION REPORT
===========================
Simulation Date: [date]
Template Used: [name] (Tier [1/2/3])
Platform: [KnowBe4 / GoPhish / etc.]

RESULTS
-------
Recipients: [N]
Click Rate: [X]% (target: <10%)
Credential Submission: [X]%
Report Rate: [X]% (target: >20%)

CLICKER BREAKDOWN
-----------------
By department: [table]
Repeat clickers: [N] (defined as >1 click across simulations this year)

TREND vs. PREVIOUS QUARTER
---------------------------
Click rate: [last quarter X%] → [this quarter Y%] ([+/-]%)
Report rate: [last quarter X%] → [this quarter Y%] ([+/-]%)

REMEDIAL TRAINING
-----------------
Assigned to: [N] employees
Completed within 5 days: [N] ([X]%)
Outstanding: [N] (escalated to HR: Y/N)

OBSERVATIONS
------------
[e.g., "Finance team had 3x higher click rate than avg — recommend targeted session"]

ISMS Owner: [Name] | Date: [Date]
```

---

## 9. Evidence for Auditors

Retain for 3 years:
- [ ] Simulation schedule for the year
- [ ] Per-simulation reports (as above)
- [ ] Remedial training completion records (from `training_completion_tracker.md`)
- [ ] Click rate trend chart (annual)
- [ ] Any policy updates made as a result of poor results

**Auditor question you will hear:** "What was your phishing click rate last year and how has it trended?" Have the report ready.
