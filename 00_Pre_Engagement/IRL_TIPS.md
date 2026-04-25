# IRL Tips — Pre-Engagement & Scoping

Hard-won nuances from real ISO 27001 engagements. The things the course doesn't fully prepare you for.

---

## 1. The Scope Conversation is a Negotiation

Clients always want to scope in their entire company. Push back. A wider scope = more controls to evidence = more audit surface = more ways to fail. For a startup's first cert:

- **Start narrow**: one product, one cloud environment, core team
- **Exclude dev/sandbox environments explicitly** — document it
- **Exclude subsidiaries** unless the cert is specifically required for them
- A tight scope you can defend is better than a broad scope with gaps

---

## 2. "We Do It But Haven't Documented It" Is Not Compliance

You'll hear this constantly: "We already rotate our keys" or "We do access reviews informally." Partial credit in a gap analysis — but **ISO 27001 is document-and-evidence driven**. If there's no record, for the auditor it didn't happen. The rule of thumb:

> **If it's not written down and evidenced, it doesn't exist.**

Your job in pre-engagement is to set this expectation early. Otherwise clients get confused when you tell them to write a policy for something they already do.

---

## 3. Management Engagement is the #1 Predictor of Project Success

The single biggest variable is whether the CEO / CTO is genuinely bought in. Signs of real engagement:
- They attend the kickoff and ask questions
- They're willing to personally approve policies
- They allocate internal resource (not just say "you handle it")

Red flags:
- "Just tell us what to do and we'll sign it" — compliance theatre, will bite them in surveillance
- "I'm too busy — just work with [junior person]" — means you'll have no authority to push through changes
- "We need this cert in 3 months" with a current posture that needs 9+ months — commercial pressure to cut corners

**What to do:** Get a verbal commitment from the CEO in the first call that they'll personally attend the management review and approve the IS policy. It signals their accountability to themselves.

---

## 4. Check for Compliance Debt Before Committing to a Timeline

Before you give a timeline estimate, ask:
- Last pentest date (>18 months ago = needs a new one before Stage 2)
- MFA deployed everywhere? (if no: 2+ months of remediation minimum)
- Centralized logging? (if no: 3+ months to set up, tune, and retain logs properly)
- Any prior security incidents? (if unresolved: auditors will dig into these)

The worst outcome is promising a 6-month cert and hitting a wall at Month 4 because the infra isn't ready.

---

## 5. SaaS-Heavy Startups Have Weird Scoping Challenges

A company with 40 SaaS tools (Notion, Linear, Slack, HubSpot, Vercel, etc.) has a supplier management problem from day one. Every SaaS tool that processes in-scope data is a supplier. Practical approach:

- Tier them: Critical (process customer PII or auth), Important (business data), Standard (internal use)
- Critical and Important get full assessment; Standard get a lighter review
- Check which ones already have ISO 27001 or SOC 2 — that's your evidence of due diligence

---

## 6. Cloud Providers Are Suppliers — But With a Shared Responsibility Model

AWS, Azure, and GCP are your largest suppliers and they all have ISO 27001 certification themselves. This is good — it satisfies parts of A.5.19 and A.5.20. But:

- Their cert covers **the cloud platform**, not **how you configure it**
- The shared responsibility model means you own: your IAM, your data encryption, your network config, your application layer
- Include cloud providers in the subprocessor register. Auditors check.

---

## 7. Agree on Document Ownership Before Writing a Single Policy

Every policy needs a named owner who is responsible for:
- Reviewing it annually
- Keeping it current when the business changes
- Answering for it in an audit

If the client says "the consultant owns all policies," that's a problem — you won't be around in a year. Set up ownership during kickoff, even if it's informal.

---

## 8. The Project Charter Is Not Just Admin — It Protects You

A signed project charter:
- Defines what you will and won't deliver
- Gives you leverage when scope creep happens ("that's outside the charter, here's a change request")
- Protects you if the certification fails because the client didn't close their NCs
- Documents management commitment, which is itself a form of evidence

Don't skip it, even for small clients.
