# Internal Audit Checklist — Annex A Controls
**ISO 27001:2022 — All 93 Controls | Version 1.0**
**Auditor:** [Name] | **Audit Date:** [Date] | **Audit Ref:** IA-[YYYY]-[NNN]

**Scoring:** C = Conformant | NC-Ma = Major NC | NC-Mi = Minor NC | O = Observation | N/A (excluded in SoA)

> Before auditing: cross-reference the SoA. Only audit controls marked "Included." For excluded controls, verify the exclusion justification is still valid.

---

## Theme A.5 — Organisational Controls (37 controls)

| Control | Title | Key Audit Questions | Evidence | Score | Notes |
|---------|-------|---------------------|---------|-------|-------|
| A.5.1 | Policies for information security | IS policy exists, approved, communicated, reviewed? | IS Policy v[X], signed by CEO, accessible to all | | |
| A.5.2 | Information security roles and responsibilities | ISMS roles defined and communicated? | RACI; job descriptions | | |
| A.5.3 | Segregation of duties | Are conflicting duties separated? (e.g., dev cannot approve own code to prod) | CODEOWNERS; deployment approvals; GitHub branch protection | | |
| A.5.4 | Management responsibilities | Do managers enforce IS policies within their teams? | Evidence of manager briefings; performance reviews including IS | | |
| A.5.5 | Contact with authorities | Are contacts with regulators/law enforcement established? | Contact list (ICO, NCSC, etc.) in IRP | | |
| A.5.6 | Contact with special interest groups | Membership in security information sharing groups? | NCSC early warning; vendor security bulletins; ISACs | | |
| A.5.7 | Threat intelligence | Is threat intelligence collected and acted upon? | Security newsletter; threat intel feed; GuardDuty findings review | | |
| A.5.8 | IS in project management | Is security considered in projects? | Project templates including security review step; change management | | |
| A.5.9 | Inventory of information and other associated assets | Asset register in place and maintained? | Asset register — last updated date, owner column | | |
| A.5.10 | Acceptable use of information and other assets | AUP exists, signed by all staff? | AUP document; signed acknowledgements | | |
| A.5.11 | Return of assets | Offboarding checklist requires device return? | Offboarding procedure; completed offboarding records | | |
| A.5.12 | Classification of information | Data classification scheme documented? | Data classification policy; handling rules | | |
| A.5.13 | Labelling of information | Are data assets labelled per classification? | S3 tags; Notion page labels; document headers | | |
| A.5.14 | Information transfer | Is data transfer controlled? (email, USB, cloud) | AUP; data transfer policy; DLP controls | | |
| A.5.15 | Access control | Access control policy in place? Least privilege applied? | Access control policy; IAM credential report; access reviews | | |
| A.5.16 | Identity management | Identity lifecycle managed (create, review, disable, delete)? | IdP (Entra/Okta) provisioning records; offboarding log | | |
| A.5.17 | Authentication information | Password/MFA policy enforced? | MFA enforcement checklist; IdP policy settings | | |
| A.5.18 | Access rights | Access provisioning and periodic review in place? | Access review records; joiner/mover/leaver log | | |
| A.5.19 | Information security in supplier relationships | Supplier security policy; supplier assessments? | Supplier policy; completed assessments; subprocessor register | | |
| A.5.20 | Addressing IS within supplier agreements | Security clauses in contracts? DPA in place? | Sample contract; DPA records | | |
| A.5.21 | Managing IS in the ICT supply chain | Software supply chain risks managed? | SCA scanning; SBOM; third-party component policy | | |
| A.5.22 | Monitoring, review and change management of supplier services | Suppliers reviewed periodically? | Supplier review records; annual assessment schedule | | |
| A.5.23 | IS for use of cloud services | Cloud security policy; shared responsibility documented? | Cloud security baselines; SoA cloud control entries | | |
| A.5.24 | IS incident management planning and preparation | IRP in place, tested? | IRP document; tabletop exercise records | | |
| A.5.25 | Assessment and decision on IS events | Incident classification scheme? | Incident classification matrix; incident logs | | |
| A.5.26 | Response to IS incidents | Incidents responded to per IRP? | Closed incident logs; response timelines | | |
| A.5.27 | Learning from IS incidents | Post-incident reviews conducted? | PIR records; IRP updates from lessons learned | | |
| A.5.28 | Collection of evidence | Evidence preservation procedure? | Evidence S3 bucket; chain of custody records | | |
| A.5.29 | IS during disruption | BCP in place covering IS during disruption? | BCP document; DR runbook | | |
| A.5.30 | ICT readiness for business continuity | RTO/RPO defined and tested? | RTO/RPO worksheet; DR test records | | |
| A.5.31 | Legal, statutory, regulatory and contractual requirements | Legal requirements identified and met? | Compliance register (GDPR, etc.); legal review records | | |
| A.5.32 | Intellectual property rights | Software licensing controls? | Software asset register; licence management | | |
| A.5.33 | Protection of records | Records retention and protection? | Retention schedule; backup records | | |
| A.5.34 | Privacy and protection of PII | GDPR/privacy controls? DPA registered? | GDPR compliance records; privacy notice; DPA | | |
| A.5.35 | Independent review of IS | Internal audit programme and external review? | Internal audit records; any external penetration test | | |
| A.5.36 | Compliance with policies, rules and standards | Compliance checks conducted? | Audit checklist; policy compliance records | | |
| A.5.37 | Documented operating procedures | Key IT procedures documented? | IT runbooks; standard operating procedures | | |

