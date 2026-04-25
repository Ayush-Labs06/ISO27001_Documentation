# IRL Tips — Security Awareness and Training

---

## 1. Completion Rate Is the KPI Auditors Care About Most

Auditors don't deeply evaluate training content quality — they check that it happened and that it was tracked. The two questions are: "What training do you provide?" and "How do you know everyone completed it?"

A 100% completion rate on a mediocre course beats an excellent course with 60% completion every time. For startups, the practical approach: one mandatory all-hands training session (video + quiz) + documented completion = fully compliant. Everything else is defence in depth.

Make sure the LMS or tracking sheet exports with names and dates — not just a total count. Auditors sample.

---

## 2. AUP Signature Is Separate from Training Completion

Training completion proves someone was trained. AUP acknowledgement proves they agreed to abide by the policy. These are different requirements and different records.

Every employee needs both:
- A training record (date, module, pass mark)
- A signed policy acknowledgement (AUP minimum; IS Policy for key roles)

The acknowledgement can be digital (e-signature, DocuSign, LMS checkbox with date). It cannot be "we told them in onboarding" without a record. If HR can't produce a signed AUP for an employee — that's a minor nonconformity.

---

## 3. Role-Specific Training Matters More Than Generic Modules

Auditors increasingly ask not just "did everyone get trained?" but "is the training relevant to the risks they face?" A generic 30-minute video for a DevOps engineer who deploys production infrastructure daily is insufficient — they need OWASP, secrets management, and supply chain risk content.

For clients with engineering teams: mandate the developer security track separately. Reference it in the SoA evidence for A.8.25 (secure development lifecycle) and A.8.29 (security testing). One training module can serve as evidence for multiple controls.

---

## 4. Phishing Simulations Are Not Surveillance — Frame Them Correctly

How you position the phishing simulation program affects whether it builds security culture or breeds resentment. Wrong framing: "We're testing you to catch people who fail." Right framing: "We're running realistic drills to help everyone get better at spotting attacks — because real attackers don't announce themselves."

Announce the program exists (but not the timing of specific simulations). Publish results in aggregate — not by naming individuals who clicked in all-hands meetings. The goal is a culture where people report suspicious emails, not a culture where people are afraid to admit they clicked.

---

## 5. Phishing Report Rate Is More Valuable Than Click Rate

Most programs obsess over click rate. But report rate — how many people actively forwarded the simulation to the security team — is a better signal of security culture maturity. A team with a 15% click rate but 30% report rate has employees who are engaged and know what to do. A team with a 5% click rate and 1% report rate has people who just ignore suspicious emails and don't report them.

Build the report button habit: ensure every email client has a one-click "report phishing" button. KnowBe4 and Proofpoint both provide this. Even without a platform, a "forward to security@" habit counts — include instructions in every phishing awareness module.

---

## 6. Security Awareness Month (October) Is Free Evidence

Cybersecurity Awareness Month in October gives you 4 weeks of legitimate, scheduled security content that's free, themed, and industry-recognized. CISA (cisa.gov/cybersecurity-awareness-month) publishes free posters, videos, and talking points annually.

For clients, this is easy: one Slack post per week in October + a short all-hands reminder session = documented evidence of ongoing awareness program. No LMS licence needed. Pair it with Q4 phishing simulation for a natural tie-in.

---

## 7. New Hires Are Your Highest Risk Window

The first 30 days of employment are when people are most likely to make security mistakes — they don't yet know the normal patterns, they're trying to seem capable, and they're often overwhelmed with setup tasks. The Day 1 security onboarding is not a checkbox — it's actually when it matters most.

Common mistakes to train against specifically:
- Using personal Gmail to store company docs "just for now" while waiting for company account access
- Sharing credentials with a colleague to "borrow" access to a system they haven't been provisioned for yet
- Skipping VPN because it's slower and "I'm just checking Slack"

The onboarding checklist in this toolkit is designed to catch these before they become habits.
