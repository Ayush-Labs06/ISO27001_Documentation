# Tabletop Exercise Script — 3 Scenarios
**ISO 27001:2022 — A.5.29, A.5.30 | Version 1.0**
**Owner:** ISMS Owner | Duration: 60-90 minutes per scenario

---

## How to Run a Tabletop Exercise

**Participants:** ISMS Owner, CTO, DevOps Lead, Engineering Lead, HR, Legal (if available)
**Format:** Facilitator reads scenario injects. Team discusses what they would do. No hands-on — just discussion.
**Goal:** Test decision-making, identify gaps in the IRP, verify roles are understood.

**Facilitator role:** You (consultant). Ask probing questions. Don't accept "we'd just fix it" — make them be specific. Note gaps for the findings log.

**Debrief:** 20-30 min at end of each scenario. Document: what worked, what didn't, what needs updating.

---

## Scenario 1 — Ransomware Attack (60 min)

### Background
[Organization Name] operates a SaaS platform serving 500 enterprise customers. It is 09:30 on a Tuesday.

### Inject 1 — Initial Discovery (0 min)

> Your on-call engineer gets a PagerDuty alert: the production database is returning errors. They check CloudWatch and see all RDS disk I/O has spiked to 100%. Logs show thousands of file write operations. A customer support ticket arrives: "Your platform is down."

**Discussion questions:**
1. Who is the first person to call? How do you reach them at 09:30?
2. Is this a performance issue or a security incident? How do you tell the difference?
3. Do you isolate the database immediately or investigate first? What are the tradeoffs?

### Inject 2 — Confirmation (10 min)

> The on-call engineer SSMs into the application server. They find a note file: `YOUR_FILES_ARE_ENCRYPTED.txt`. Another engineer checks the database — the data files are encrypted. CloudTrail shows an IAM credential was used to run a Lambda function that accessed EBS snapshots 3 hours ago. The IAM key belongs to a former contractor who left 6 weeks ago.

**Discussion questions:**
1. Who in this room is the Incident Commander right now?
2. The contractor's IAM key is still active. What do you do first?
3. Do you notify the CEO? When? What do you tell them?
4. You have customer data encrypted. Who needs to know? In what order?
5. Do you pay the ransom? Who makes this decision?

### Inject 3 — Scope Expands (20 min)

> IT Lead discovers the same IAM credential was also used to access an S3 bucket containing customer PII (names, emails, subscription data) for ~12,000 customers. CloudTrail shows 2.3 GB of data was copied out to an external IP address.

**Discussion questions:**
1. This is now a GDPR breach. What is your 72-hour clock status? (Incident started ~3 hours ago based on CloudTrail)
2. Who makes the decision to notify the ICO?
3. Do you notify affected customers before or after the ICO?
4. Your customers' CISO is calling. The support team is forwarding calls to the CEO. Who takes this call?
5. Your PR team is asking: "Should we post on Twitter that we're investigating an outage?" — what do you say?

### Inject 4 — Recovery Decision (35 min)

> You have clean backups from 24 hours ago. Recovery from backup will take approximately 4 hours. The ransom demand is $180,000 in cryptocurrency (to recover data from the past 24 hours — 1 day of transactions). Customers are demanding updates every 30 minutes.

**Discussion questions:**
1. What is your decision on the backup restore? Who authorizes it?
2. Walk through the restore procedure. Who does what?
3. What do you tell customers while you're restoring? Draft a one-paragraph status update.
4. After restore, what additional monitoring do you put in place before bringing the platform back up?

### Debrief (15 min)

**Facilitator: document answers to these questions:**
- Was the 24-hour offboarding gap known? Is it fixed?
- Did everyone know their role? Who was confused?
- What would have made this response faster?
- What controls, if in place, would have prevented or detected this sooner?

---

## Scenario 2 — Data Breach via Third-Party (60 min)

### Background
[Organization Name] uses Zendesk for customer support. All customer tickets contain customer names, email addresses, and sometimes detailed usage data.

### Inject 1 — Supplier Notification (0 min)

> At 17:45 Friday, you receive an email from Zendesk: "We are writing to inform you of a security incident affecting our platform. An unauthorized third party accessed Zendesk data for a subset of customers between [date range]. We are still investigating. We will provide more information within 48 hours."

**Discussion questions:**
1. Is this a breach of YOUR data or Zendesk's data?
2. What is the first action? (Hint: 72-hour clock consideration)
3. Who in your organization needs to know about this tonight?
4. Do you disable Zendesk access for your support team while you investigate?
5. Zendesk hasn't confirmed whether YOUR data was affected. Do you start GDPR breach procedures now or wait?

