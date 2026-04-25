# Business Continuity Plan (BCP)
**ISO 27001:2022 — A.5.29, A.5.30 | Version 1.0**
**Owner:** ISMS Owner / CEO
**Approved by:** [CEO Name] | [Date]
**Review cycle:** Annual + after any major incident or org change

---

## 1. Purpose and Scope

This Business Continuity Plan defines how [Organization Name] will maintain or restore critical business functions during and after a disruptive event. It covers all systems, processes, and personnel within the ISMS scope.

**This plan is activated when:**
- A critical system is unavailable for more than [X hours]
- A security incident (ransomware, major breach) causes operational disruption
- A key supplier experiences an outage affecting our services
- A physical event (fire, flood, loss of office) prevents normal operations
- Key personnel are unavailable (pandemic, mass illness, sudden departure)

---

## 2. Critical Business Functions

Identify what must keep running. Everything else can wait.

| Function | Description | Maximum Tolerable Downtime (MTD) | RTO | RPO | Owner |
|----------|-------------|----------------------------------|-----|-----|-------|
| Customer-facing SaaS platform | Core product serving paying customers | 4 hours | 2 hours | 1 hour | CTO |
| Customer authentication | Login / SSO for customer accounts | 4 hours | 1 hour | 30 min | DevOps Lead |
| Production database | Primary datastore (PostgreSQL/RDS) | 4 hours | 2 hours | 1 hour | DevOps Lead |
| Payment processing | Stripe payment collection | 8 hours | 4 hours | N/A (Stripe-managed) | Finance |
| Customer support | Ability to receive/respond to support requests | 24 hours | 4 hours | N/A | Support Lead |
| Internal communications | Slack / email for incident coordination | 2 hours | 1 hour | N/A | IT Lead |
| Source code repository | GitHub access for emergency patching | 8 hours | 2 hours | N/A | Engineering |
| Secrets/credentials store | Access to production credentials | 1 hour | 30 min | N/A | DevOps Lead |

> **MTD** = maximum time the function can be unavailable before causing unacceptable business damage.
> **RTO** = target time to restore function. Must be ≤ MTD.
> **RPO** = acceptable data loss window. Must be tested against actual backup frequency.

---

## 3. BCP Team

| Role | Name | Primary Contact | Backup Contact |
|------|------|-----------------|----------------|
| BCP Coordinator (ISMS Owner) | [Name] | [phone/email] | [backup name] |
| Technical Recovery Lead (CTO) | [Name] | [phone/email] | [backup name] |
| Communications Lead (CEO) | [Name] | [phone/email] | [backup name] |
| Customer Success Lead | [Name] | [phone/email] | [backup name] |
| HR Lead | [Name] | [phone/email] | [backup name] |

**Out-of-hours contact tree:**
1. BCP Coordinator → Technical Recovery Lead → CEO
2. If primary unavailable: use backup contacts above
3. Last resort: [personal mobile list in secure location]

---

## 4. Threat Scenarios and Recovery Strategies

### 4.1 Scenario: Cloud Provider Outage (AWS us-east-1)

**Likelihood:** Low | **Impact:** Critical

| Action | Owner | Target Time |
|--------|-------|-------------|
| Confirm AWS service health dashboard | On-call engineer | T+15 min |
| Notify BCP Coordinator | On-call engineer | T+30 min |
| Assess impact: which services are affected? | Technical Lead | T+1 hour |
| Activate DR region (eu-west-1) if RTO requires it | DevOps Lead | T+2 hours |
| Post customer status update | Communications Lead | T+1 hour |
| Notify affected enterprise customers directly | Customer Success | T+2 hours |

**Recovery strategy:** Multi-region failover (see DR Runbook for step-by-step).
**Activation threshold:** Primary region unavailable >1 hour with no ETA.

---

### 4.2 Scenario: Ransomware / Major Security Incident

**Likelihood:** Low | **Impact:** Critical

| Action | Owner | Target Time |
|--------|-------|-------------|
| Invoke IRP (Incident Response Plan) | ISMS Owner | T+30 min |
| Isolate affected systems | DevOps Lead | T+1 hour |
| Assess backup integrity (are backups unaffected?) | DevOps Lead | T+2 hours |
| Decision: restore from backup or DR? | CTO + CEO | T+3 hours |
| Begin restoration (see DR Runbook) | DevOps Lead | T+4 hours |
| Customer communication | Communications Lead | T+2 hours |
| GDPR 72-hour notification assessment | ISMS Owner | T+2 hours |

**Recovery strategy:** Restore from AWS Backup (verified clean) to new infrastructure. Do not restore to compromised infrastructure — rebuild from IaC (Terraform).

---

### 4.3 Scenario: Key Personnel Unavailability

**Likelihood:** Medium | **Impact:** High

| Personnel Lost | Critical Functions at Risk | Mitigation |
|---------------|--------------------------|-----------|
| CTO / Lead DevOps | Production deployments, incident response | Document runbooks; cross-train 2nd engineer on all critical procedures |
| CEO | External communications, legal decisions | BCP Coordinator takes comms; legal counsel on retainer |
| On-call engineer | After-hours incident detection | Rotation with minimum 2 people; PagerDuty escalation to backup |

**Policy:** All critical procedures must be documented in runbooks (see `/04_Infrastructure_Compliance/`). No single point of knowledge.

---

### 4.4 Scenario: Key Supplier Outage

