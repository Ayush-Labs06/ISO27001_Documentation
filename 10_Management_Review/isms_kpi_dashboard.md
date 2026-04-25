# ISMS KPI Dashboard
**ISO 27001:2022 — Clause 9.1 | Version 1.0**
**Owner:** ISMS Owner
**Update frequency:** Monthly | **Report to management:** Quarterly / at Management Review

---

## How to Use

Update each metric monthly. Use the Trend column (↑ improving / ↓ worsening / → stable). Highlight any metric in the "At Risk" or "Missed" columns for management attention. Present the full dashboard at each management review.

---

## KPI Category 1: Security Incidents

| Metric | Target | Jan | Feb | Mar | Q1 | Trend | Status |
|--------|--------|-----|-----|-----|----|-------|--------|
| P1 incidents (this period) | 0 | | | | | | |
| P2 incidents (this period) | ≤1 | | | | | | |
| P3 incidents (this period) | ≤5 | | | | | | |
| P4 incidents (this period) | Informational | | | | | | |
| Mean time to detect (MTTD) for P1/P2 | <1 hour | | | | | | |
| Mean time to contain (MTTC) for P1 | <2 hours | | | | | | |
| Post-incident reviews completed on time | 100% | | | | | | |
| GDPR breach notifications sent | 0 (target: none needed) | | | | | | |

**12-Month Incident Trend (running total):**

| Year | P1 | P2 | P3 | P4 | Notes |
|------|----|----|----|----|-------|
| [Year-1] | | | | | |
| [Year] | | | | | |

---

## KPI Category 2: Vulnerability Management

| Metric | Target | Jan | Feb | Mar | Q1 | Trend | Status |
|--------|--------|-----|-----|-----|----|-------|--------|
| Critical vulns open >3 days (CISA KEV) | 0 | | | | | | |
| Critical vulns open >7 days (non-KEV) | 0 | | | | | | |
| High vulns open >30 days | 0 | | | | | | |
| Medium vulns open >90 days | ≤5 | | | | | | |
| Patch compliance — Critical (7-day SLA) | 100% | | | | | | |
| Patch compliance — High (30-day SLA) | ≥95% | | | | | | |
| Container images with known critical CVEs in prod | 0 | | | | | | |
| SAST findings blocking pipeline (unresolved) | 0 | | | | | | |

```bash
# Pull critical open vulns from Security Hub
aws securityhub get-findings \
  --filters '{"SeverityLabel":[{"Value":"CRITICAL","Comparison":"EQUALS"}],"RecordState":[{"Value":"ACTIVE","Comparison":"EQUALS"}]}' \
  --query 'Findings[*].[Title,CreatedAt,ProductName]' \
  --output table

# Pull Inspector findings for EC2/ECR
aws inspector2 list-findings \
  --filter-criteria '{"severity":[{"comparison":"EQUALS","value":"CRITICAL"}]}' \
  --query 'findings[*].[title,firstObservedAt,type]'
```

---

## KPI Category 3: Access Control and Identity

| Metric | Target | Jan | Feb | Mar | Q1 | Trend | Status |
|--------|--------|-----|-----|-----|----|-------|--------|
| Users without MFA (any critical system) | 0 | | | | | | |
| IAM users with long-lived access keys (>90 days) | 0 | | | | | | |
| Access reviews completed on time (quarterly) | 100% | | | | | | |
| Overdue access deprovisioning (>24h after termination) | 0 | | | | | | |
| Privileged accounts with JIT access configured | 100% | | | | | | |
| Service accounts reviewed quarterly | 100% | | | | | | |

```bash
# Check for IAM users with old access keys
aws iam generate-credential-report && sleep 10
aws iam get-credential-report --query 'Content' --output text | base64 -d | \
  awk -F, 'NR>1 && $10!="N/A" && ($10 > 90) {print $1, $10}'

# Check MFA not enabled
aws iam generate-credential-report && sleep 10
aws iam get-credential-report --query 'Content' --output text | base64 -d | \
  awk -F, 'NR>1 && $8=="false" {print $1, "NO MFA"}'
```