### Inject 2 — Scope Confirmed (15 min)

> Zendesk's second email (8 hours later) confirms: your customer data was included in the breach. Specifically: customer names, email addresses, and support ticket contents for ~800 customers were accessed. The breach period spans 6 weeks.

**Discussion questions:**
1. What's your GDPR notification timeline now? (Clock started when you became "aware" — when was that exactly?)
2. What do you need to tell the ICO? Draft the 5 required elements of the Article 33 notification.
3. Zendesk's DPA says they'll notify you within 72 hours. They took 8 hours. Is this a breach of contract?
4. Your customer success team is receiving calls from customers who heard about "a Zendesk breach." What script do you give them?
5. Do you include this breach in your risk register? Does it affect your Zendesk supplier assessment?

### Inject 3 — Media Coverage (30 min)

> A security researcher has posted on Twitter about the Zendesk breach, naming your company as one of the affected organizations. Your company's Twitter mentions are blowing up. A journalist from TechCrunch emails asking for comment.

**Discussion questions:**
1. Who is authorized to respond to media? Can the CTO respond?
2. Draft a 3-sentence response to the TechCrunch journalist.
3. Should you proactively post on your company blog/status page?
4. One of your biggest enterprise customers (represents 20% of ARR) calls and says "If this is confirmed, we may have to suspend our contract." Who handles this call?

---

## Scenario 3 — Insider Threat / Employee Data Exfiltration (60 min)

### Background
A senior engineer has given their notice. Their last day is in 2 weeks. They have full access to the production codebase, AWS dev environment, and customer data in staging.

### Inject 1 — Anomalous Activity Detected (0 min)

> IT Lead receives a DLP alert: the departing engineer's Google account downloaded 4.7 GB from the shared Google Drive in the past 3 hours. This is unusual — their typical daily usage is under 100 MB. The download included folders: "Customer Lists," "Product Roadmap," "Sales Pipeline."

**Discussion questions:**
1. Is this an incident? At what point does "downloading a lot of data" become a security concern?
2. Do you confront the employee? Do you suspend their access first?
3. HR must be involved before any personnel action. Have you called HR?
4. The employee has two more weeks on notice. Do they continue working?
5. Can you legally review the contents of what they downloaded?

### Inject 2 — Evidence Escalates (15 min)

> HR reviews with the ISMS Owner. CloudTrail shows the engineer also used their AWS dev credentials (which have read access to a staging database) to query the customers table — 45,000 rows — at 22:00 last night. They exported the query result to a CSV. The file does not appear in any company storage.

**Discussion questions:**
1. This is now a confirmed data breach (personal data likely exfiltrated). What is your immediate action?
2. When do you disable the engineer's access? If they're still physically in the office, who handles this?
3. Who is your contact point for potential legal action?
4. The exfiltrated data is likely on a personal device. What are your options?
5. Do you inform other engineers? What do you tell them?

### Inject 3 — Post-Incident (35 min)

> Access has been revoked. HR is managing the personnel process. Legal has been engaged. The engineer has now left the building.

**Discussion questions:**
1. The employee's personal laptop is suspected to contain customer data. Can you demand they return or wipe it?
2. GDPR breach: 45,000 rows of customer data potentially exfiltrated. Walk through your notification decisions.
3. What control gaps allowed this? (Access to staging customer data, no DLP on AWS exports, etc.)
4. What technical and procedural controls would you implement to prevent this in future?
5. 6 months later, a customer sues because their data was in the exfiltrated set and they received spam targeting them. What's your response?

---

## Exercise Findings Log

Document after each scenario:

| Finding | Type | Priority | Owner | Action Required |
|---------|------|---------|-------|----------------|
| Contractor access not revoked for 6 weeks | Process gap | Critical | IT Lead | Update offboarding checklist |
| GDPR notification procedure unclear | Documentation gap | High | ISMS Owner | Update IRP with GDPR decision tree |
| No staging environment customer data masking | Technical gap | High | DevOps | Data masking implementation |
| Media response not defined | Process gap | Medium | CEO | Assign Communications Lead |

---

## Post-Exercise Requirements

1. **Document** the exercise: date, participants, scenarios, findings
2. **Update** the IRP based on findings
3. **Close** high-priority technical gaps within 30 days
4. **Retain** exercise records as evidence (auditors love to see this — A.5.29)
5. **Schedule** next exercise (annual minimum; recommend every 6 months)

**Evidence for auditors:** Exercise agenda, attendance list, findings log, IRP version updated after exercise.
