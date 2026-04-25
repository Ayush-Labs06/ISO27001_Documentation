# Common Auditor Q&A — 45 Questions with Model Answers
**ISO 27001:2022 — Certification Preparation | Version 1.0**
**Owner:** ISMS Owner

> Practice these with your client before Stage 2. Answers should be in the person's own words — never scripted. These are model answers to calibrate the depth and direction expected.

---

## Section 1: Governance and Leadership (Questions 1–8)

**Q1: How does top management demonstrate commitment to information security?**
> A: The CEO signed and approved the Information Security Policy. Management reviews are attended by the CEO and held twice a year. The IS budget is approved at board level. The CEO personally reviewed and accepted risks above our acceptance threshold.

**Q2: What are your IS objectives and how do you measure them?**
> A: We have four IS objectives defined in the IS Policy: (1) maintain critical vulnerability remediation within 7 days — measured monthly via Security Hub; (2) phishing click rate below 10% — measured quarterly via simulation; (3) training completion above 95% — tracked in our LMS; (4) zero P1 data breach incidents. We review progress at management review.

**Q3: What is the scope of your ISMS?**
> A: Our ISMS covers the development and operation of our SaaS platform, including all AWS production infrastructure in us-east-1, our engineering and support teams, and the SaaS tools our staff use to build and run the platform. We exclude our parent company's infrastructure — there's a clear boundary at our AWS account boundary.

**Q4: Who is responsible for information security and what decisions can they make?**
> A: [Name] is the ISMS Owner, responsible for day-to-day ISMS operation, risk assessment, and policy. The CEO has final authority on risk acceptance above our threshold, scope changes, and certification decisions. The CTO is responsible for technical control implementation.

**Q5: Walk me through how the ISMS works in practice.**
> A: It's organised around a Plan-Do-Check-Act cycle. We plan by maintaining our risk register and treatment plan. We do by operating the controls — our DevSecOps pipeline, access reviews, training programme. We check via monthly KPI reporting and internal audits. We act via our corrective action process and management review decisions.

**Q6: How do you handle changes to your ISMS — for example, if a new service was launched?**
> A: We have a change management policy. Significant changes — like a new product, new cloud region, or new major supplier — trigger a risk assessment review. The ISMS Owner assesses whether the scope needs updating and whether existing controls are sufficient. If the change is within scope and controls still apply, we document the review. If it expands scope, we update the ISMS scope statement and inform the CB.

**Q7: What was your last major security incident and how was it handled?**
> A: [Be specific — describe a real incident, even a P3/P4. Example:] We had a P3 incident in Q2 where an employee's laptop was lost during travel. The laptop had full disk encryption and MDM. We triggered our incident response plan, performed a remote wipe via MDM, confirmed no customer data was accessible, and closed the incident. We updated our travel device procedure as a lesson learned.

**Q8: How do you ensure the ISMS is continually improving?**
> A: We have an improvement backlog. Items come in from: internal audit findings, post-incident reviews, management review discussions, and staff suggestions. The ISMS Owner prioritises and assigns. Progress is reviewed at management review. We also version our policies — if a procedure is found inadequate, we update it.

---

## Section 2: Risk Assessment (Questions 9–14)

**Q9: Describe your risk assessment methodology.**
> A: We use an asset-based methodology. We identify information assets, then for each asset we identify threats and vulnerabilities. We score likelihood (1–5) and impact on CIA triad (1–5), giving an inherent risk score of 1–25. We then assess existing controls and score residual risk. Risks above a score of 15 must be treated. Risks between 10–15 are reviewed by the ISMS Owner and accepted by the CEO.

**Q10: How often do you perform a risk assessment?**
> A: We do a full risk assessment annually, and a targeted review when there are significant changes — new product launch, significant infrastructure change, major new supplier, or after a security incident. The last full assessment was completed [date].

