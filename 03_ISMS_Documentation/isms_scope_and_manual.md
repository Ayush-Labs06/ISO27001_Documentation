# ISMS Scope Statement & Manual
**ISO 27001:2022 — Clause 4.3, 4.4 | Version 1.0**
**Owner:** [ISMS Owner / CTO]
**Approved by:** [CEO] | [Date]

---

## Part 1 — ISMS Scope Statement (Clause 4.3)

### Formal Scope Statement

> **[Organization Name]'s** Information Security Management System (ISMS) encompasses the design, development, operation, and support of **[Product/Service Name]**, a [brief product description], including all underlying cloud infrastructure, internal IT systems, corporate networks, and supporting business processes used to deliver and maintain these services.
>
> The ISMS applies to all employees, contractors, and third parties who have access to in-scope information assets operated from **[primary location(s)]** and managed through cloud services hosted on **AWS (primary)** and **Microsoft Azure (identity and endpoint management)**.
>
> The following are explicitly excluded from the ISMS scope:
> - **[Legacy product / sunset service]:** No active customer data; decommissioned Q[X] [Year]
> - **[Subsidiary / affiliate]:** Separate legal entity with independent systems
> - **Development/sandbox environments:** No production customer data; developer workstations are covered under this ISMS via policy controls

### Scope Boundary Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                    ISMS SCOPE BOUNDARY                        │
│                                                              │
│  ┌─────────────┐    ┌──────────────┐    ┌────────────────┐  │
│  │  AWS Prod   │    │  Azure Entra │    │  HQ Office     │  │
│  │  Account    │    │  + Defender  │    │  + Laptops     │  │
│  │             │    │              │    │                │  │
│  │ - ECS/EKS   │    │ - Identity   │    │ - Employee     │  │
│  │ - RDS       │    │ - MFA        │    │   workstations │  │
│  │ - S3        │    │ - Conditional│    │ - Office       │  │
│  │ - CloudTrail│    │   Access     │    │   network      │  │
│  └─────────────┘    └──────────────┘    └────────────────┘  │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                SaaS Tools (in scope)                │    │
│  │  GitHub | Slack | Google Workspace | Zendesk | ...  │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ────────── SCOPE BOUNDARY ──────────────────────────────── │
│                                                              │
│  CUSTOMERS ──→ [API/Web] ──→ PRODUCT ──→ Data storage       │
│  EMPLOYEES ──→ IdP (Entra ID / Okta) ──→ All systems        │
│  SUPPLIERS ──→ Vendor register; DPA in place                │
│                                                              │
└──────────────────────────────────────────────────────────────┘

OUT OF SCOPE:
├── AWS Dev Account (no prod data; covered by AUP)
├── [Legacy product] (decommissioned)
└── [Subsidiary] (separate legal entity)
```

---

## Part 2 — ISMS Manual (Clause 4.4)

This manual provides the high-level framework for [Organization Name]'s ISMS. It references the supporting documents, policies, and procedures that together constitute the ISMS.

### 2.1 Purpose

[Organization Name] is committed to protecting the confidentiality, integrity, and availability of its information assets and those entrusted to it by customers, suppliers, and employees. This ISMS manual describes how [Organization Name] establishes, implements, maintains, and continually improves its information security management system in accordance with ISO 27001:2022.

### 2.2 Context of the Organization (Clause 4.1, 4.2)

**Internal factors relevant to the ISMS:**
- Growth-stage technology company; engineering team is the primary ISMS stakeholder
- Cloud-native infrastructure; no owned data centres
- Agile development practices; frequent deployments to production
- Remote-first team across [locations]
- Reliance on third-party SaaS tools for core business operations

**External factors relevant to the ISMS:**
- Customer security requirements (enterprise sales pipeline)
- GDPR obligations for EU personal data processing
- Sector-specific regulations: [list any applicable]
- Increasing threat landscape: credential attacks, supply chain compromises, ransomware

**Interested parties and their requirements:**

| Party | Requirement |
|-------|------------|
| Customers | Confidentiality of their data; uptime; breach notification |
| Employees | Protection of personal data; secure work environment |
| Regulators | GDPR compliance; incident notification |
| Cloud providers | Compliance with cloud service agreements |
| Investors / board | Risk management; compliance posture |
| Certification body | ISO 27001:2022 conformance |

### 2.3 Leadership and Policy (Clause 5)

Top management ([CEO name]) is committed to the ISMS and has:
- Approved this manual and the Information Security Policy
- Allocated resources for ISMS implementation and maintenance
- Assigned ISMS ownership to [ISMS Owner / CTO name]
- Committed to participating in annual management review

The **Information Security Policy** (ref: `information_security_policy.md`) is the top-level policy governing this ISMS.

### 2.4 ISMS Document Hierarchy

```
Level 1: ISMS Scope & Manual (this document)
           │
