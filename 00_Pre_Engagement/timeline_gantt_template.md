# ISO 27001 Project Timeline
**Adapt to client deadline. Standard engagement: 9-12 months for first certification.**

---

## Fast-Track Timeline (6-9 months — startup with decent security hygiene)

```
MONTH 1      MONTH 2      MONTH 3      MONTH 4      MONTH 5      MONTH 6
Jan          Feb          Mar          Apr          May          Jun

SCOPING & GAP ANALYSIS
│────────────┤
│ Scope def  │
│ Kickoff Q  │
│ Gap analysis────────┤
             │ Gap report │

RISK ASSESSMENT
             │────────────────────────┤
             │ Asset register         │
             │ Risk register          │
             │ SoA + RTP              │

DOCUMENTATION
                          │────────────────────────┤
                          │ ISMS Manual + Policies  │
                          │ Document control        │

INFRASTRUCTURE REMEDIATION
                          │─────────────────────────────────┤
                          │ Cloud hardening                  │
                          │ IAM + MFA                        │
                          │ Logging & monitoring             │
                          │ Vulnerability mgmt               │

SUPPLIERS
                                       │────────────┤
                                       │ Vendor Q   │
                                       │ Register   │

TRAINING
                                                    │────────┤
                                                    │ Prog   │
                                                    │ Records│

MONTH 7      MONTH 8      MONTH 9
Jul          Aug          Sep

INTERNAL AUDIT
│────────────┤
│ Audit plan │
│ Conduct    │
│ NC report  │

CORRECTIVE ACTIONS
│─────────────────────┤
│ Close NCs           │

MANAGEMENT REVIEW
             │────────┤
             │ Review │
             │ Minutes│

CERTIFICATION
                          │────────────┤
                          │ Stage 1    │
                          │ (doc check)│
                          │            │
                          │ → Findings │
                          │ → Close    │
                          │            │
                          │ Stage 2    │
                          │ (evidence) │
                          │ → CERT     │
```

---

## Standard Timeline (10-12 months — typical startup with gaps)

| Month | Phase | Key Activities | Deliverables |
|-------|-------|----------------|-------------|
| 1 | Scoping | Discovery questionnaire, site visits, stakeholder mapping | Scope doc, project charter |
| 2 | Gap Analysis | Clause + Annex A gap assessment, interviews | Gap analysis report (RAG) |
| 3 | Risk Methodology | Define methodology, asset categorization | Risk methodology doc |
| 4 | Risk Assessment | Asset register, threat modelling, risk scoring | Risk register draft |
| 5 | Risk Treatment | SoA, risk treatment decisions, RTP | SoA, Risk Treatment Plan |
| 6 | Documentation | ISMS manual, core policies (IS policy, AUP, etc.) | 5-6 policies approved |
| 7 | Documentation | Remaining policies + procedures | Full policy suite |
| 8 | Infrastructure | Cloud baseline, IAM hardening, logging, patching | Infra compliance report |
| 9 | People Controls | Supplier assessments, training programme, onboarding/offboarding | Training records |
| 10 | Internal Audit | Plan + conduct internal audit | NC report, CAR tracker |
| 11 | Remediation | Close NCs, management review | CAR closure, MR minutes |
| 12 | Cert Prep | Stage 1 readiness, book CB, Stage 1 | Stage 1 result |
| 13+ | Certification | Close Stage 1 findings, Stage 2 | Certificate |

---

## Milestone Gates

These are the hard dependencies — don't move to the next phase without them:

```
Milestone 1: Scope agreed + signed by top management
      ↓
Milestone 2: Gap analysis complete → priorities set → budget confirmed
      ↓
Milestone 3: Risk register approved + SoA signed
      ↓
Milestone 4: All policies approved (not just drafted)
      ↓
Milestone 5: Critical infrastructure gaps closed (audit logging, MFA, encryption)
      ↓
Milestone 6: First training cycle complete with records
      ↓
Milestone 7: Internal audit complete, NCs raised
      ↓
Milestone 8: All major NCs closed (corrective action evidence available)
      ↓
Milestone 9: Management review complete with minutes
      ↓
Milestone 10: Stage 1 audit → minor findings only
      ↓
Milestone 11: Stage 2 audit → CERTIFICATION
```

---

## Buffer Planning

| Risk | Add Buffer |
|------|-----------|
| Client has major infra debt | +2 months |
| No existing documentation at all | +1 month |
| < 0.25 FTE client availability | +2 months |
| Platform migration planned mid-project | Pause; restart post-migration |
| First pentest needed | +1 month (can run in parallel) |
| Certification body slot unavailable | +1-3 months (book early!) |

---

## When to Book the Certification Body

**Book when:** You have completed the risk assessment and have a clear remediation plan (Month 5-6 typically).

**Why early:** Popular CBs (BSI, Bureau Veritas, LRQA, DNV) have 3-6 month lead times for Stage 1 slots. Missing a slot can delay certification by a quarter.

**What to provide to the CB:**
- ISMS scope statement
- Number of employees in scope
- Number of sites
- Cloud/physical infrastructure overview
- Target Stage 1 date range
