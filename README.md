# ISObuddy — ISO 27001:2022 Lead Implementor Toolkit

# claude --resume "iso27001-toolkit-build"

Practical field toolkit for ISO 27001:2022 consulting engagements. Every template contains real content. Every checklist references the actual clause or Annex A control it covers. Built for startups: cloud-native, fast-moving, resource-constrained.

---

## Engagement Lifecycle

```
CLIENT SAYS "WE NEED ISO 27001"
            │
            ▼
  ┌─────────────────────┐
  │  00_Pre_Engagement  │  Scope it. Charter it. Meet the people.
  └─────────┬───────────┘
            │
            ▼
  ┌─────────────────────┐
  │  01_Gap_Analysis    │  Where are they NOW vs. ISO requirements?
  └─────────┬───────────┘
            │
            ▼
  ┌─────────────────────┐
  │  02_Risk_Assessment │  Asset register → threats → risks → SoA
  └─────────┬───────────┘
            │
     ┌──────┴──────┐
     ▼             ▼
  ┌──────────┐  ┌──────────────────────────┐
  │  03_ISMS │  │  04_Infrastructure       │
  │  Docs    │  │  Compliance              │
  │ Policies │  │  Cloud + Infra controls  │
  └──────┬───┘  └──────────┬───────────────┘
         └────────┬─────────┘
                  │
         ┌────────┴────────┐
         ▼                 ▼
  ┌─────────────┐   ┌──────────────────┐
  │  05_Access  │   │  06_Supplier     │
  │  Control    │   │  Third-Party     │
  └──────┬──────┘   └────────┬─────────┘
         └────────┬───────────┘
                  │
                  ▼
  ┌───────────────────────┐
  │  07_Incident_BCP      │  Response + continuity
  └───────────┬───────────┘
              │
              ▼
  ┌───────────────────────┐
  │  08_Awareness_Training│  People layer
  └───────────┬───────────┘
              │
              ▼
  ┌───────────────────────┐
  │  09_Internal_Audit    │  Pre-certification check
  └───────────┬───────────┘
              │
              ▼
  ┌───────────────────────┐
  │  10_Management_Review │  Leadership sign-off (mandatory)
  └───────────┬───────────┘
              │
              ▼
  ┌───────────────────────┐
  │  11_Certification_Prep│  Stage 1 + Stage 2 readiness
  └───────────┬───────────┘
              │
              ▼
  ┌───────────────────────┐
  │  12_Post_Certification│  Surveillance + continuous improvement
  └───────────────────────┘
```

---

## Quick Reference: Which Folder For What

| Situation                        | Go to                          |
| -------------------------------- | ------------------------------ |
| First client call / scoping      | `00_Pre_Engagement`            |
| "Where do we stand?" assessment  | `01_Gap_Analysis`              |
| Building the risk register       | `02_Risk_Assessment`           |
| Writing/reviewing policies       | `03_ISMS_Documentation`        |
| Hardening AWS, Azure, Linux, K8s | `04_Infrastructure_Compliance` |
| IAM audit, access reviews, MFA   | `05_Access_Control_Identity`   |
| Vendor questionnaires, SaaS risk | `06_Supplier_ThirdParty`       |
| Incident response, DR, tabletop  | `07_Incident_BCP`              |
| Training records, awareness      | `08_Awareness_Training`        |
| Running the internal audit       | `09_Internal_Audit`            |
| Prepping the management review   | `10_Management_Review`         |
| Certification audit readiness    | `11_Certification_Prep`        |
| Post-cert, surveillance audits   | `12_Post_Certification`        |

---

## ISO 27001:2022 Clause → Folder Map

| Clause | Title                               | Primary Folder                                   |
| ------ | ----------------------------------- | ------------------------------------------------ |
| 4      | Context of the Organization         | `00_Pre_Engagement`, `03_ISMS_Documentation`     |
| 5      | Leadership                          | `03_ISMS_Documentation`, `10_Management_Review`  |
| 6      | Planning (risks, objectives)        | `02_Risk_Assessment`                             |
| 7      | Support (resources, training, docs) | `08_Awareness_Training`, `03_ISMS_Documentation` |
| 8      | Operation (implement controls)      | `03–06_*`                                        |
| 9      | Performance Evaluation              | `09_Internal_Audit`, `10_Management_Review`      |
| 10     | Improvement                         | `09_Internal_Audit`, `12_Post_Certification`     |

---

## Annex A Structure (ISO 27001:2022)

| Theme              | Controls | Range      |
| ------------------ | -------- | ---------- |
| A.5 Organizational | 37       | 5.1 – 5.37 |
| A.6 People         | 8        | 6.1 – 6.8  |
| A.7 Physical       | 14       | 7.1 – 7.14 |
| A.8 Technological  | 34       | 8.1 – 8.34 |
| **Total**          | **93**   |            |

> **Note:** ISO 27001:2022 retired the old 14-domain structure from 2013 (114 controls). If a client shows you a 2013 SoA, it needs updating before certification.

---