**Q11: Show me a risk in your register and walk me through it.**
> A: [Pull up a specific risk — e.g., R-001.] This is the risk of PII database credential compromise via credential stuffing. The asset is our RDS production database. The threat is credential stuffing / brute force. The vulnerability was no rate limiting on the API login endpoint. The inherent risk was score 20 (likelihood 4, impact 5). We mitigated with: bcrypt password hashing, rate limiting, MFA for admin accounts, and no direct DB access from the internet. Residual risk is now 15 — below our treatment threshold.

**Q12: How did you determine which Annex A controls to include in your SoA?**
> A: We used the risk assessment to identify which controls address our identified risks. We also reviewed the Annex A controls against our business context — for example, we excluded A.7.11 (supporting utilities) because we operate entirely in AWS and AWS manages physical infrastructure. Every control we excluded has a documented justification in the SoA.

**Q13: How do you know that the residual risk scores in your register are accurate?**
> A: We review controls implementation against each risk before scoring. If a control is listed but not yet implemented, we score as if it's not in place. We verify control effectiveness through our internal audit and technical testing (vulnerability scans, access reviews). The ISMS Owner signs off residual risk scores after reviewing evidence.

**Q14: Who accepts residual risk above your threshold?**
> A: The CEO accepts residual risk above our threshold score of [X]. This is documented in the risk register — there's a formal acceptance record signed by the CEO for each above-threshold risk. Currently [N] risks are above threshold, all formally accepted with documented mitigating context.

---

## Section 3: Access Control and Identity (Questions 15–21)

**Q15: How do you manage access to your production environment?**
> A: All production access goes through AWS SSO. No long-lived IAM keys. Engineers access production via SSM Session Manager — no SSH key pairs. Privileged actions require time-limited role assumption. We have a break-glass account for emergencies with CloudWatch alerting if it's used.

**Q16: How do you handle a new employee's access?**
> A: We have a formal onboarding checklist. Access is provisioned based on role — we have pre-defined access groups in our IdP. The IT Lead provisions access; the ISMS Owner reviews group assignments. New employees only get access to systems required for their role — no blanket admin access. MFA is enrolled on Day 1 before any access is granted.

**Q17: What happens when an employee leaves?**
> A: We have an offboarding checklist. On the final day, access is revoked in the IdP — this triggers SSO revocation across all connected tools. AWS access is checked separately (IAM credential report). GitHub org membership is removed. We do a 24-hour check to confirm all access is gone. The checklist is signed off by HR and IT Lead.

**Q18: Show me your most recent access review.**
> A: [Show the completed access review template for the last quarter. Walk through: who reviewed, which systems, what was found, what actions were taken.]

**Q19: How is MFA enforced?**
> A: MFA is enforced at the IdP level — Conditional Access policies in Entra ID require MFA for all sign-ins. We also enforce it at the application level for AWS, GitHub, and our production systems. Monthly, I run an IAM credential report and a script checking Entra sign-in logs for any MFA-skipped authentications. Any exception requires documented approval.

**Q20: How do you manage privileged access?**
> A: Privileged access follows just-in-time principles. For AWS, we use SSO with time-limited role assumption. Production database access goes through SSM Session Manager tunnels — no direct DB connections from developer machines. We have an Entra PIM configuration for Azure resources. All privileged actions are logged in CloudTrail.

**Q21: How do you handle third-party/contractor access?**
> A: Contractors get provisioned through the same IdP as employees, with time-boxed access tied to their contract end date. We set an explicit account expiry. When a contractor's engagement ends, the offboarding procedure applies. We've had instances of forgetting to revoke — our current control is an automated account expiry flag in the IdP for contractor accounts.

---

## Section 4: Infrastructure and Technical Controls (Questions 22–30)

**Q22: How do you know if someone is attacking your systems?**
> A: AWS GuardDuty provides threat detection — it analyses CloudTrail, VPC Flow Logs, and DNS logs. We have CloudWatch alarms on critical events: root account usage, IAM changes, security group changes, etc. GuardDuty findings go to a Slack channel and are reviewed weekly by the DevOps Lead. Critical-severity findings page us immediately via PagerDuty.

