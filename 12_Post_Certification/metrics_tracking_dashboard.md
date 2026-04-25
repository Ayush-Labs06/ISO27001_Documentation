# Metrics Tracking Dashboard
**ISO 27001:2022 — Clause 9.1, 10.2 | Version 1.0**
**Owner:** ISMS Owner
**Update frequency:** Monthly | **Management review input:** Quarterly

---

## Instructions

This dashboard is the single source of truth for ISMS health metrics post-certification. Update monthly. Use it as the primary input to management reviews. Archive each quarterly snapshot for 3 years (auditors may pull historical data at surveillance).

Color coding for Status column:
- **Green** — On target
- **Amber** — Within 20% of target / one missed period
- **Red** — Beyond 20% miss / systemic failure

---

## Section 1: Security Incidents

| Metric | Target | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec | YTD |
|--------|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| P1 incidents | 0 | | | | | | | | | | | | | |
| P2 incidents | ≤1/quarter | | | | | | | | | | | | | |
| P3 incidents | Informational | | | | | | | | | | | | | |
| P4 / near-miss | Informational | | | | | | | | | | | | | |
| MTTD P1/P2 (hours) | <1h | | | | | | | | | | | | | |
| MTTC P1 (hours) | <2h | | | | | | | | | | | | | |
| PIR on time | 100% | | | | | | | | | | | | | |
| GDPR notifications | 0 target | | | | | | | | | | | | | |

**Incident Trend Chart (12-Month Rolling):**

```
P1+P2 Incidents per Quarter
Q1: ████ (4)
Q2: ██ (2)
Q3: █ (1)
Q4: (fill in)
Target: ≤2/quarter
```

---

## Section 2: Vulnerability Management

| Metric | Target | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec |
|--------|--------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Critical vulns open >3d (KEV) | 0 | | | | | | | | | | | | |
| Critical vulns open >7d | 0 | | | | | | | | | | | | |
| High vulns open >30d | 0 | | | | | | | | | | | | |
| Patch compliance — critical | 100% | | | | | | | | | | | | |
| Patch compliance — high | ≥95% | | | | | | | | | | | | |
| Container images: critical CVEs in prod | 0 | | | | | | | | | | | | |
| Pen test findings (critical/high open) | 0 / ≤3 | | | | | | | | | | | | |

**Data sources:**
```bash
# Monthly: export Inspector findings
aws inspector2 list-findings \
  --filter-criteria '{"severity":[{"comparison":"EQUALS","value":"CRITICAL"},{"comparison":"EQUALS","value":"HIGH"}]}' \
  --query 'findings[*].[title,severity.label,firstObservedAt,status]' \
  --output table > vuln_report_$(date +%Y%m).txt

# Patch compliance
aws ssm list-compliance-summaries \
  --filters Key=ComplianceType,Values=Patch \
  --query 'ComplianceSummaryItems[*].[InstanceId,Status]'
```

---

## Section 3: Access Control and Identity

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Annual Status |
|--------|--------|----|----|----|----|--------------|
| Users without MFA (critical systems) | 0 | | | | | |
| IAM keys >90 days (no rotation) | 0 | | | | | |
| Access reviews completed on time | 100% | | | | | |
| Overdue deprovisioning (>24h post-term) | 0 | | | | | |
| Privileged accounts with JIT | 100% | | | | | |
| Service account quarterly review | 100% | | | | | |

**Data sources:**
```bash
# Monthly MFA check
aws iam generate-credential-report && sleep 10
aws iam get-credential-report --query 'Content' --output text | \
  base64 -d | awk -F, 'NR>1 && $4=="true" && $8=="false" {print $1, "MFA disabled"}'

# Old access keys
aws iam get-credential-report --query 'Content' --output text | \
  base64 -d | awk -F, 'NR>1 && $14!="N/A" {print $1, $14}'
```

---

## Section 4: Awareness and Training

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Annual |
|--------|--------|----|----|----|----|--------|
| Mandatory training completion (all staff) | ≥95% | | | | | |
| New hire training within 5 days | 100% | | | | | |
| AUP signed (all staff) | 100% | | | | | |
| Phishing click rate | <10% | | | | | |
| Phishing report rate | ≥20% | | | | | |
| Remedial training completion (≤5 days) | 100% | | | | | |

**Phishing Trend (Quarterly):**

| Quarter | Click Rate | Target | Status | Report Rate | Target |
|---------|-----------|--------|--------|------------|--------|
| Q1 | | <10% | | | ≥20% |
| Q2 | | | | | |
| Q3 | | | | | |
| Q4 | | | | | |

---

## Section 5: Audit and Nonconformities

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Annual |
|--------|--------|----|----|----|----|--------|
| Internal audits completed vs. planned | 100% | | | | | |
| Major NCs raised (period) | Informational | | | | | |
| Minor NCs raised (period) | Informational | | | | | |
| NCs closed within target timeframe | 100% | | | | | |
| Overdue NCs (past due date) | 0 | | | | | |
| Repeat NCs (same finding twice) | 0 | | | | | |

---

## Section 6: Supplier Management

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Status |
|--------|--------|----|----|----|----|--------|
| Tier 1 supplier assessments current | 100% | | | | | |
| Tier 2 supplier assessments current | ≥80% | | | | | |
| DPAs in place (Tier 1+2) | 100% | | | | | |
| Assessments overdue | 0 | | | | | |
| Supplier incidents affecting us | 0 preferred | | | | | |
| New Tier 1 suppliers assessed before onboarding | 100% | | | | | |

---

## Section 7: BCP and DR

| Metric | Target | Last Measurement | Status |
|--------|--------|-----------------|--------|
| Backup restore test (quarterly) | Pass quarterly | | |
| DR failover test (annual) | Pass annually | | |
| RTO achieved (last test) | ≤2 hours | | |
| RPO achieved (last test) | ≤1 hour | | |
| BCP reviewed/approved | Annual | | |
| Tabletop exercise (annual) | Completed | | |

---

## Section 8: Risk

| Metric | Target | Q1 | Q2 | Q3 | Q4 |
|--------|--------|----|----|----|----|
| Risks above threshold (untreated) | 0 | | | | |
| Risk treatments on schedule | 100% | | | | |
| Risk register reviewed | Quarterly | | | | |
| Avg residual risk score (all risks) | Decreasing trend | | | | |
| New risks added (emerging threats) | Informational | | | | |

---

## Quarterly Executive Snapshot

Copy this section into the management review pack each quarter.

**Quarter:** [Q1/Q2/Q3/Q4 Year] | **Date:** [Date]

| Domain | RAG | Key Metric | Value | vs Target |
|--------|-----|-----------|-------|-----------|
| Incidents | 🟢/🟡/🔴 | P1+P2 this quarter | | |
| Vulnerabilities | | Critical vulns in SLA | | |
| Access Control | | MFA compliance | | |
| Training | | Completion rate | | |
| Phishing | | Click rate | | |
| Audit | | Open major NCs | | |
| Suppliers | | Tier 1 assessed | | |
| BCP/DR | | Last DR test RTO | | |
| Risk | | Above-threshold untreated | | |

**Overall ISMS Health:** 🟢 Green / 🟡 Amber / 🔴 Red

**ISMS Owner sign-off:** ___________________ **Date:** ___________________