---

## KPI Category 4: Security Awareness and Training

| Metric | Target | Jan | Feb | Mar | Q1 | Trend | Status |
|--------|--------|-----|-----|-----|----|-------|--------|
| Training completion rate (all mandatory) | ≥95% | | | | | | |
| New employee training within 5 days | 100% | | | | | | |
| Phishing simulation click rate | <10% | | | | Q sim | | |
| Phishing simulation report rate | ≥20% | | | | Q sim | | |
| Remedial training completion (within 5 days) | 100% | | | | | | |
| AUP signature completion | 100% | | | | | | |

---

## KPI Category 5: Internal Audit and Nonconformities

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Annual | Status |
|--------|--------|----|----|----|----|----|--------|
| Internal audits completed vs. planned | 100% | | | | | | |
| Major NCs raised | Informational | | | | | | |
| Minor NCs raised | Informational | | | | | | |
| NCs closed within target timeframe | 100% | | | | | | |
| Overdue NCs (>target closure date) | 0 | | | | | | |
| Repeat NCs (same finding in consecutive audits) | 0 | | | | | | |

---

## KPI Category 6: Supplier and Third-Party

| Metric | Target | Q1 | Q2 | Q3 | Q4 | Status |
|--------|--------|----|----|----|----|--------|
| Tier 1 suppliers with current assessment | 100% | | | | | |
| Tier 2 suppliers with current assessment | ≥80% | | | | | |
| Suppliers with DPA in place | 100% (Tier 1&2) | | | | | |
| Supplier assessments overdue | 0 | | | | | |
| Supplier security incidents affecting us | 0 preferred | | | | | |

---

## KPI Category 7: Business Continuity

| Metric | Target | Current | Last Tested | Next Test | Status |
|--------|--------|---------|------------|----------|--------|
| Backup test (restore success) | Quarterly | | | | |
| DR test completed annually | Annual | | | | |
| RTO achieved vs. target (last DR test) | ≤2 hours | | | | |
| RPO achieved vs. target (last DR test) | ≤1 hour | | | | |
| BCP reviewed/updated | Annual | | | | |

---

## KPI Category 8: Risk

| Metric | Target | Current | Trend | Status |
|--------|--------|---------|-------|--------|
| Risks above acceptance threshold (untreated) | 0 | | | |
| Risks in treatment (on-schedule) | 100% | | | |
| Risk register reviewed | Quarterly + triggers | | | |
| New risks identified since last review | Informational | | | |
| Average residual risk score (all risks) | Decreasing | | | |

---

## Dashboard Snapshot — Management Review Pack

*This section is the one-page summary for management review. Pull from the full dashboard above.*

**Period:** [Q1/Q2/Q3/Q4 Year] | **Prepared by:** [ISMS Owner] | **Date:** [Date]

| Domain | Key Metric | Status |
|--------|-----------|--------|
| Incidents | [N] incidents this period (P1: [N], P2: [N]) | ☐ Green ☐ Amber ☐ Red |
| Vulnerabilities | Critical/High vulns within SLA: [X]% | ☐ Green ☐ Amber ☐ Red |
| Access Control | MFA gaps: [N] \| Access reviews on time: [X]% | ☐ Green ☐ Amber ☐ Red |
| Training | Completion: [X]% \| Phishing click: [X]% | ☐ Green ☐ Amber ☐ Red |
| Audit / NCs | Open NCs: [N] (Major: [N]) | ☐ Green ☐ Amber ☐ Red |
| Suppliers | Assessments current: [X]% | ☐ Green ☐ Amber ☐ Red |
| BCP/DR | Last DR test RTO: [X]h (target: 2h) | ☐ Green ☐ Amber ☐ Red |
| Risk | Untreated above-threshold risks: [N] | ☐ Green ☐ Amber ☐ Red |

**Overall ISMS Health:** ☐ Green (on track) ☐ Amber (attention needed) ☐ Red (management action required)

**Top 3 priorities for next period:**
1.
2.
3.
