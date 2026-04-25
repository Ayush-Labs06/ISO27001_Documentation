# Change Management Policy
**ISO 27001:2022 — A.8.32, A.5.8 | Version 1.0**
**Owner:** Engineering Lead / ISMS Owner
**Approved by:** [CTO] | [Date]

---

## 1. Purpose

To ensure changes to information systems, infrastructure, and ISMS processes are made in a planned, authorized, and reversible manner — reducing security risk from uncontrolled changes.

> **IRL Note for startups:** This doesn't mean a change approval committee for every PR. It means having a defined, consistent process. A PR with 1 required reviewer + CI passing is a valid change control process.

---

## 2. Scope

Applies to:
- Application code changes deployed to production
- Infrastructure changes (Terraform, CloudFormation, manual console changes)
- Configuration changes (IAM policies, security groups, database settings, Kubernetes configs)
- ISMS process and policy changes
- Network and access control changes

---

## 3. Change Categories

| Category | Description | Authorization Required | Process |
|----------|-------------|----------------------|---------|
| **Standard** | Low-risk, well-understood, pre-approved | Pre-approved by ISMS Owner | Follow standard deployment process (PR + CI) |
| **Normal** | Planned change with defined impact | Tech Lead / Engineering Manager | PR review + test in staging + deployment plan |
| **Significant** | High-impact, high-risk, or novel change | CTO + Tech Lead | CAB review (informal); staged rollout; rollback plan required |
| **Emergency** | Urgent fix to restore service or contain incident | On-call Lead (verbal); document post-hoc | Fast-track; post-change review required |

---

## 4. Change Request Process

### Normal Changes

1. **Raise PR** with description of change, rationale, and impact assessment
2. **Security check** — does this change affect:
   - [ ] IAM roles, policies, or access controls?
   - [ ] Network configuration (security groups, routing, firewall)?
   - [ ] Encryption settings?
   - [ ] Authentication or authorisation logic?
   - [ ] External-facing endpoints?
   - [ ] Data handling or storage?
   - If any checked: flag for additional review or ISMS Owner sign-off
3. **Peer review** — minimum 1 reviewer; 2 for security-relevant changes
4. **CI/CD pipeline** — all tests must pass; SAST must pass; no secrets detected
5. **Staging test** — deploy to staging; verify expected behaviour
6. **Approval and merge** — approved by required reviewers
7. **Deploy and verify** — monitor deployment; verify post-deploy

### Significant Changes

Significant changes (IAM policy overhaul, new cloud account, major architecture change, security control removal) require:

1. **Change proposal document** (1-pager): what, why, impact, risks, rollback plan
2. **Review session** with CTO + ISMS Owner (15-30 min)
3. **Staged rollout** (canary or phased) where possible
4. **Rollback procedure** documented and tested before deployment
5. **Post-change monitoring** — define metrics and alert thresholds; monitor for 24-48h

---

## 5. Emergency Changes

Emergency changes bypass normal review for speed but require:
- Verbal approval from CTO or Tech Lead before implementation
- Post-change review within 24 hours
- Documentation in change log with: reason for emergency, approver, actions taken
- Security review if the emergency involved any security control modification

---

## 6. Infrastructure-as-Code Requirement

All infrastructure changes must be:
- Defined in IaC (Terraform, CloudFormation, Pulumi) where possible
- Reviewed via PR before applying
- Applied via CI/CD pipeline (Atlantis, GitHub Actions, Terraform Cloud) — not via manual console changes in production

**Console changes in production are prohibited except for emergency response.**

If an emergency console change is made:
1. Document immediately in the incident or change log
2. Convert to IaC within 5 business days
3. Apply via proper process to ensure state consistency

---

## 7. Rollback Requirements

Every significant change must have a rollback plan documented before deployment:

| Change Type | Rollback Method |
|------------|----------------|
| Application deployment | Previous Docker image tag; ECS/EKS rollout undo |
| Database migration | Reverse migration script; restore from snapshot |
| Infrastructure (Terraform) | `terraform plan` of revert; tested in staging |
| IAM policy change | Previous policy version stored; one-click revert |
| Network config change | Previous security group rules saved |

---

## 8. ISMS Process and Policy Changes (Clause 6.3)

Changes to ISMS documentation, policies, or procedures follow this process:
1. Draft change with rationale
2. Review by ISMS Owner (and relevant stakeholders for major changes)
3. Update version number and review date in document
4. Approval sign-off (same as original approval level)
5. Communicate change to affected parties
6. Archive previous version

---

## 9. Change Log

All changes to production systems are captured via:
- **Application:** Git commit history + PR records (GitHub)
- **Infrastructure:** Terraform state + Atlantis plan logs
- **Configuration:** AWS Config change history; CloudTrail

Significant and emergency changes are additionally logged in the Change Register:

| Date | Change ID | Description | Category | Approver | Outcome | Notes |
|------|----------|-------------|----------|----------|---------|-------|
| | | | | | | |

---

## 10. Prohibited Changes

The following changes are **never** permitted without explicit ISMS Owner + CTO approval:
- Disabling audit logging (CloudTrail, GuardDuty, Security Hub)
- Removing MFA requirements from any system
- Opening inbound 0.0.0.0/0 on production security groups (except WAF/ALB)
- Making S3 buckets publicly accessible
- Granting AdministratorAccess IAM policies to users or roles
- Disabling encryption on RDS, S3, or EBS volumes

| Field | Detail |
|-------|--------|
| Document ID | ISMS-CHG-001 |
| Owner | Engineering Lead / ISMS Owner |
| Review cycle | Annual |
| Next review | [Date] |