| Supplier | Service | Alternatives | RTO Impact |
|----------|---------|-------------|-----------|
| AWS (primary cloud) | All infrastructure | Azure DR region (see DR Runbook) | +4 hours |
| GitHub | Source code, CI/CD | Local clones; manual deployment from verified artifact | +8 hours |
| Slack | Internal comms | WhatsApp group (pre-configured), email | Minimal |
| Zendesk | Customer support | Email direct (support@company.com) | Minimal |
| Stripe | Payment processing | PayPal backup (if configured); defer new sign-ups | 8 hours for new payments |
| Auth0/Okta | Customer SSO | Fall back to local auth (if implemented) | High |
| Cloudflare | CDN / DDoS protection | DNS failover to direct AWS ALB; increased DDoS risk | 30 min |

---

### 4.5 Scenario: Loss of Office / Physical Location

**Impact for remote-first companies:** Minimal — already remote.
**Impact for office-based companies:**

| Action | Owner | Notes |
|--------|-------|-------|
| Activate remote work policy | HR | All staff work from home |
| Confirm all staff can access systems remotely (VPN/SSO) | IT Lead | Test annually |
| Redirect calls and postal mail | Office Manager | Virtual business address |
| Notify customers if service affected | Communications Lead | Only if actually affected |

**Physical asset recovery:** Laptops are encrypted (FDE) and managed via MDM. Data is in cloud. Loss of office = loss of physical assets only.

---

## 5. Communication Plan

### Internal Communication

| Situation | Channel | Owner | Audience |
|-----------|---------|-------|---------|
| BCP activated | Email + Slack DM to BCP team | BCP Coordinator | BCP team |
| Status updates (every 2 hours) | Slack #incident-status | BCP Coordinator | All staff |
| All-clear | Email from CEO | CEO | All staff |

**If Slack is unavailable:** WhatsApp group "[Company] Emergency" — pre-create this now.
**If email is unavailable:** Phone tree (see contact list above).

### External Communication

| Situation | Channel | Owner | Timing |
|-----------|---------|-------|--------|
| Platform outage >30 min | Status page (statuspage.io) | On-call | Within 30 min |
| Major incident / data breach | Customer email | CEO draft; Legal approve | ASAP, no speculation |
| Enterprise customer direct call | Phone | Customer Success Lead | Within 2 hours |
| Media enquiry | Press email; no ad-hoc comment | CEO only | Within 24 hours |
| Regulatory notification (GDPR) | ICO / DPA formal notification | ISMS Owner + Legal | Within 72 hours of awareness |

**Customer communication template (outage):**
> We are currently experiencing [service disruption / degraded performance] affecting [specific feature or all services]. Our team is actively investigating. We will provide an update by [time]. We apologize for the inconvenience. Current status: [link to status page].

**Do not:** Speculate on cause, quote a restore time you're not confident in, or use the word "breach" unless confirmed.

---

## 6. Recovery Priorities

When resources are limited, restore in this order:

| Priority | System / Function | Reason |
|---------|------------------|--------|
| 1 | Authentication service | Nothing works without login |
| 2 | Production database (read-only) | Data access for triage |
| 3 | Core API / application server | Customer-facing functionality |
| 4 | Payment processing | Revenue protection |
| 5 | Monitoring and alerting | Visibility to verify recovery |
| 6 | Customer support tooling | Communication |
| 7 | Internal tools (Slack, Notion, etc.) | Team coordination |
| 8 | Development environments | Can wait |

---

## 7. Dependencies and Single Points of Failure

| Dependency | Criticality | Mitigation |
|-----------|-------------|-----------|
| AWS account root credentials | Critical | Hardware MFA + locked safe; backup in 1Password Shared |
| Production DB master credentials | Critical | AWS Secrets Manager + break-glass procedure |
| Domain registrar access | High | Document login; ensure CEO has backup access |
| SSL/TLS certificates | High | Auto-renew via ACM; alert on <30 days expiry |
| DNS (Route 53 or Cloudflare) | Critical | Failover routing configured |
| CI/CD pipeline (GitHub Actions) | High | Manual deployment procedure documented |
| Single on-call engineer | High | Minimum 2-person rotation |

---

## 8. BCP Testing and Maintenance

### Test Schedule

| Test Type | Frequency | Participants | Last Tested | Next Test |
|-----------|-----------|-------------|-------------|-----------|
| Tabletop exercise (BCP scenarios) | Annual | BCP team | [date] | [date+1yr] |
| Backup restore test | Quarterly | DevOps Lead | [date] | [date+3mo] |
| DR failover test | Annual | DevOps + CTO | [date] | [date+1yr] |
| Communication tree test | Annual | All BCP team | [date] | [date+1yr] |
| Remote work capability test | Annual | All staff | [date] | [date+1yr] |

### After Each Test

1. Document results: what worked, what didn't, how long each step took
2. Update RTO/RPO actuals vs. targets (see `rto_rpo_worksheet.md`)
3. Update this plan if procedures were found to be wrong
4. Update DR runbook if technical steps needed correction
5. Log test as evidence (required for A.5.30 audit evidence)

---

## 9. Plan Maintenance

| Trigger | Action Required |
|---------|----------------|
| Major organizational change (merger, pivot, headcount >50%) | Full review |
| New critical supplier added | Update section 4.4 |
| New critical system deployed | Update section 2 (critical functions) |
| After any actual BCP invocation | Lessons-learned review + update |
| Annual surveillance | Review + re-approve |

**Document owner:** ISMS Owner
**Review date:** [Date of next review]
**Version:** 1.0

---

## 10. Plan Approval and Distribution

| Role | Name | Signature | Date |
|------|------|-----------|------|
| CEO | | | |
| ISMS Owner | | | |
| CTO | | | |

**Distribution:** This plan is stored in [ISMS document store location] and shared with BCP team members. Hard copy stored in [location] for use if systems are unavailable.

**Confidentiality:** This document is classified **CONFIDENTIAL — INTERNAL**. Do not share externally without CEO approval.
