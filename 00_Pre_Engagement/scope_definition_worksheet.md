# ISMS Scope Definition Worksheet
**ISO 27001:2022 — Clause 4.3**

The scope statement is the legal boundary of your ISMS. Everything inside is audited. Everything outside is explicitly excluded. Get this wrong and you either over-commit (audit hell) or under-commit (certification is worthless to customers).

---

## Part 1 — Organization Context

| Field | Answer |
|-------|--------|
| Legal entity name | |
| Trading name (if different) | |
| Primary industry / sector | |
| Number of employees (FTE) | |
| Number of contractors with system access | |
| Headquarters location | |
| Other office locations | |
| Primary business activities | |
| Key products / services to be in scope | |

---

## Part 2 — Drivers for Certification

Why do they actually need ISO 27001? (Check all that apply)

- [ ] Customer / enterprise sales requirement
- [ ] Procurement / tender requirement (government or regulated sector)
- [ ] Board / investor requirement
- [ ] Regulatory requirement (specify: _______________)
- [ ] Insurance requirement
- [ ] Internal maturity goal
- [ ] Competitive differentiation
- [ ] M&A / due diligence preparation
- [ ] Other: _______________

> **Why this matters:** The driver changes the urgency, the scope, and what you emphasize. A sales-driven cert focuses on Annex A controls that customer security questionnaires ask about. A regulatory cert needs specific clause alignment.

---

## Part 3 — Services / Products in Scope

List each product or service and decide: **IN** or **OUT** of ISMS scope.

| Product / Service | In Scope? | Reason |
|-------------------|-----------|--------|
| | IN / OUT | |
| | IN / OUT | |
| | IN / OUT | |
| | IN / OUT | |

**Guidance:** If a product processes customer data or is what customers are buying, it goes IN. Legacy or sunset products can often go OUT — document the rationale.

---

## Part 4 — Systems & Infrastructure in Scope

### Cloud Accounts

| Account / Subscription | Provider | Environment | In Scope? |
|------------------------|----------|-------------|-----------|
| | AWS / Azure / GCP | Prod / Dev / Staging | IN / OUT |
| | | | |

### SaaS Tools (used to process in-scope data)

| Tool | Category | Data processed | In Scope? |
|------|----------|----------------|-----------|
| GitHub / GitLab | Source code | Source code, secrets | IN |
| Slack / Teams | Comms | Business data | IN |
| Notion / Confluence | Docs | Business data | IN |
| Jira / Linear | Issue tracking | Project data | IN |
| HubSpot / Salesforce | CRM | Customer PII | IN |
| Google Workspace / M365 | Productivity | All email + docs | IN |
| Vercel / Netlify | Hosting | App delivery | IN |
| Stripe / Paddle | Payments | Card data (note: PCI scope) | IN |
| | | | |

### On-Premises / Physical

| Asset | Location | In Scope? |
|-------|----------|-----------|
| Office servers | HQ | IN / OUT |
| Employee laptops | All offices | IN |
| Network equipment | HQ | IN |

---

## Part 5 — Locations in Scope

| Location | Type | In Scope? | Reason |
|----------|------|-----------|--------|
| HQ office | Office | IN | Primary operations |
| Remote employee home offices | Remote | IN (note) | Covered by AUP, not physically audited |
| Data centre / colocation | DC | IN / OUT | |
| Cloud regions | Virtual | IN | Covered by cloud provider shared responsibility |

> **IRL Note:** Remote/home workers are covered by policy and asset controls, not physical site audits. The auditor won't inspect employee bedrooms. Make sure your AUP covers home office security requirements.

---

## Part 6 — Explicitly Out of Scope

Document what is excluded and **why** — auditors will challenge vague exclusions.

| Item | Exclusion Rationale |
|------|---------------------|
| Legacy product [name] — sunset Q2 | No customer data, no active development |
| Subsidiary [name] | Separate legal entity, different systems |
| Development environment | No production data; developer workstations covered by AUP |
| | |

---

## Part 7 — Interfaces & Dependencies

Map what crosses the scope boundary — these are the highest-risk points and what auditors probe.

| Interface | Direction | What crosses | Control mechanism |
|-----------|-----------|-------------|-------------------|
| Customers → SaaS platform | Inbound | Data, API calls | TLS, auth, WAF |
| Platform → AWS RDS | Internal | Customer data | VPC, encryption |
| AWS → Stripe | Outbound | Payment tokens | API key, TLS |
| Employees → GitHub | Internal | Source code | SSO + MFA, branch protection |
| CI/CD → Production | Internal | Deployments | Signed artifacts, OIDC auth |
| Support team → Zendesk | Outbound | Customer tickets | SSO, DLP |

---

## Part 8 — Draft Scope Statement

Use this template to write the formal ISMS scope statement that goes in the ISMS Manual (Clause 4.3):

> **[Organization Name]'s** Information Security Management System covers the design, development, delivery, and support of **[product/service name]**, including the underlying cloud infrastructure, internal IT systems, and supporting business processes operated from **[location(s)]**.
>
> The ISMS addresses the protection of information assets belonging to [Organization Name], its customers, and its suppliers that are processed, stored, or transmitted in connection with the in-scope services.
>
> The following are explicitly excluded from the ISMS scope: **[list exclusions with rationale]**.

---

## Checklist — Before Finalizing Scope

- [ ] Scope covers at least one revenue-generating or customer-facing service
- [ ] All cloud accounts processing customer data are listed
- [ ] Exclusions are documented with rationale (not just "not in scope")
- [ ] Key interfaces across the boundary are identified
- [ ] Scope statement is reviewed and signed by top management
- [ ] Scope is realistic — you can implement and evidence controls for everything in scope within the project timeline
