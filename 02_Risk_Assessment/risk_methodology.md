# Information Security Risk Assessment Methodology
**ISO 27001:2022 — Clause 6.1.2 | Version 1.0**

**Document Owner:** [CISO/CTO Name]
**Review Date:** [Annual]
**Approved by:** [CEO Name] | [Date]

---

## 1. Purpose

This document defines the methodology used by [Organization Name] to identify, analyse, evaluate, and treat information security risks. It establishes criteria for performing risk assessments and ensures consistent, repeatable, and comparable results across all assessments.

---

## 2. Scope

This methodology applies to all information assets within the ISMS scope as defined in the ISMS Scope Statement. It covers risks to the confidentiality, integrity, and availability of information assets.

---

## 3. Risk Assessment Approach

We use an **asset-based risk assessment** approach:

```
ASSET → THREAT → VULNERABILITY → RISK → CONTROL → RESIDUAL RISK
```

Each risk is associated with a specific information asset, a threat that could exploit a vulnerability, and the resulting impact on the CIA triad.

---

## 4. Risk Identification

### 4.1 Asset Identification
All in-scope assets are captured in the Asset Register. Assets are categorized as:
- **Primary assets:** Information (data, databases, software) and business processes
- **Supporting assets:** Hardware, networks, infrastructure, people, locations

### 4.2 Threat Identification
For each asset, identify applicable threats from the Threat & Vulnerability Library. Common threat categories:
- Malicious external actor (hacker, nation-state, competitor)
- Malicious insider
- Accidental/human error
- Natural disaster / environmental
- System failure / technical failure
- Supplier/third-party failure

### 4.3 Vulnerability Identification
For each threat, identify vulnerabilities that could be exploited:
- Technical: unpatched software, misconfiguration, weak authentication
- Procedural: no backup test, no incident process, no access review
- Human: lack of training, social engineering susceptibility

---

## 5. Risk Analysis

### 5.1 Likelihood Scale

| Score | Level | Definition |
|-------|-------|-----------|
| 1 | Rare | Unlikely to occur; no known incidents in sector |
| 2 | Unlikely | Could occur; infrequent sector incidents |
| 3 | Possible | Might occur; occasional sector incidents |
| 4 | Likely | Will probably occur; regular sector incidents |
| 5 | Almost Certain | Expected to occur; frequent attacks on this asset type |

### 5.2 Impact Scale

Impact is assessed across the CIA triad. Use the highest applicable impact.

| Score | Level | Confidentiality | Integrity | Availability |
|-------|-------|----------------|-----------|-------------|
| 1 | Negligible | Internal data only, no PII, no customer impact | Minor data error, self-correcting | < 1 hour downtime |
| 2 | Minor | Limited PII exposed, no regulatory breach | Data errors detected quickly | 1-4 hours downtime |
| 3 | Moderate | PII of multiple individuals, possible regulatory notification | Significant data corruption | 4-24 hours downtime |
| 4 | Significant | Bulk PII/sensitive data, regulatory breach | Critical business data compromised | 1-3 days downtime |
| 5 | Critical | Mass PII breach, severe regulatory/reputational damage | Complete data loss | > 3 days or permanent loss |

### 5.3 Risk Score Calculation

```
Risk Score = Likelihood × Impact
```

| | Impact 1 | Impact 2 | Impact 3 | Impact 4 | Impact 5 |
|--|----------|----------|----------|----------|----------|
| **Likelihood 5** | 5 | 10 | 15 | 20 | **25** |
| **Likelihood 4** | 4 | 8 | 12 | **16** | **20** |
| **Likelihood 3** | 3 | 6 | 9 | **12** | **15** |
| **Likelihood 2** | 2 | 4 | 6 | 8 | 10 |
| **Likelihood 1** | 1 | 2 | 3 | 4 | 5 |

### 5.4 Risk Rating

| Score | Rating | Colour |
|-------|--------|--------|
| 1-4 | Low | 🟢 |
| 5-9 | Medium | 🟡 |
| 10-15 | High | 🟠 |
| 16-25 | Critical | 🔴 |

---

## 6. Risk Evaluation

### 6.1 Risk Acceptance Criteria

| Rating | Acceptance | Action Required |
|--------|-----------|----------------|
| 🟢 Low (1-4) | Acceptable — accept with monitoring | Document and monitor |
| 🟡 Medium (5-9) | Conditionally acceptable | Must have treatment plan; review annually |
| 🟠 High (10-15) | Not acceptable without treatment | Treatment plan required; review quarterly |
| 🔴 Critical (16-25) | Never acceptable without immediate action | Immediate treatment; escalate to CEO |

### 6.2 Risk Owner Assignment

Every risk must have a named owner. The risk owner is responsible for:
- Approving the risk treatment decision
- Overseeing implementation of the treatment plan
- Accepting residual risk

---

## 7. Risk Treatment Options

| Option | Definition | When to Use |
|--------|-----------|------------|
| **Treat (Modify)** | Implement controls to reduce likelihood or impact | Most risks — controls from Annex A |
| **Tolerate (Accept)** | Accept the risk without further action | Low risks; cost of control exceeds benefit |
| **Transfer** | Transfer risk to third party (insurance, contract) | Specific liability risks; supply chain |
| **Terminate (Avoid)** | Eliminate the risk by stopping the activity | When risk cannot be reduced to acceptable level |

---

## 8. Residual Risk

After controls are applied, recalculate:
```
Residual Risk = Revised Likelihood × Revised Impact
```

Residual risk must be at an acceptable level before being signed off by the risk owner.

---

## 9. Review Frequency

| Trigger | Action |
|---------|--------|
| Annual | Full risk assessment review |
| Significant change (new product, new cloud account, new supplier) | Partial re-assessment of affected areas |
| Security incident | Risk re-assessment for affected asset |
| Post-audit finding | Risk re-assessment for gap areas |
| Management review | Status review (not full re-assessment) |

---

## 10. Documentation

The following are retained as documented information (Clause 7.5):

| Document | Location | Retention |
|----------|---------|-----------|
| Risk Assessment Methodology (this doc) | [ISMS doc store] | Permanent |
| Asset Register | [ISMS doc store] | Current + 3 years |
| Risk Register | [ISMS doc store] | Current + 3 years |
| Risk Treatment Plan | [ISMS doc store] | Current + 3 years |
| Statement of Applicability | [ISMS doc store] | Permanent |