## Mandatory Documented Information (Clause 7.5)

These documents are non-negotiable — auditors will ask for all of them:

1. ISMS Scope (Clause 4.3)
2. Information Security Policy (Clause 5.2)
3. Risk Assessment Methodology (Clause 6.1.2)
4. Risk Register (Clause 6.1.2)
5. Risk Treatment Plan (Clause 6.1.3)
6. Statement of Applicability (Clause 6.1.3d)
7. Information Security Objectives (Clause 6.2)
8. Evidence of Competence (Clause 7.2)
9. Internal Audit Programme + Results (Clause 9.2)
10. Management Review Results (Clause 9.3)
11. Nonconformity + Corrective Action records (Clause 10.1)

---

## Toolkit Contents

```
ISObuddy/
├── README.md
├── 00_Pre_Engagement/
│   ├── scope_definition_worksheet.md
│   ├── client_kickoff_questionnaire.md
│   ├── project_charter_template.md
│   ├── stakeholder_map.md
│   ├── timeline_gantt_template.md
│   └── IRL_TIPS.md
├── 01_Gap_Analysis/
│   ├── gap_analysis_methodology.md
│   ├── clause_gap_checklist.md
│   ├── annex_a_gap_checklist.md
│   ├── gap_analysis_report_template.md
│   └── IRL_TIPS.md
├── 02_Risk_Assessment/
│   ├── risk_methodology.md
│   ├── asset_register_template.md
│   ├── threat_vulnerability_library.md
│   ├── risk_register_template.md
│   ├── risk_treatment_plan_template.md
│   ├── statement_of_applicability_template.md
│   └── IRL_TIPS.md
├── 03_ISMS_Documentation/
│   ├── isms_scope_and_manual.md
│   ├── information_security_policy.md
│   ├── acceptable_use_policy.md
│   ├── access_control_policy.md
│   ├── cryptography_policy.md
│   ├── data_classification_policy.md
│   ├── incident_response_policy.md
│   ├── supplier_security_policy.md
│   ├── change_management_policy.md
│   ├── document_control_procedure.md
│   └── IRL_TIPS.md
├── 04_Infrastructure_Compliance/
│   ├── cloud_security_baseline_aws.md
│   ├── cloud_security_baseline_azure.md
│   ├── linux_hardening_checklist.md
│   ├── container_security_checklist.md
│   ├── network_architecture_review.md
│   ├── secrets_management_guide.md
│   ├── logging_monitoring_setup.md
│   ├── vulnerability_management_procedure.md
│   ├── patch_management_procedure.md
│   ├── backup_recovery_procedure.md
│   ├── devsecops_integration_checklist.md
│   ├── annex_a_infra_control_mapping.md
│   └── IRL_TIPS.md
├── 05_Access_Control_Identity/
│   ├── iam_policy_template.md
│   ├── privileged_access_management.md
│   ├── onboarding_offboarding_procedure.md
│   ├── mfa_enforcement_checklist.md
│   ├── access_review_template.md
│   └── IRL_TIPS.md
├── 06_Supplier_ThirdParty/
│   ├── vendor_risk_questionnaire.md
│   ├── supplier_assessment_template.md
│   ├── contract_security_clauses.md
│   ├── subprocessor_register.md
│   └── IRL_TIPS.md
├── 07_Incident_BCP/
│   ├── incident_response_plan.md
│   ├── incident_classification_matrix.md
│   ├── incident_log_template.md
│   ├── bcp_template.md
│   ├── disaster_recovery_runbook.md
│   ├── rto_rpo_worksheet.md
│   ├── tabletop_exercise_script.md
│   └── IRL_TIPS.md
├── 08_Awareness_Training/
│   ├── security_awareness_program_plan.md
│   ├── training_completion_tracker.md
│   ├── new_employee_security_onboarding.md
│   ├── phishing_simulation_plan.md
│   └── IRL_TIPS.md
├── 09_Internal_Audit/
│   ├── internal_audit_plan_template.md
│   ├── audit_checklist_clauses.md
│   ├── audit_checklist_annex_a.md
│   ├── nonconformity_report_template.md
│   ├── corrective_action_tracker.md
│   ├── audit_report_template.md
│   └── IRL_TIPS.md
├── 10_Management_Review/
│   ├── management_review_procedure.md
│   ├── review_agenda_template.md
│   ├── review_inputs_checklist.md
│   ├── review_minutes_template.md
│   ├── isms_kpi_dashboard.md
│   └── IRL_TIPS.md
├── 11_Certification_Prep/
│   ├── stage1_readiness_checklist.md
│   ├── stage2_readiness_checklist.md
│   ├── evidence_collection_guide.md
│   ├── document_control_review.md
│   ├── common_auditor_qa.md
│   └── IRL_TIPS.md
└── 12_Post_Certification/
    ├── surveillance_audit_prep.md
    ├── continuous_improvement_plan.md
    ├── metrics_tracking_dashboard.md
    ├── annual_review_calendar.md
    └── IRL_TIPS.md
```