Level 2: Information Security Policy (top-level)
           │
Level 3: Topic-specific Policies
           ├── Acceptable Use Policy
           ├── Access Control Policy
           ├── Cryptography Policy
           ├── Data Classification Policy
           ├── Incident Response Policy
           ├── Supplier Security Policy
           ├── Change Management Policy
           └── [others as needed]
           │
Level 4: Procedures and Guidelines
           ├── Document Control Procedure
           ├── Risk Assessment Methodology
           ├── Internal Audit Procedure
           ├── Onboarding/Offboarding Procedure
           └── [operational procedures]
           │
Level 5: Records and Evidence
           ├── Risk Register
           ├── Asset Register
           ├── Statement of Applicability
           ├── Audit Reports
           ├── Training Records
           └── Incident Logs
```

### 2.5 Risk Management Framework (Clause 6)

[Organization Name] manages information security risk using an asset-based methodology documented in `risk_methodology.md`. Key elements:
- Likelihood × Impact scoring (1–5 × 1–5 = 1–25)
- Risk acceptance threshold: Low (≤4) accepted; Medium–Critical require treatment
- Risk register maintained and reviewed at least annually
- Statement of Applicability covers all 93 Annex A controls

### 2.6 Supporting Processes (Clause 7)

| Process | Document | Owner |
|---------|---------|-------|
| Document control | `document_control_procedure.md` | ISMS Owner |
| Security awareness training | `08_Awareness_Training/` | HR |
| Competence management | Training records | HR |
| Communication | IS Policy Section 4 | ISMS Owner |

### 2.7 Operational Controls (Clause 8)

Controls are implemented per the Risk Treatment Plan and Statement of Applicability. Technical controls are documented in `04_Infrastructure_Compliance/`. Access controls in `05_Access_Control_Identity/`. Supplier controls in `06_Supplier_ThirdParty/`. Incident response in `07_Incident_BCP/`.

### 2.8 Performance Evaluation (Clause 9)

| Activity | Frequency | Owner | Document |
|----------|-----------|-------|---------|
| Internal audit | Annual (minimum) | ISMS Owner | `09_Internal_Audit/` |
| Management review | Annual (minimum) | CEO | `10_Management_Review/` |
| Risk assessment review | Annual + on change | ISMS Owner | Risk Register |
| KPI monitoring | Monthly | ISMS Owner | KPI Dashboard |
| Supplier review | Annual | ISMS Owner | Supplier Register |

### 2.9 Continual Improvement (Clause 10)

[Organization Name] improves the ISMS through:
- Corrective actions from internal audits (nonconformity reports)
- Lessons learned from security incidents
- Management review outputs
- Continuous improvement log (`12_Post_Certification/continuous_improvement_plan.md`)

---

## Part 3 — Document Control

| Field | Detail |
|-------|--------|
| Document ID | ISMS-001 |
| Version | 1.0 |
| Status | Approved |
| Owner | [ISMS Owner] |
| Approved by | [CEO] |
| Approval date | YYYY-MM-DD |
| Next review | YYYY-MM-DD (annual) |
| Classification | Confidential |
| Location | [ISMS document store] |
