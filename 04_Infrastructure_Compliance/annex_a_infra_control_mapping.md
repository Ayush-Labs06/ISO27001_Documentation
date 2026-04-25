# Annex A Controls → Infrastructure Implementation Mapping
**ISO 27001:2022 | Cross-reference: Every Annex A control → How it's implemented in AWS/Azure infra**

This is the master map. For any Annex A control, find the tool, config, or evidence that satisfies it in your cloud environment.

---

## A.5 — Organizational Controls (Infra-relevant subset)

| Control | Title | Infra Implementation | Tool / Evidence |
|---------|-------|---------------------|----------------|
| A.5.7 | Threat intelligence | Consume NVD, CISA KEV, GitHub Advisory; GuardDuty findings | GuardDuty, Security Hub, Dependabot |
| A.5.9 | Asset inventory | Cloud asset inventory | AWS Config, Terraform state, asset register |
| A.5.12 | Data classification | S3 bucket tags (`DataClassification`), RDS tags | Terraform tags, AWS Config rules |
| A.5.13 | Information labelling | S3 object metadata, document headers | S3 tags, tagging policy |
| A.5.15 | Access control | IAM policies, SCPs, VPC security groups | AWS IAM, Entra ID Conditional Access |
| A.5.16 | Identity management | IdP (Entra ID/Okta), SSO, SCIM provisioning | Azure Entra ID, Okta |
| A.5.17 | Authentication information | Secrets Manager, KMS, password manager | AWS Secrets Manager, Vault |
| A.5.18 | Access rights | Least-privilege IAM roles, access reviews | IAM, Entra ID PIM, access review logs |
| A.5.19 | Supplier IS | Cloud provider ISO certs; SaaS vendor assessments | AWS/Azure certs; vendor questionnaires |
| A.5.20 | Supplier agreements | DPA, security clauses in contracts | Contract register |
| A.5.21 | ICT supply chain | SCA scanning, image pinning, SBOM | Dependabot, Snyk, Trivy, Syft |
| A.5.22 | Supplier monitoring | Annual supplier review; provider status pages | Supplier register, review calendar |
| A.5.23 | Cloud service security | Cloud security baselines; Security Hub | AWS Security Hub, Defender for Cloud |
| A.5.24 | Incident management | Incident Response Plan; CloudWatch alarms | IRP, PagerDuty, CloudWatch |
| A.5.25 | IS event assessment | GuardDuty findings triage; SIEM | GuardDuty, Security Hub |
| A.5.26 | Incident response | IRP executed; CloudTrail for evidence | IRP, CloudTrail, SSM for containment |
| A.5.27 | Learning from incidents | Post-incident reviews; risk register updates | Incident log, risk register |
| A.5.28 | Evidence collection | CloudTrail log preservation; S3 Object Lock | CloudTrail, S3 Object Lock |
| A.5.29 | IS during disruption | BCP; multi-AZ; cross-region failover | AWS multi-AZ, Route 53 failover |
| A.5.30 | ICT readiness for BCP | DR runbook; RTO/RPO tested | AWS Backup, tested restore procedures |
| A.5.33 | Protection of records | S3 Object Lock; immutable CloudTrail logs | CloudTrail, S3 WORM |
| A.5.36 | Policy compliance | MDM compliance, Security Hub score | Intune/Jamf, AWS Security Hub |
| A.5.37 | Documented procedures | Runbooks, IaC, operational procedures | Terraform, runbook docs |

---

## A.6 — People Controls (Infra-relevant)

| Control | Title | Infra Implementation | Tool / Evidence |
|---------|-------|---------------------|----------------|
| A.6.1 | Screening | Background checks for prod access roles | HR records + IdP access grant record |
| A.6.3 | Security training | Developer secure coding training; security awareness | Training records + KnowBe4/Proofpoint |
| A.6.7 | Remote working | VPN, MDM enforced, endpoint compliance | Conditional Access, Intune, VPN |
| A.6.8 | IS event reporting | `security@company.com`; Slack `#security-incidents` | Documented in IRP |

