# Stage 2 Audit Readiness Checklist
**ISO 27001:2022 — Certification Preparation | Version 1.0**
**Owner:** ISMS Owner
**Purpose:** Ensure all evidence is in place before the Stage 2 (evidence sampling) audit

---

## What Happens at Stage 2

Stage 2 is the evidence audit. The CB auditor will:
1. Sample records to verify that controls are actually operating (not just documented)
2. Interview staff across multiple functions
3. Observe technical controls and system configurations
4. Verify that corrective actions from Stage 1 have been addressed
5. Test that processes are embedded (not just written down)

**Duration:** 1–3 days depending on organization size. For a startup of <50 people: typically 1.5 days.
**Result:** Certificate granted (possibly with minor conditions) OR major NCs requiring re-audit.

---

## Evidence Readiness — By Control Domain

### Access Control and Identity

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| IAM credential report (within 7 days) | ☐ | ☐ | | ☐ |
| Access review records (last 2 quarters) | ☐ | ☐ | | ☐ |
| MFA enforcement evidence (Entra/Okta settings screenshot) | ☐ | ☐ | | ☐ |
| Onboarding records (2-3 recent hires with checklist) | ☐ | ☐ | | ☐ |
| Offboarding records (2-3 recent leavers with checklist) | ☐ | ☐ | | ☐ |
| Privileged access log (PAM / PIM activation records) | ☐ | ☐ | | ☐ |
| SSO/IdP configuration showing least privilege groups | ☐ | ☐ | | ☐ |

### Infrastructure and Technical Controls

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| CloudTrail enabled — all regions (AWS Config or console screenshot) | ☐ | ☐ | | ☐ |
| GuardDuty enabled and findings reviewed (findings + review record) | ☐ | ☐ | | ☐ |
| Security Hub score (or equivalent) | ☐ | ☐ | | ☐ |
| Vulnerability scan results (Inspector / Trivy) — last 30 days | ☐ | ☐ | | ☐ |
| Patch compliance report (SSM Patch Manager or equivalent) | ☐ | ☐ | | ☐ |
| S3 account-level block public access — enabled (screenshot) | ☐ | ☐ | | ☐ |
| RDS encryption at rest — enabled for prod DB | ☐ | ☐ | | ☐ |
| Backup test record (last restore test with outcome) | ☐ | ☐ | | ☐ |
| MDM compliance report (all enrolled devices — encryption + lock screen) | ☐ | ☐ | | ☐ |
| Secrets scanning in CI/CD (last pipeline run showing check passing) | ☐ | ☐ | | ☐ |
| KMS key usage / rotation evidence | ☐ | ☐ | | ☐ |
| Network architecture diagram (current) | ☐ | ☐ | | ☐ |

### Incident Response

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| Incident log (at least 6-12 months of records, including P3/P4) | ☐ | ☐ | | ☐ |
| At least 1 completed post-incident review | ☐ | ☐ | | ☐ |
| Tabletop exercise record (agenda + attendance + findings) | ☐ | ☐ | | ☐ |
| IRP tested or reviewed within 12 months | ☐ | ☐ | | ☐ |

### Supplier Management

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| Subprocessor register (all Tier 1/2 suppliers, review dates) | ☐ | ☐ | | ☐ |
| At least 2 completed supplier security assessments (Tier 1) | ☐ | ☐ | | ☐ |
| DPA records for Tier 1/2 suppliers | ☐ | ☐ | | ☐ |
| Sample contract with security clauses | ☐ | ☐ | | ☐ |

### Training and Awareness

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| Training completion tracker (all staff, last 12 months) | ☐ | ☐ | | ☐ |
| Training content evidence (LMS screenshots or module descriptions) | ☐ | ☐ | | ☐ |
| Phishing simulation results (last 2-4 simulations with metrics) | ☐ | ☐ | | ☐ |
| Signed AUP records (all current staff) | ☐ | ☐ | | ☐ |
| New employee onboarding records (2-3 samples) | ☐ | ☐ | | ☐ |

### Risk and Business Continuity

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| Current risk register (signed off, within 12 months) | ☐ | ☐ | | ☐ |
| Risk treatment plan with completed actions | ☐ | ☐ | | ☐ |
| DR test record (last test date, RTO/RPO achieved) | ☐ | ☐ | | ☐ |
| Backup test record (last successful restore) | ☐ | ☐ | | ☐ |

### Governance

| Evidence Required | Exists | Current | Location | Auditor-Ready |
|-----------------|--------|---------|---------|--------------|
| Management review minutes (at least 1 in last 12 months, CEO attending) | ☐ | ☐ | | ☐ |
| Internal audit report (at least 1 in last 12 months) | ☐ | ☐ | | ☐ |
| Corrective action tracker with at least some closed NCs | ☐ | ☐ | | ☐ |
| IS objectives with measurement evidence | ☐ | ☐ | | ☐ |

---

## Staff Interview Preparation

The auditor will interview staff beyond the ISMS Owner. Prepare the following people:

### CEO / Top Management
- "How do you demonstrate leadership and commitment to information security?"
- "What are the IS objectives and are they being met?"
- "What was the most significant security incident you had this year?"
- "Who is responsible for information security decisions?"

### DevOps Lead / Technical Staff
- "Walk me through how a new change gets deployed to production."
- "How do you manage access to the production environment?"
- "What happens when you find a critical vulnerability?"
- "How do you handle secrets and credentials?"

### Random Employee (non-technical)
- "How do you report a security incident?"
- "What would you do if you clicked a suspicious link?"
- "What is the data classification policy?" (They don't need to quote it — just know it exists and roughly what it means)
- "Can you store customer data on your personal device?"

### HR
- "What is the process when an employee leaves the company?"
- "How do you ensure employees understand their security responsibilities?"
- "What is your background check process?"

---

## Day-Before Audit Checklist

- [ ] Evidence folder organised and accessible (not buried in Notion or random Google Drives)
- [ ] Stage 1 findings — all addressed, responses documented
- [ ] All documents have current version dates (nothing expired/showing as due for review)
- [ ] ISMS Owner has a clean summary of: scope, key controls, incident history, audit history
- [ ] CEO calendar cleared for Stage 2 day (CEO must be available for interview)
- [ ] Technical staff briefed and available for interviews/demonstrations
- [ ] AWS console access available for live demonstration (read-only view is fine)
- [ ] Auditor's laptop access / screen-sharing configured for remote audit
- [ ] Offline backup of evidence pack (if remote audit — screenshare issues happen)

---

## After Stage 2

**If certificate granted (possibly with conditions):**
- Address any minor conditions within agreed timeframe
- Register the certificate — it's typically valid for 3 years with annual surveillance audits
- Announce to customers/prospects (a key business value of the exercise)

**If major NC found:**
- Understand exactly what the finding is
- Implement corrective action
- CB will either: (a) schedule a focused re-visit, or (b) require full Stage 2 repeat in worst case
- Major NCs at Stage 2 are rare if Stage 1 was taken seriously — address Stage 1 findings properly

**Recertification (year 3):** Full re-audit (similar to initial). Surveillance audits (years 1 and 2) are lighter — typically 1 day, focused on areas that changed or had previous findings.
