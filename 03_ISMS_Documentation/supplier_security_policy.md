# Supplier Security Policy
**ISO 27001:2022 — A.5.19, A.5.20, A.5.21, A.5.22, A.5.23 | Version 1.0**
**Owner:** ISMS Owner
**Approved by:** [CEO] | [Date]

---

## 1. Purpose

To ensure third-party suppliers, cloud services, and partners who access, process, or store [Organization Name]'s information are subject to appropriate security requirements and ongoing oversight.

---

## 2. Supplier Definitions

| Term | Definition |
|------|-----------|
| **Critical supplier** | Processes Restricted data; direct access to production systems; single-point-of-failure for operations |
| **Important supplier** | Processes Confidential data; significant operational dependency |
| **Standard supplier** | Processes Internal/Public data only; limited operational impact |
| **Subprocessor** | Processes personal data on our behalf (GDPR Article 28) |

---

## 3. Supplier Register

All suppliers with access to in-scope information must be listed in the Supplier Register (`06_Supplier_ThirdParty/subprocessor_register.md`). The register includes:
- Supplier name and service provided
- Data types accessed / processed
- Criticality tier
- Last assessment date
- DPA / contractual security clauses in place
- Certifications held (ISO 27001, SOC 2, etc.)

---

## 4. Pre-Engagement Assessment

Before engaging a new supplier that will access Confidential or Restricted information:

1. **Tier classification** — Assign Critical / Important / Standard
2. **Security questionnaire** — Critical and Important suppliers must complete the Vendor Security Questionnaire (`06_Supplier_ThirdParty/vendor_risk_questionnaire.md`)
3. **Certification review** — Accept ISO 27001 / SOC 2 Type II / CSA STAR as evidence in lieu of questionnaire for well-known providers
4. **Risk assessment** — Document supplier risk in the main risk register
5. **Contract review** — Ensure security clauses are included (see Section 5)
6. **DPA** — Required for all subprocessors (GDPR Article 28)
7. **ISMS Owner approval** — Required before Critical supplier onboarding

### Fast-track for Major Cloud/SaaS Providers

For widely-used providers with public certifications (AWS, Azure, GitHub, Google, Stripe, Zendesk):
- Accept their published ISO 27001 / SOC 2 certificates as primary evidence
- Review their shared responsibility model
- Complete abbreviated questionnaire covering: data residency, incident notification, encryption, access controls
- No full questionnaire required — document the rationale in the supplier register

---

## 5. Contractual Security Requirements (A.5.20)

All contracts with Critical and Important suppliers must include:

| Clause | Requirement |
|--------|------------|
| Data Processing Agreement | Required for all subprocessors (GDPR Art. 28) |
| Confidentiality | Supplier must protect confidential information |
| Security standards | Minimum security controls specified or referenced by certification |
| Incident notification | Must notify [Organization Name] within 24–72 hours of any breach affecting our data |
| Right to audit | [Organization Name] may audit or commission audit of supplier security |
| Data return/deletion | On contract end, return or destroy our data within 30 days |
| Sub-processors | Supplier must notify us of any new sub-processors handling our data |
| Data residency | Data must remain in approved regions (EU for GDPR scope data) |

---

## 6. Ongoing Monitoring (A.5.22)

| Activity | Frequency | Scope |
|----------|-----------|-------|
| Supplier register review | Annual | All suppliers |
| Security questionnaire renewal | Annual | Critical suppliers |
| Certificate review (ISO/SOC2) | Annual | All certified suppliers |
| Incident notification review | Per incident | Relevant suppliers |
| Contract review | On renewal | All contracts |
| Critical supplier service review | Quarterly | Critical suppliers |

Monitoring includes:
- Checking that supplier certifications haven't expired
- Reviewing supplier security bulletins and incident disclosures
- Reviewing access granted to suppliers in IdP audit logs
- Checking sub-processor notifications from critical suppliers

---

## 7. Cloud Services (A.5.23)

All cloud services used for in-scope workloads must:
- Be on the approved cloud services list
- Have a shared responsibility model documented
- Have appropriate security configuration applied (see `04_Infrastructure_Compliance/`)
- Be included in the supplier register and covered by DPA where applicable

**AWS Shared Responsibility:** AWS is responsible for security **of** the cloud (physical, hypervisor, network). [Organization Name] is responsible for security **in** the cloud (IAM, data encryption, application, network configuration).

**Azure:** Microsoft is responsible for physical infrastructure, hypervisor, and base OS. [Organization Name] manages identity (Entra ID configuration), application layer, and data.

---

## 8. ICT Supply Chain Security (A.5.21)

For software supply chain risk:
- Software dependencies scanned via Dependabot / Snyk (see `04_Infrastructure_Compliance/devsecops_integration_checklist.md`)
- Docker base images from official/verified publishers; pinned by digest
- Open-source components reviewed for license compliance and known vulnerabilities
- Private package repositories used where feasible to avoid dependency confusion attacks

---

## 9. Supplier Offboarding

When a supplier relationship ends:
1. Revoke all access (IdP, system access, VPN credentials) within 5 business days
2. Request confirmation of data deletion within 30 days
3. Archive supplier record in supplier register with end date
4. DPA obligations survive until data is confirmed deleted
5. Update subprocessor register (required for GDPR transparency)

| Field | Detail |
|-------|--------|
| Document ID | ISMS-SUP-001 |
| Owner | ISMS Owner |
| Review cycle | Annual |
| Next review | [Date] |
