# Supplier & Subprocessor Register
**ISO 27001:2022 — A.5.19, A.5.22, A.5.23 | Version 1.0**
**Owner:** ISMS Owner
**Review Cycle:** Annual (add new suppliers within 30 days of onboarding)

---

## Criticality Definitions

| Tier | Definition |
|------|-----------|
| **Critical** | Processes Restricted data (PII, credentials) OR single-point-of-failure for revenue operations |
| **Important** | Processes Confidential data OR significant operational dependency |
| **Standard** | Internal data only; easily replaced; limited security impact |

---

## Active Supplier Register

### Infrastructure & Cloud

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review | Notes |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|-------|
| S-001 | Amazon Web Services | Primary cloud infrastructure | Customer PII, application data, logs | Critical | EU (eu-west-1) | ✅ | ✅ | ✅ | [Date] | Shared responsibility model documented |
| S-002 | Microsoft Azure | Entra ID, Defender, Intune | Employee identity, device data | Critical | EU | ✅ | ✅ | ✅ | [Date] | Used for identity only |
| S-003 | Cloudflare | CDN, WAF, DNS, email security | Traffic metadata | Important | Global | ✅ | ✅ | ✅ | [Date] | Data in EU PoPs; minimal PII |

### Identity & Access

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-004 | [Okta / Azure Entra] | Identity provider, SSO | Employee identity, auth logs | Critical | EU | ✅ | ✅ | ✅ | [Date] |
| S-005 | [1Password / Bitwarden] | Password manager | Credentials vault | Critical | EU/US | ✅ | ✅ | ✅ | [Date] |

### Source Code & Development

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-006 | GitHub (Microsoft) | Source code, CI/CD | Source code, secrets (if misconfigured) | Critical | US/Global | ✅ | ✅ | ✅ | [Date] |
| S-007 | Snyk | SCA / vulnerability scanning | Source code metadata, dependency data | Important | US/EU | ✅ | ✅ | ✅ | [Date] |

### Communication & Productivity

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-008 | Slack Technologies | Team communication | Business data, employee PII | Important | US (EU workspace opt-in) | ✅ | ✅ | ✅ | [Date] |
| S-009 | Google Workspace | Email, docs, calendar | All business data + employee PII | Critical | EU (data region policy) | ✅ | ✅ | ✅ | [Date] |
| S-010 | Notion Labs | Internal documentation | Internal/Confidential data | Standard | US | ✅ | ✅ | ✅ | [Date] |

### Customer & Support

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-011 | Zendesk | Customer support platform | Customer PII in tickets | Critical | EU (data residency add-on) | ✅ | ✅ | ✅ | [Date] |
| S-012 | HubSpot / Salesforce | CRM | Customer PII, sales data | Important | US/EU | ✅ | ✅ | ✅ | [Date] |
| S-013 | Intercom | In-app messaging | Customer PII | Important | EU | ✅ | ✅ | ✅ | [Date] |

### Payments & Finance

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-014 | Stripe | Payment processing | Customer financial data (tokenized) | Critical | US/EU | ✅ | ✅ | ✅ | [Date] | PCI-DSS compliant; no raw card data |
| S-015 | [Accounting tool] | Accounting/invoicing | Financial data | Important | | | | | [Date] |

### Monitoring & Observability

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-016 | Datadog / Grafana Cloud | Application monitoring | Application metrics, logs (may contain PII in logs) | Important | EU | ✅ | ✅ | ✅ | [Date] |
| S-017 | PagerDuty / OpsGenie | Incident alerting | Employee contact data, alert data | Standard | US | ✅ | ✅ | ✅ | [Date] |

### Security Tools

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-018 | CrowdStrike / Defender | EDR | Endpoint telemetry | Important | US/EU | ✅ | ✅ | ✅ | [Date] |
| S-019 | KnowBe4 / Proofpoint | Security awareness training | Employee data, phishing results | Standard | US/EU | ✅ | ✅ | ✅ | [Date] |

### HR & Recruitment

| # | Supplier | Service | Data Processed | Tier | Data Residency | ISO 27001 | SOC 2 | DPA | Last Review |
|---|---------|---------|----------------|------|---------------|-----------|-------|-----|------------|
| S-020 | [HR System] | HR, payroll | Employee PII, salary data | Critical | EU | | | ✅ | [Date] |

---

## Suppliers Under Review (Awaiting Questionnaire / Assessment)

| Supplier | Service | Expected Tier | Questionnaire Sent | Response Due |
|---------|---------|--------------|-------------------|-------------|
| | | | | |

---

## Offboarded Suppliers (Last 12 Months)

| Supplier | Service | Offboarded Date | Data Deletion Confirmed | Notes |
|---------|---------|----------------|------------------------|-------|
| | | | Y/N | |

---

## Annual Review Status

| Supplier | Tier | Last Review | Next Review | Status |
|---------|------|------------|------------|--------|
| All Critical suppliers | Critical | | | |

---

## Change Log

| Date | Changed By | Change |
|------|-----------|--------|
| | | Initial register created |