**Q23: Show me your vulnerability management process.**
> A: We run Amazon Inspector for EC2 and ECR image scanning continuously. Trivy runs in our CI/CD pipeline and blocks merges if it finds critical CVEs. We have severity-based SLAs: critical = 7 days, high = 30 days. The DevOps Lead reviews Inspector findings weekly and tracks remediation. I can show you the last vulnerability report.

**Q24: How do you manage cryptography?**
> A: Our cryptography policy defines approved algorithms. All data in transit uses TLS 1.2 minimum. All data at rest uses AES-256 — enforced via KMS for RDS, S3, and EBS. We don't use deprecated algorithms — SHA-1 and MD5 are prohibited. KMS keys are rotated annually. We have an alert if a KMS key is disabled or deleted.

**Q25: Walk me through your CI/CD security controls.**
> A: Our GitHub Actions pipeline has mandatory gates: (1) gitleaks for secret scanning — if a secret is committed, the pipeline fails; (2) Semgrep SAST scanning; (3) Trivy container scanning; (4) Checkov for Terraform IaC scanning. PRs require CODEOWNERS review. Deployments to production require a separate approval step. OIDC is used for AWS authentication — no long-lived keys in GitHub.

**Q26: How do you manage backups?**
> A: AWS Backup manages our RDS and EBS backups. We have a daily backup with 35-day retention and a monthly backup with 1-year retention. Backups are encrypted with KMS and stored in a separate account to protect against ransomware. We do a restore test quarterly — last test was [date], RTO achieved was [X] minutes.

**Q27: How is your network segmented?**
> A: Production runs in a dedicated VPC with public subnets (load balancer only), private subnets (application), and data subnets (RDS/ElastiCache with no route to internet). Security groups follow least-privilege — no 0.0.0.0/0 ingress except the load balancer on 443. Dev and prod are in separate AWS accounts with no peering.

**Q28: How do you prevent secrets from being hardcoded in code?**
> A: Multiple layers: pre-commit hooks with gitleaks that run locally before commit; gitleaks also runs in CI as a pipeline gate; all secrets are stored in AWS Secrets Manager and accessed via SDK at runtime. We had one historical secret committed to a private repo — we found it via audit, rotated the secret, and added it to gitleaks rules. The incident is in our incident log.

**Q29: How do you ensure your endpoints (laptops) are secure?**
> A: All laptops are enrolled in Intune/Jamf. The MDM enforces: disk encryption, screen lock (5 min), OS patch compliance, and blocks unenrolled devices from accessing company resources via Conditional Access. CrowdStrike EDR is deployed to all endpoints. Monthly, I pull the MDM compliance report and chase non-compliant devices.

**Q30: How do you handle penetration testing?**
> A: We conduct an annual penetration test by an external provider with CHECK or CREST accreditation. We notify AWS before testing per their pen test policy. The last test was [date]. Findings are tracked in our corrective action tracker. Critical findings must be remediated within 30 days. I can show you the last pen test executive summary and our remediation status.

---

## Section 5: Incident Response and Business Continuity (Questions 31–36)

**Q31: If you got ransomware right now, what would you do?**
> A: Alert the Incident Commander immediately. The Incident Commander calls the on-call engineer and CEO. We isolate affected systems using the quarantine security group in AWS. We preserve evidence before touching anything — CloudTrail logs to an S3 bucket with Object Lock. We assess backup integrity to determine if clean backups exist. Then we follow the IRP phases: contain, investigate, notify, remediate. GDPR 72-hour clock assessment starts immediately.

**Q32: How do you handle data breach notifications under GDPR?**
> A: Our IRP has a specific GDPR decision tree. When personal data is involved, the ISMS Owner assesses: what data, how many individuals, what risk to those individuals. If it's likely to result in a risk to individuals' rights and freedoms, we notify the ICO within 72 hours of awareness. If it's high risk, we also notify affected individuals. We've had [N] GDPR-notifiable incidents — [describe if any, or say "none to date"].

**Q33: Has your business continuity plan been tested?**
> A: Yes. We conducted a tabletop exercise on [date] using three scenarios from our exercise script. We also conducted a DR failover test on [date] — we successfully promoted the read replica to DR region in [X] minutes against a 2-hour RTO target. The restore test result is documented in our RTO/RPO worksheet.

