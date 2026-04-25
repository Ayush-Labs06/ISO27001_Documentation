# Contract Security Clauses — Template Library
**ISO 27001:2022 — A.5.20 | Version 1.0**
**Owner:** ISMS Owner / Legal Counsel

These clauses should be included in all supplier agreements for Critical and Important suppliers. Adapt as needed for specific engagements. Legal review recommended before use.

---

## Clause 1 — Information Security Standards

> Supplier shall maintain an information security management system that is appropriate to the nature, scope, and risk of the services provided. Where Supplier processes Customer Data, Supplier shall implement and maintain security controls that are no less protective than those required by ISO/IEC 27001:2022 or equivalent recognized standard, and shall provide evidence of such compliance upon request (including certifications, audit reports, or completed security questionnaires).

---

## Clause 2 — Data Processing and GDPR (DPA Requirement)

> 2.1 Where Supplier processes Personal Data on behalf of [Organization Name] (as defined under GDPR), the parties agree to execute a Data Processing Agreement ("DPA") in the form provided by [Organization Name], pursuant to Article 28 of the General Data Protection Regulation (EU) 2016/679.
>
> 2.2 Supplier shall process Personal Data only on documented instructions from [Organization Name] and shall not process such data for any other purpose.
>
> 2.3 Supplier shall ensure that persons authorized to process Personal Data are subject to binding confidentiality obligations.
>
> 2.4 Supplier shall assist [Organization Name] in fulfilling its obligations under Articles 32–36 of GDPR (security, breach notification, data subject rights, DPIAs, and prior consultation).

---

## Clause 3 — Incident Notification

> 3.1 Supplier shall notify [Organization Name] of any confirmed or suspected security incident, data breach, or unauthorized access to [Organization Name]'s systems or data within **24 hours** of becoming aware of such incident, regardless of whether the incident has been fully assessed.
>
> 3.2 Notification shall be provided to [security@company.com] and [cto@company.com] and shall include:
> - The nature of the incident
> - Categories and approximate number of data records and individuals affected
> - Likely consequences of the breach
> - Measures taken or proposed to address the breach
>
> 3.3 Supplier shall provide updated information as it becomes available and shall cooperate fully with [Organization Name]'s incident response and any regulatory investigation.

---

## Clause 4 — Right to Audit

> 4.1 [Organization Name] reserves the right, on reasonable notice (not less than 30 days except in the event of a suspected security incident), to conduct or commission an independent security audit of Supplier's systems and processes relevant to the services provided under this agreement.
>
> 4.2 In lieu of an audit, Supplier may provide [Organization Name] with a current ISO 27001 certificate or SOC 2 Type II report. Supplier shall provide penetration test executive summaries upon request.
>
> 4.3 Supplier shall cooperate fully with any such audit and shall promptly remediate any material findings.

---

## Clause 5 — Subprocessors

> 5.1 Supplier shall not engage any subprocessor to process [Organization Name]'s data without prior written consent from [Organization Name].
>
> 5.2 Supplier shall maintain a current list of subprocessors and shall provide this list to [Organization Name] upon request and make it publicly available.
>
> 5.3 Where Supplier intends to add or replace a subprocessor, it shall notify [Organization Name] with at least 30 days' prior notice. [Organization Name] may reasonably object to the change within that period.
>
> 5.4 Supplier shall impose data protection and security obligations on its subprocessors that are no less protective than those imposed on Supplier by this agreement.

---

## Clause 6 — Data Return and Deletion

> 6.1 Upon termination or expiry of this agreement, or upon request by [Organization Name], Supplier shall:
> - Return all [Organization Name] data in a commonly used, machine-readable format within 30 calendar days; and
> - Securely delete all copies of [Organization Name] data from all systems, including backup systems, within 60 calendar days of the return.
>
> 6.2 Supplier shall provide written confirmation of deletion, including the method used, within 10 business days of completion.
>
> 6.3 Supplier may retain data only where required by applicable law, in which case it shall notify [Organization Name] of the legal basis and duration.

---

## Clause 7 — Data Residency

> Supplier shall process and store [Organization Name]'s data only within the European Economic Area (EEA) or in countries approved by [Organization Name] in writing. Any international transfer of data shall comply with applicable data protection laws, including the use of EU Standard Contractual Clauses where required.

---

## Clause 8 — Confidentiality

> 8.1 Supplier and its personnel shall treat all [Organization Name] information (including technical, commercial, and customer information) as confidential and shall not disclose it to any third party without prior written consent.
>
> 8.2 This confidentiality obligation survives the termination of the agreement for a period of **5 years**.
>
> 8.3 Supplier shall ensure all personnel with access to [Organization Name]'s information are bound by equivalent confidentiality obligations.

---

## Clause 9 — Access and Authentication

> 9.1 Supplier shall ensure that access to [Organization Name]'s systems, networks, or data is restricted to named, authorized individuals only.
>
> 9.2 Multi-factor authentication (MFA) is required for all accounts with access to [Organization Name]'s systems or data.
>
> 9.3 Supplier shall revoke access to [Organization Name]'s systems within 24 hours of any personnel change.
>
> 9.4 All access to [Organization Name]'s systems by Supplier personnel shall be logged and these logs shall be made available to [Organization Name] upon request.

---

## Clause 10 — Intellectual Property and Source Code

> Any custom code, configurations, or deliverables produced by Supplier under this agreement that incorporate or access [Organization Name]'s systems or data shall be subject to security review by [Organization Name] prior to deployment. Supplier warrants that deliverables are free of known malicious code and material security vulnerabilities.

---

## Clause 11 — Liability for Security Failures

> Where a security incident, data breach, or failure of Supplier's security obligations under this agreement results in loss, damage, or liability to [Organization Name], Supplier shall indemnify [Organization Name] for reasonable costs of incident response, notification, regulatory fines, and reputational damage, subject to the liability cap agreed in the main commercial terms.

---

## Clause 12 — Minimum Security Requirements (Schedule)

For Critical suppliers, attach a Security Requirements Schedule specifying:

| Requirement | Minimum Standard |
|------------|-----------------|
| Encryption at rest | AES-256 |
| Encryption in transit | TLS 1.2 minimum |
| MFA | Required for all accounts accessing customer data |
| Vulnerability scanning | Quarterly minimum |
| Penetration testing | Annual minimum |
| Background checks | Required for personnel with customer data access |
| Security awareness training | Annual for all staff |
| Patch management — critical | 7 days |
| Incident notification | 24 hours |
| Business continuity test | Annual |
