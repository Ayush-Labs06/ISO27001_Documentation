# IRL Tips — Incident Response & Business Continuity

---

## 1. The 72-Hour GDPR Clock Starts at "Awareness," Not Discovery

The clock starts when your organization becomes "aware" — meaning when you have enough information to conclude a breach of personal data has likely occurred. It does not start when the incident began (e.g., when an attacker first got in). It starts when YOU found out.

This distinction matters: if an attack happened 3 weeks ago but you found out today, your 72-hour window starts today. However, you're then required to explain why you didn't detect it sooner — which opens a different line of questioning about monitoring and detection controls.

**Practical implication:** The moment you believe personal data is "probably involved," start your GDPR clock. Don't wait for full confirmation — you can notify with "we are still investigating" and update the notification. A late notification is a separate infringement.

---

## 2. IRP Is Evidence — It Must Show It Has Actually Been Used

Auditors don't just want to see a polished IRP document. They want to see evidence it's been exercised. Look for:
- Incident logs (even for minor events, P3/P4)
- Tabletop exercise records with attendance lists
- Post-incident review documents
- IRP version changes after lessons learned

A pristine IRP that has never been opened is a yellow flag. A slightly messy IRP with actual incident logs and exercise findings attached is much stronger evidence.

**What to tell clients:** "Every phishing email your security awareness training catches should generate a P4 incident log entry. It's 5 minutes of work and gives you 12+ evidence records per year."

---

## 3. BCP and IRP Overlap More Than the ISO Clauses Suggest

ISO 27001 A.5.29 (IS during disruption) and A.5.30 (ICT readiness) are continuity-focused, but they reference A.5.24–A.5.26 (incident response). In practice, every major incident IS a continuity event. You don't need two totally separate plans — you need a clear decision point in the IRP:

> "If this incident is expected to cause >4 hours of service disruption, invoke the BCP."

Make sure the two plans cross-reference each other at that junction. Auditors will ask: "If ransomware hit you tonight, which plan kicks in? At what point does the BCP activate?"

---

## 4. RTO/RPO Targets Are Useless Without Test Evidence

Every startup writes "RTO: 2 hours" in their BCP. Almost none of them have tested it. Auditors know this and will ask: "When did you last test your DR? What was the actual RTO achieved?"

If you haven't tested: the honest answer is "we haven't tested yet — our next DR test is scheduled for [date]." Then actually schedule it.

If RTO tests come back worse than targets: that's a nonconformity on the corrective action tracker, not a reason to quietly adjust the target. Fix the gap or formally accept the higher RTO with documented rationale.

**Common finding:** "DR test achieved 5.5-hour RTO against a 2-hour target." This comes from underestimating secrets recreation, DNS TTL wait times, and cold-start times. Build those into runbook timelines.

---

## 5. Contractor Offboarding Gaps Are the #1 Source of Ransomware Entry Points

In incident scenario after incident scenario, the pattern is: former contractor's credentials → still active → used for initial access. The Scenario 1 tabletop exercise uses this deliberately because it's real.

For clients, the fix is simple: offboarding checklist that explicitly includes:
- IAM keys (not just console access)
- GitHub collaborator access (often missed)
- Any direct DB access granted for a project
- Secrets they may have had copied locally (rotate, don't just revoke)
- Any 3rd-party tool invites (Jira, Notion, Slack, Datadog)

The audit question will be: "Show me your offboarding procedure and show me a completed offboarding record from the last 6 months." Have both.

---

## 6. Tabletop Exercises Are Underused and Overvalued at the Same Time

Underused: Most clients never run them. When prompted, they'll say "we'd just call the CTO." That's not a plan — that's a hope.

Overvalued: Some clients think one tabletop per year means their incident response is sorted. It doesn't — it means you've identified gaps. The value is in what you update after the exercise.

**Run the exercise as a facilitator, not a participant.** Your job is to ask "what do you do next?" and probe gaps — not to solve the problem for them. When they say "we'd disable the account," ask: "Who has the access to do that right now? How long does it take? Is there a ticket? Who approves?" That's where the real gaps surface.

---

## 7. Evidence Preservation Order Matters Legally

If an incident might result in criminal referral or civil litigation (insider threat, ransomware with identifiable threat actor), the order of operations for evidence matters:

1. Preserve evidence **before** remediation (not after)
2. Use Object Lock / WORM storage — don't just copy to a normal S3 bucket
3. Document chain of custody: who accessed the evidence bucket, when, why
4. Contact legal counsel before doing anything that might be construed as evidence tampering (wiping an endpoint, even a compromised one)

In practice: Create the evidence S3 bucket with Object Lock on Day 1 of the engagement, not during an incident.

---

## 8. Status Pages Are Part of Incident Response

Many startups forget that a status page (statuspage.io, Atlassian, etc.) is an incident response tool, not just a marketing asset. During an outage, customers who can see a status update with honest information are measurably less likely to escalate or churn.

Procedure: Within 30 minutes of a P1/P2 incident, someone posts to the status page — even if it just says "We are investigating reports of a service disruption and will post updates every 30 minutes." That message alone reduces inbound support volume by 40–60%.

Make sure the status page update responsibility is named in the IRP. It often isn't, and during a chaotic incident nobody wants to draft customer-facing copy.
