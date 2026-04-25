# Statement of Applicability (SoA)
**ISO 27001:2022 — Clause 6.1.3(d) | Version 1.0**
**Organization:** [Organization Name]
**ISMS Scope:** [Scope summary]
**Prepared by:** [ISMS Owner]
**Approved by:** [CEO/CTO] | [Date]

---

## How to Read This Document

- **Included:** Control is applicable and implemented (or being implemented)
- **Excluded:** Control is not applicable — justification must be documented
- **Status:** Implemented / Partially implemented / Planned (with target date) / Not started
- **Evidence Ref:** Where to find evidence of implementation

Every inclusion must be justified by a risk treatment decision. Every exclusion must be justified — "not applicable" alone is not acceptable to an auditor.

---

## Theme A.5 — Organizational Controls

| Control | Title | Inc/Exc | Justification | Status | Evidence Ref |
|---------|-------|---------|--------------|--------|-------------|
| A.5.1 | Policies for information security | Inc | Risk R-013; mandatory for all ISMSs | Implemented | IS Policy v1.0 |
| A.5.2 | IS roles and responsibilities | Inc | Clause 5.3 requirement; stakeholder map | Implemented | Stakeholder Map v1.0 |
| A.5.3 | Segregation of duties | Inc | Risk R-001; prevents single-person fraud/error | Partially | IAM roles review pending |
| A.5.4 | Management responsibilities | Inc | Clause 5.1 requirement | Implemented | Policy v1.0; management review minutes |
| A.5.5 | Contact with authorities | Inc | Required for incident response; regulatory obligations | Planned | IRP contact list |
| A.5.6 | Contact with special interest groups | Inc | Threat intelligence; sector-specific groups | Planned | [target: Q+1] |
| A.5.7 | Threat intelligence | Inc | Required for vulnerability management programme | Planned | [target: Q+1] |
| A.5.8 | IS in project management | Inc | New features and infrastructure changes must include security | Partially | Change mgmt policy |
| A.5.9 | Inventory of information and other assets | Inc | Risk R-015; mandatory for risk assessment | Implemented | Asset Register v1.0 |
| A.5.10 | Acceptable use of information and assets | Inc | Risk R-011; all employees must know acceptable use | Implemented | AUP v1.0 |
| A.5.11 | Return of assets | Inc | Risk R-009; assets must be returned on offboarding | Partially | Offboarding checklist |
| A.5.12 | Classification of information | Inc | Risk R-011; data handling depends on classification | Implemented | Data Classification Policy v1.0 |
| A.5.13 | Labelling of information | Inc | Risk R-011; documents must be labelled per classification | Partially | Labels applied to key docs |
| A.5.14 | Information transfer | Inc | Risk R-011; transfer controls needed for PII | Partially | AUP + DLP policy |
| A.5.15 | Access control | Inc | Risk R-001, R-006; foundational control | Implemented | Access Control Policy v1.0 |
| A.5.16 | Identity management | Inc | Risk R-001, R-009; identity lifecycle | Partially | IdP (Entra ID / Okta) in use |
| A.5.17 | Authentication information | Inc | Risk R-001, R-003; password and key management | Implemented | Cryptography Policy; password manager enforced |
| A.5.18 | Access rights | Inc | Risk R-001, R-009; access must be least-privilege | Partially | Access review planned quarterly |
| A.5.19 | IS in supplier relationships | Inc | Risk R-015; all critical suppliers assessed | Partially | Supplier register; questionnaires Q+2 |
| A.5.20 | IS within supplier agreements | Inc | Risk R-015; contracts must include security clauses | Partially | Contract clauses template created |
| A.5.21 | IS in ICT supply chain | Inc | Risk R-004; software supply chain risk | Planned | Dependabot + SCA in roadmap |
| A.5.22 | Monitoring, review and change management of supplier services | Inc | Risk R-015; suppliers reviewed annually | Planned | Supplier review calendar |
| A.5.23 | IS for use of cloud services | Inc | Risk R-005, R-006; AWS/Azure are primary infrastructure | Implemented | Cloud security baselines |
| A.5.24 | IS incident management planning | Inc | Risk R-013; IRP required | Implemented | Incident Response Plan v1.0 |
| A.5.25 | Assessment of IS events | Inc | IS events must be classified and assessed | Implemented | Incident Classification Matrix |
| A.5.26 | Response to IS incidents | Inc | Incidents must be responded to per IRP | Implemented | IRP v1.0 |
| A.5.27 | Learning from IS incidents | Inc | Post-incident review required; continuous improvement | Partially | Incident log template; post-mortems planned |
| A.5.28 | Collection of evidence | Inc | Forensic evidence preservation for incidents | Planned | Evidence collection procedure |
| A.5.29 | IS during disruption | Inc | Risk R-007, R-014; BCP covers IS during disruption | Implemented | BCP v1.0 |
| A.5.30 | ICT readiness for business continuity | Inc | Risk R-007; cloud recovery procedures documented | Implemented | DR Runbook v1.0 |
| A.5.31 | Legal, statutory, regulatory requirements | Inc | GDPR obligations; sector-specific regulations | Partially | Legal register maintained by legal counsel |
| A.5.32 | Intellectual property rights | Inc | Source code, third-party licenses | Partially | License scanning (Fossa/TLDR Legal) |
| A.5.33 | Protection of records | Inc | Clause 7.5 retention requirements | Implemented | Document control procedure v1.0 |
| A.5.34 | Privacy and protection of PII | Inc | GDPR; customer PII in scope | Implemented | Data classification + DPA with suppliers |
| A.5.35 | Independent review of IS | Inc | Clause 9.2; internal audit required | Implemented | Internal audit programme v1.0 |
| A.5.36 | Compliance with IS policies | Inc | Clause 9.1; monitoring of policy compliance | Partially | Quarterly access review; MDM compliance reports |
| A.5.37 | Documented operating procedures | Inc | Critical operational procedures must be documented | Partially | Runbooks in progress |