---

## Theme A.6 — People Controls (8 controls)

| Control | Title | Key Audit Questions | Evidence | Score | Notes |
|---------|-------|---------------------|---------|-------|-------|
| A.6.1 | Screening | Background checks conducted pre-employment? | HR records; background check policy; third-party screening provider | | |
| A.6.2 | Terms and conditions of employment | IS responsibilities in employment contracts? | Sample contract; AUP signed | | |
| A.6.3 | IS awareness, education and training | Annual training programme; records? | Training programme plan; completion tracker; phishing sim results | | |
| A.6.4 | Disciplinary process | Disciplinary policy covers IS violations? | Disciplinary policy; any precedent cases (anonymised) | | |
| A.6.5 | Responsibilities after termination or change of employment | Offboarding procedure; NDA? | Offboarding checklist; post-employment NDA clause in contract | | |
| A.6.6 | Confidentiality or non-disclosure agreements | NDAs in place for all staff and relevant third parties? | NDA template; signed NDA records | | |
| A.6.7 | Remote working | Remote working security policy/controls? | AUP remote section; VPN requirement; MDM on remote devices | | |
| A.6.8 | IS event reporting | All staff know how to report? Events actually reported? | Reporting procedure; incident log (P3/P4 events too) | | |

---

## Theme A.7 — Physical Controls (14 controls)

| Control | Title | Key Audit Questions | Evidence | Score | Notes |
|---------|-------|---------------------|---------|-------|-------|
| A.7.1 | Physical security perimeters | Office/server room access controlled? | Badge access records; CCTV; lock logs |  | |
| A.7.2 | Physical entry | Visitor log; escort procedure? | Visitor log; entry policy | | |
| A.7.3 | Securing offices, rooms and facilities | Sensitive areas restricted (server rooms, HR offices)? | Access control records | | |
| A.7.4 | Physical security monitoring | CCTV or monitoring in place? Reviewed? | CCTV records; monitoring policy | | |
| A.7.5 | Protecting against physical and environmental threats | Protection from fire, flood, power failure? | Fire suppression; UPS; physical security assessment | | |
| A.7.6 | Working in secure areas | Rules for secure area working (no visitors, no notes)? | Secure area policy | | |
| A.7.7 | Clear desk and clear screen | Clear desk policy? Enforced? | Policy; spot check evidence; screen lock enforcement via MDM | | |
| A.7.8 | Equipment siting and protection | Equipment positioned to reduce eavesdropping/damage risk? | Physical layout review | | |
| A.7.9 | Security of assets off-premises | Laptop policy for off-site use (encryption, VPN)? | AUP; MDM records showing FDE; VPN logs | | |
| A.7.10 | Storage media | USB policy; media disposal? | AUP; media disposal records; degaussing/shredding policy | | |
| A.7.11 | Supporting utilities | UPS; power conditioning; generator? | UPS records; test logs | | |
| A.7.12 | Cabling security | Network cable labelling; protection? | Physical inspection; cabling documentation | | |
| A.7.13 | Equipment maintenance | Hardware maintenance schedules? | Maintenance records; cloud hardware = AWS responsibility | | |
| A.7.14 | Secure disposal or re-use of equipment | Wiping procedure before disposal/reuse? | Disposal records; MDM remote wipe logs | | |

> **Note for cloud-native orgs:** A.7.11, A.7.12, A.7.13 may be largely addressed by AWS/Azure physical security (document in SoA exclusion or note cloud provider responsibility).

---

## Theme A.8 — Technological Controls (34 controls)