**Q34: How do you capture lessons learned from incidents?**
> A: Every incident — even P3/P4 — has a post-incident review section in the incident log template. After the incident is closed, the Incident Commander completes the review within 5 business days. Lessons are categorised into: process gaps, control gaps, documentation gaps. Significant lessons feed into the corrective action tracker and may update the IRP.

**Q35: What is your RTO for the production platform?**
> A: Our target RTO is 2 hours. Our last DR test achieved [X] hours. The gap was due to [reason — e.g., DNS TTL wait time + manual secrets recreation]. We have an action in our corrective action tracker to automate Route 53 health-check failover and secrets sync to reduce this.

**Q36: Who makes the decision to declare a major incident?**
> A: The Incident Commander (ISMS Owner) declares severity based on our classification matrix. For P1 incidents, the CEO is notified within 1 hour and co-decides on external notifications and customer communications. The ISMS Owner has authority to invoke the IRP immediately — no waiting for CEO approval on containment actions.

---

## Section 6: Supplier Management (Questions 37–41)

**Q37: How do you manage your third-party suppliers?**
> A: We maintain a subprocessor register of all Tier 1/2 suppliers. Tier 1 suppliers undergo an annual security assessment using our questionnaire. All Tier 1/2 suppliers have a Data Processing Agreement. We check certificate validity (ISO 27001 or SOC 2) annually. Any supplier security incidents are tracked and assessed.

**Q38: Is AWS in your supplier register?**
> A: Yes. AWS is listed as a critical (Tier 1) supplier. We've documented the shared responsibility model — AWS manages physical and hypervisor security; we're responsible for everything above the platform. AWS's ISO 27001 certificate is on file. We use AWS's click-through DPA.

**Q39: What happens if a key supplier has a security incident affecting our data?**
> A: Our supplier contracts require notification within 24 hours (Tier 1) or 72 hours (Tier 2). When we receive a notification, we assess: did the breach affect our data? If yes, is personal data involved? We follow our GDPR breach notification procedure. We also review whether to continue using the supplier or implement additional controls.

**Q40: How do you evaluate new suppliers?**
> A: New Tier 1/2 suppliers go through our security questionnaire before contract signature. We review their certifications (ISO 27001, SOC 2), DPA terms, data residency, and sub-processor list. Tier 1 requires a full security assessment. Tier 2 can use their certifications as evidence. We document the assessment in our supplier register.

**Q41: Show me your supplier register and a completed assessment.**
> A: [Show the subprocessor register. Pick a Tier 1 supplier — e.g., the data pipeline tool — and walk through the completed assessment: questionnaire scores, certificate on file, DPA, gaps identified, risk rating, decision.]

---

## Section 7: Awareness Training (Questions 42–45)

**Q42: How do you know staff are security-aware?**
> A: We run an annual mandatory training programme covering IS fundamentals, phishing, and GDPR. Completion is tracked in our LMS — we're currently at [X]% completion. We also run quarterly phishing simulations. Last simulation click rate was [X]% — down from [X]% at the start of the year. Staff who click receive remedial training.

**Q43: What training do your developers receive?**
> A: Developers complete the standard annual training plus a developer-specific secure coding track covering OWASP Top 10, secrets management, dependency management, and IaC security. This is run annually. I can show you the training syllabus and completion records for the engineering team.

**Q44: Ask me [employee name] — what would they say if I asked how to report a security incident?**
> A: [Brief the employee in advance.] They should say: "Email security@[company.com] or post in the Slack #security-incidents channel. For something urgent, call [ISMS Owner name] directly."

**Q45: How do you handle a new employee's security orientation?**
> A: On Day 1, every new employee gets a 30-minute walkthrough of key policies — AUP, IS Policy, data classification, incident reporting. They sign the policy acknowledgement before they get system access. We assign LMS training that must be completed within their first 5 days. I can show you a completed onboarding checklist from a recent hire.