---

## Theme A.6 — People Controls

| Control | Title | Inc/Exc | Justification | Status | Evidence Ref |
|---------|-------|---------|--------------|--------|-------------|
| A.6.1 | Screening | Inc | Background checks for sensitive roles | Partially | HR policy; checks for engineering roles |
| A.6.2 | Terms and conditions of employment | Inc | All employees must have IS obligations in contract | Implemented | Employment contracts include AUP reference |
| A.6.3 | IS awareness, education and training | Inc | Risk R-013; training reduces human error and phishing | Partially | Training programme v1.0; records being established |
| A.6.4 | Disciplinary process | Inc | Policy violations must have consequences | Implemented | HR disciplinary policy (existing) |
| A.6.5 | Responsibilities after termination | Inc | Risk R-009; NDA obligations survive employment | Implemented | Offboarding checklist; NDA in contracts |
| A.6.6 | Confidentiality / NDA agreements | Inc | All employees and contractors sign NDA | Implemented | NDA template; contractor agreements |
| A.6.7 | Remote working | Inc | All employees remote/hybrid; endpoint + home security required | Implemented | AUP; MDM enforced; VPN available |
| A.6.8 | IS event reporting | Inc | Staff must have a clear channel to report incidents | Implemented | IRP specifies reporting channel (security@company.com) |

---

## Theme A.7 — Physical Controls

| Control | Title | Inc/Exc | Justification | Status | Evidence Ref |
|---------|-------|---------|--------------|--------|-------------|
| A.7.1 | Physical security perimeters | Inc (partial) | Office perimeter; cloud DCs covered by provider | Implemented | Cloud: AWS/Azure ISO certs; Office: keycard entry |
| A.7.2 | Physical entry | Inc | Visitor management at office required | Partially | Visitor log to be implemented |
| A.7.3 | Securing offices, rooms, facilities | Inc | Office and server rooms must be secured | Implemented | Keycard access; server room locked |
| A.7.4 | Physical security monitoring | Inc | Office CCTV | Implemented | CCTV in use at HQ |
| A.7.5 | Protecting against physical/environmental threats | Inc | Office fire safety; cloud DC environmental | Implemented | Cloud: provider responsibility; Office: fire safety |
| A.7.6 | Working in secure areas | Inc | Secure areas must have access restrictions | Implemented | Server room access log |
| A.7.7 | Clear desk and clear screen | Inc | Sensitive information not left unattended | Partially | Screen lock enforced via MDM; clear desk policy |
| A.7.8 | Equipment siting and protection | Inc | Equipment must be protected from environmental hazards | Implemented | Office: standard office safety |
| A.7.9 | Security of assets off-premises | Inc | Risk R-008; laptops taken off-site | Implemented | FDE enforced; MDM tracking |
| A.7.10 | Storage media | Inc | Secure handling and disposal of storage media | Partially | Disposal procedure to be documented |
| A.7.11 | Supporting utilities | Exc | No owned power/cooling infrastructure; cloud DC responsibility | N/A | AWS/Azure cover this; documented in SoA |
| A.7.12 | Cabling security | Exc | No owned data centre cabling | N/A | Cloud provider responsibility; no server rooms with production systems |
| A.7.13 | Equipment maintenance | Inc | Laptop maintenance; network equipment | Partially | MDM handles software; hardware maintenance ad-hoc |
| A.7.14 | Secure disposal or re-use of equipment | Inc | Risk R-008; laptop disposal must be secure | Planned | Disposal procedure to be written |

---

## Theme A.8 — Technological Controls