---

## A.7 — Physical Controls (Cloud-relevant notes)

| Control | Title | Infra Implementation | Tool / Evidence |
|---------|-------|---------------------|----------------|
| A.7.1 | Physical perimeters | Cloud DC: AWS/Azure ISO 27001 (their responsibility) | AWS/Azure compliance docs |
| A.7.5 | Environmental threats | Cloud provider handles power, cooling, fire | AWS/Azure shared responsibility |
| A.7.8 | Equipment siting | Cloud DC managed by provider; office: standard | Provider responsibility + office survey |
| A.7.9 | Assets off-premises | Laptop FDE, MDM, remote wipe | Intune/Jamf FDE enforcement |
| A.7.11 | Supporting utilities | Excluded — cloud provider | AWS ISO 27001 certificate |
| A.7.12 | Cabling security | Excluded — cloud provider | AWS ISO 27001 certificate |

---

## A.8 — Technological Controls — Complete Mapping

| Control | Title | Implementation | Tool | AWS Command / Evidence |
|---------|-------|---------------|------|----------------------|
| A.8.1 | User endpoint devices | MDM enrolled; FDE; EDR | Intune/Jamf + Defender/CrowdStrike | MDM compliance report |
| A.8.2 | Privileged access rights | IAM roles; PIM; no standing admin; JIT | AWS IAM + Entra PIM | IAM credential report; PIM activation logs |
| A.8.3 | Information access restriction | S3 bucket policies; IAM policies; private by default | AWS IAM, S3 Block Public Access | `aws s3api get-public-access-block` |
| A.8.4 | Access to source code | Private GitHub org; branch protection; CODEOWNERS | GitHub | Org settings; branch protection rules |
| A.8.5 | Secure authentication | MFA everywhere; FIDO2/TOTP; SSO; OIDC for CI/CD | Entra ID, Okta, AWS IAM | MFA status report; CA policies |
| A.8.6 | Capacity management | CloudWatch metrics; Auto Scaling; alarms | CloudWatch, Auto Scaling | Alarm history; capacity reports |
| A.8.7 | Protection against malware | EDR on endpoints; GuardDuty malware protection; ECR scanning | CrowdStrike/Defender + GuardDuty | EDR console; GuardDuty findings |
| A.8.8 | Vulnerability management | Trivy CI scan; Inspector; Dependabot; SSM Patch Manager | Amazon Inspector, Trivy, Dependabot | Inspector findings; patch compliance |
| A.8.9 | Configuration management | IaC (Terraform); AWS Config; Security Hub | Terraform, AWS Config | Terraform state; Config compliance |
| A.8.10 | Information deletion | S3 lifecycle policies; RDS PITR window; data retention | S3 lifecycle, RDS backup retention | S3 lifecycle rules; RDS backup config |
| A.8.11 | Data masking | Masked data in non-prod envs; Faker/Mimesis in test data | Application-level masking | Test data procedure |
| A.8.12 | Data leakage prevention | Google Workspace DLP; Defender DLP; S3 block public | Google DLP, Microsoft Purview | DLP policy config; S3 block public |
| A.8.13 | Information backup | AWS Backup; S3 versioning; RDS automated backups | AWS Backup | Backup plan config; restore test logs |
| A.8.14 | Redundancy | Multi-AZ RDS; ECS multi-AZ; Route 53 health checks | AWS Multi-AZ, Auto Scaling | Architecture diagram; uptime metrics |
| A.8.15 | Logging | CloudTrail (multi-region); VPC Flow Logs; CloudWatch | CloudTrail, CloudWatch | Trail config; log group retention |
| A.8.16 | Monitoring activities | GuardDuty; Security Hub; CloudWatch alarms; SIEM | GuardDuty, Security Hub | Findings dashboard; alarm config |
| A.8.17 | Clock synchronisation | AWS NTP (169.254.169.123) — automatic | AWS-managed | EC2 metadata NTP config |
| A.8.18 | Privileged utility programs | SSM Session Manager preferred over SSH; audit logging | AWS SSM | Session Manager audit logs |
| A.8.19 | Software installation | IaC-only prod changes; MDM software control | Terraform, Intune/Jamf | IaC deploy logs; MDM app catalog |
| A.8.20 | Network security | Security groups; NACLs; WAF; VPC private subnets | AWS Security Groups, WAF | SG rules; WAF logs |
| A.8.21 | Security of network services | TLS 1.2+ everywhere; HTTPS enforced; VPN | ALB TLS policy; ACM | Certificate config; TLS policy |
| A.8.22 | Segregation of networks | Separate VPCs (prod/dev); subnets (app/data/public) | VPC design | VPC architecture; route tables |
| A.8.23 | Web filtering | DNS filtering for endpoints; WAF for app | Cloudflare Teams / Route 53 DNS Firewall | DNS filter policy; WAF rules |
| A.8.24 | Cryptography | KMS CMKs; TLS 1.2+; AES-256 at rest; ACM | AWS KMS, ACM | Encryption status; KMS key policy |
| A.8.25 | Secure SDLC | Security requirements in tickets; SAST in CI | Semgrep, CodeQL | CI scan results; OWASP ASVS ref |
| A.8.26 | Application security requirements | OWASP ASVS; security user stories; API auth | OWASP ASVS | Security requirements doc |
| A.8.27 | Secure system architecture | Threat modelling; architecture review with security lens | STRIDE/PASTA | Architecture review docs |
| A.8.28 | Secure coding | Secure coding standards; SAST; code review | Semgrep, bandit, eslint-security | CI SAST results; PR review records |
| A.8.29 | Security testing in dev/acceptance | DAST in staging; pen test annual | OWASP ZAP, external pen tester | Pen test report; ZAP scan results |
| A.8.30 | Outsourced development | Contractor security agreement; same code review process | GitHub PR process | Contractor NDA; PR history |
| A.8.31 | Env separation | Separate AWS accounts (prod/dev); no prod data in dev | AWS Organizations | Account structure; data masking |
| A.8.32 | Change management | PR + CI required; branch protection; deployment approvals | GitHub, GitHub Actions | PR history; deployment logs |
| A.8.33 | Test information | No production PII in test; synthetic data used | Data masking script | Test environment data verification |
| A.8.34 | Audit testing protection | Pen test rules of engagement; no prod disruption | Pen test engagement letter | Rules of engagement; pen test scope |

---

## Quick Audit Evidence Map

When the auditor asks for evidence of a control, here's what to show:

| Control | What to Show the Auditor |
|---------|--------------------------|
| A.8.2 (privileged access) | IAM credential report CSV; PIM activation log export |
| A.8.5 (MFA) | Screenshot of CA policy "require MFA for all users"; IAM MFA status from credential report |
| A.8.8 (vulnerability mgmt) | Last 30-day Dependabot PR list; Inspector finding dashboard screenshot; vulnerability register |
| A.8.9 (config management) | Terraform repo URL; last `terraform plan` in CI; AWS Config compliance dashboard |
| A.8.13 (backup) | AWS Backup job history; backup test log with dated restore evidence |
| A.8.15 (logging) | CloudTrail trail config screenshot; CloudWatch log group retention screenshot |
| A.8.16 (monitoring) | GuardDuty dashboard; CloudWatch alarm list; recent security alert examples |
| A.8.24 (cryptography) | RDS encryption status; S3 encryption config; KMS key list with rotation status |
| A.8.25 (secure SDLC) | CI pipeline config showing SAST gate; sample failed PR due to security finding |
| A.8.32 (change management) | GitHub branch protection rules; sample PR with required reviewers; deploy approval log |