| Control | Title | Key Audit Questions | Evidence | Score | Notes |
|---------|-------|---------------------|---------|-------|-------|
| A.8.1 | User end point devices | MDM on all devices? FDE enforced? EDR deployed? | MDM compliance report; FDE policy; EDR dashboard | | |
| A.8.2 | Privileged access rights | PAM implemented? JIT access? Break-glass accounts? | PAM procedure; PIM activation logs; break-glass account CloudWatch alarm | | |
| A.8.3 | Information access restriction | Access controls enforced on systems? | IAM policies; RBAC; database access controls | | |
| A.8.4 | Access to source code | Who has prod code access? Is it reviewed? | GitHub org members; branch protection rules; CODEOWNERS | | |
| A.8.5 | Secure authentication | MFA on all systems? Strong auth standards? | MFA checklist; Conditional Access policies; IAM password policy | | |
| A.8.6 | Capacity management | Capacity monitored? Alerts set? | CloudWatch capacity alarms; auto-scaling configuration | | |
| A.8.7 | Protection against malware | EDR/AV on endpoints? Email scanning? | CrowdStrike/Defender dashboard; M365 Safe Attachments | | |
| A.8.8 | Management of technical vulnerabilities | Vulnerability scanning? Patch SLAs? | Inspector/Security Hub reports; patch management records; Trivy CI results | | |
| A.8.9 | Configuration management | Baseline configurations documented? IaC used? | Terraform repo; cloud security baselines; SSM compliance | | |
| A.8.10 | Information deletion | Data deleted per retention policy? Deletion verified? | S3 lifecycle policies; DB deletion records; test evidence | | |
| A.8.11 | Data masking | PII masked in non-production environments? | Staging environment data masking config; anonymisation scripts | | |
| A.8.12 | Data leakage prevention | DLP controls in place? (email, USB, cloud) | M365 DLP policies; AUP; cloud storage controls | | |
| A.8.13 | Information backup | Backups configured, tested? 3-2-1 rule? | AWS Backup reports; restore test records; backup policy | | |
| A.8.14 | Redundancy of information processing facilities | Redundancy for critical systems? Multi-AZ? | RDS Multi-AZ; ALB; auto-scaling; DR region | | |
| A.8.15 | Logging | Audit logs enabled for all critical systems? | CloudTrail; VPC Flow Logs; application log config | | |
| A.8.16 | Monitoring activities | Logs reviewed? Alerts configured? | CloudWatch alarms; GuardDuty findings; SIEM rules | | |
| A.8.17 | Clock synchronisation | System clocks synced (NTP)? | AWS NTP (default); verify with `timedatectl` on EC2 | | |
| A.8.18 | Use of privileged utility programs | Admin tools usage controlled and logged? | SSM Session Manager logs; admin access audit | | |
| A.8.19 | Installation of software on operational systems | Software installation controlled? Patch process? | SSM Patch Manager; MDM app allow-list; change management | | |
| A.8.20 | Networks security | Network segmentation? Firewall rules? WAF? | VPC security group review; WAF config; network architecture diagram | | |
| A.8.21 | Security of network services | Network services (VPN, DNS) secured? | VPN config; Route 53 DNSSEC; network security policy | | |
| A.8.22 | Segregation of networks | Production separated from dev/test? | VPC separation; account separation (AWS Organizations) | | |
| A.8.23 | Web filtering | Web content filtering for endpoints? | DNS filtering (Cloudflare Gateway); MDM web filtering | | |
| A.8.24 | Use of cryptography | Encryption at rest and in transit? Approved algorithms? | Cryptography policy; RDS encryption check; TLS config; KMS key usage | | |
| A.8.25 | Secure development life cycle | Secure SDLC? Security in CICD? | DevSecOps checklist; SAST/SCA/secret scan in pipeline | | |
| A.8.26 | Application security requirements | Security requirements defined for applications? | Threat model; security requirements in tickets/PRs | | |
| A.8.27 | Secure system architecture and engineering principles | Security-by-design? Architecture review? | Architecture documents; security review in design phase | | |
| A.8.28 | Secure coding | Secure coding standards? Code review? | Secure coding guide; CODEOWNERS; PR review requirement | | |
| A.8.29 | Security testing in development and acceptance | SAST/DAST/pen test? | CI pipeline results; pen test report; security gate in pipeline | | |
| A.8.30 | Outsourced development | Third-party code reviewed? Supplier code security? | Supplier security requirements; code review for vendor code | | |
| A.8.31 | Separation of development, test and production environments | Dev/test/prod separated? No prod data in test? | Account separation; data masking in test; SDLC policy | | |
| A.8.32 | Change management | Change process controls deployment? Security review? | Change management policy; change log; deployment approvals | | |
| A.8.33 | Test information | Test data management? No real PII in test? | Test data policy; data masking evidence | | |
| A.8.34 | Protection of information systems during audit testing | Audit testing does not risk production? | Audit procedure; read-only access for auditors | | |

---

## Annex A Audit Summary

| Theme | Controls In Scope | C | NC-Mi | NC-Ma | O | N/A |
|-------|-----------------|---|-------|-------|---|-----|
| A.5 Organisational | 37 | | | | | |
| A.6 People | 8 | | | | | |
| A.7 Physical | 14 | | | | | |
| A.8 Technological | 34 | | | | | |
| **Total** | **93** | | | | | |

**Auditor:** ___________________ **Date:** ___________________