| Control | Title | Inc/Exc | Justification | Status | Evidence Ref |
|---------|-------|---------|--------------|--------|-------------|
| A.8.1 | User endpoint devices | Inc | All laptops must have MDM, FDE, EDR | Implemented | MDM (Intune/Jamf) enrolled; compliance report |
| A.8.2 | Privileged access rights | Inc | Risk R-001, R-005; admin access must be controlled | Partially | IAM roles review; PAM procedure drafted |
| A.8.3 | Information access restriction | Inc | Risk R-001, R-006; access based on need | Implemented | IAM policies; role-based access |
| A.8.4 | Access to source code | Inc | Risk R-003; source code is confidential | Implemented | GitHub private org; branch protection |
| A.8.5 | Secure authentication | Inc | Risk R-001, R-005; MFA enforced | Partially | MFA on cloud and GitHub; SSO rollout in progress |
| A.8.6 | Capacity management | Inc | Production system capacity must be monitored | Implemented | CloudWatch alarms; auto-scaling configured |
| A.8.7 | Protection against malware | Inc | EDR on all endpoints; malware protection | Implemented | CrowdStrike/Defender on all laptops |
| A.8.8 | Management of technical vulnerabilities | Inc | Risk R-010; vulnerability scanning programme | Partially | Dependabot; Trivy in CI; formal programme planned |
| A.8.9 | Configuration management | Inc | Infrastructure must be defined as code | Partially | Terraform used; drift detection planned |
| A.8.10 | Information deletion | Inc | Data retention and deletion policy required | Planned | Data retention procedure to be written |
| A.8.11 | Data masking | Inc | PII must be masked in non-prod environments | Planned | Risk R-015c; masking procedure to be implemented |
| A.8.12 | Data leakage prevention | Inc | Risk R-011; DLP controls for PII | Partially | Google Workspace DLP rules configured |
| A.8.13 | Information backup | Inc | Risk R-007, R-014; backup and restore | Implemented | AWS Backup; cross-account; restore tests |
| A.8.14 | Redundancy of IS processing facilities | Inc | Production availability requirement | Implemented | Multi-AZ RDS; ECS multi-AZ |
| A.8.15 | Logging | Inc | Risk R-010; security event logging | Partially | CloudTrail enabled; central log store planned |
| A.8.16 | Monitoring activities | Inc | Security events must be monitored | Partially | GuardDuty enabled; SIEM planned |
| A.8.17 | Clock synchronisation | Inc | All systems must use NTP synchronization | Implemented | AWS/Azure enforce NTP automatically |
| A.8.18 | Use of privileged utility programs | Inc | Admin tools must be controlled | Partially | Documented in PAM procedure |
| A.8.19 | Installation of software on operational systems | Inc | Only approved software on production | Partially | MDM software management; production IaC only |
| A.8.20 | Network security | Inc | Risk R-012; network traffic must be controlled | Implemented | Security groups; NACLs; WAF |
| A.8.21 | Security of network services | Inc | Network services must be secured | Implemented | TLS everywhere; private subnets |
| A.8.22 | Segregation of networks | Inc | Production/dev/corp networks must be separated | Partially | Separate VPCs; dev/prod separation |
| A.8.23 | Web filtering | Inc | Web browsing must be filtered for threats | Planned | DNS filtering (Cloudflare/Umbrella) to be deployed |
| A.8.24 | Use of cryptography | Inc | Data at rest and in transit must be encrypted | Implemented | TLS 1.2+; AES-256 at rest; KMS key management |
| A.8.25 | Secure development lifecycle | Inc | Risk R-002; security in SDLC | Partially | Code review; SAST planned |
| A.8.26 | Application security requirements | Inc | Security requirements in user stories | Planned | OWASP ASVS to be adopted |
| A.8.27 | Secure system architecture | Inc | Security by design in architecture decisions | Partially | Architecture reviews include security |
| A.8.28 | Secure coding | Inc | Risk R-002; secure coding standards required | Partially | Code review; OWASP Top 10 training planned |
| A.8.29 | Security testing in dev and acceptance | Inc | Risk R-002; security testing in pipeline | Partially | Manual pen test; DAST planned |
| A.8.30 | Outsourced development | Inc | Contractors must follow same secure coding standards | Partially | Contractor security agreement |
| A.8.31 | Separation of dev, test, production | Inc | No production data in dev; separate accounts | Partially | Separate AWS accounts; data masking needed |
| A.8.32 | Change management | Inc | All changes to production must be controlled | Implemented | Change management policy; PR review required |
| A.8.33 | Test information | Inc | Production data not used in testing | Partially | Policy exists; technical controls planned |
| A.8.34 | Protection of IS during audit testing | Inc | Audit tools must not disrupt production | Planned | Audit testing procedure |

---

## SoA Summary

| Theme | Total | Included | Excluded |
|-------|-------|---------|---------|
| A.5 Organizational | 37 | 37 | 0 |
| A.6 People | 8 | 8 | 0 |
| A.7 Physical | 14 | 12 | 2 |
| A.8 Technological | 34 | 34 | 0 |
| **Total** | **93** | **91** | **2** |

**Excluded controls (A.7.11, A.7.12):** Excluded because [Organization] operates exclusively in cloud environments (AWS, Azure) with no owned data centre infrastructure. Physical utility and cabling controls are the responsibility of the cloud providers, who maintain their own ISO 27001 certification covering these controls. This position is supported by the AWS/Azure shared responsibility model.

---

## Approval

| Role | Name | Signature | Date |
|------|------|-----------|------|
| ISMS Owner | | | |
| Top Management | | | |
