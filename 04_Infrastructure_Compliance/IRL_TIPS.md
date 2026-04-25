# IRL Tips — Infrastructure Compliance

---

## 1. CloudTrail is Evidence — Without It You Have Nothing

The most common critical gap: CloudTrail not enabled in all regions, or not delivering to a locked S3 bucket. Auditors ask for:
- "Show me your audit logging setup"
- "How do you know if someone made unauthorized changes?"
- "How would you detect if an admin account was compromised?"

All roads lead to CloudTrail. Enable it on day 1, set log file validation, put logs in S3 with Object Lock. Then you have 1 year of tamper-proof audit trail. Without it, you can't evidence most of A.8.15 and A.8.16.

---

## 2. IaC Drift is the Silent Killer

The infra compliance checklist means nothing if someone made a manual console change that drifted Terraform state. Before any audit:

```bash
# Check for drift in all Terraform workspaces
terraform plan -detailed-exitcode
# Exit code 2 = changes detected (drift)

# AWS Config drift detection for CloudFormation stacks
aws cloudformation detect-stack-drift --stack-name [stack-name]
```

Set up AWS Config rules that alert on key resource changes (security groups, S3 bucket policies, IAM policies). Any console change that contradicts IaC is a finding.

---

## 3. The MFA Gap That Always Exists

Even when clients say "we have MFA everywhere," there's always a gap. Check these:
- AWS IAM users created for old integrations (pre-OIDC era) — no MFA because they're "service accounts"
- GitHub personal accounts of contractors — org-level 2FA may not be enforced retroactively
- Stale SSO accounts that bypass Conditional Access due to legacy auth
- Break-glass accounts that have MFA but it's stored in the same vault that's compromised

Run the IAM credential report. Export it. Check `mfa_active=false` for every user. That list is your gap.

---

## 4. S3 Public Access — Check at Account Level, Not Bucket Level

Clients often say "our S3 buckets are private." Check:

```bash
# This is the one that matters — account-level block
aws s3control get-public-access-block --account-id [account-id]
```

If `BlockPublicAcls`, `IgnorePublicAcls`, `BlockPublicPolicy`, and `RestrictPublicBuckets` are all `true` at the account level, individual bucket settings can't override it. This is the right place to enforce it. Bucket-level settings are fragile.

---

## 5. ECR Scanning is Not Enough — You Need Trivy in CI Too

Amazon Inspector ECR scanning catches CVEs after push. But you want to catch them before push (fail the build). Put Trivy in your CI pipeline with `--exit-code 1` for CRITICAL. Then Inspector provides defense-in-depth and a central findings dashboard.

The common mistake: enabling Inspector but not blocking deployments on findings. The developer pushes a Critical CVE image, Inspector finds it — but it's already in production.

---

## 6. Kubernetes Security: The Auditor Doesn't Know K8s

Most ISO 27001 auditors are not Kubernetes experts. They will ask:
- "How do you control who can access the cluster?"
- "How do you ensure containers don't run as root?"
- "How are secrets managed in K8s?"

Have screenshots ready:
- RBAC role list for production namespace
- Pod security context from a production deployment (`kubectl get pod [pod] -o yaml | grep -A10 securityContext`)
- External Secrets Operator syncing from Secrets Manager

You don't need to explain Kubernetes in depth — just map each control to the ISO clause and show evidence.

---

## 7. Pen Test: What Auditors Actually Check

Pen test is not required by ISO 27001 (unlike PCI-DSS), but auditors ask about it under:
- A.8.8 (technical vulnerability management)
- A.8.29 (security testing in development and acceptance)
- A.5.35 (independent review of IS)

If the client had a pen test in the last 12 months: bring the executive summary and the finding remediation evidence. If not: frame your automated scanning (Inspector + Trivy + SAST) as the primary control. The auditor may accept this for a first cert, but recommend scheduling a pen test before Year 2 surveillance.

---

## 8. Shared Responsibility Is Not an Excuse

"AWS handles that" is only valid when you've documented what AWS handles and what you handle. For every physical/environmental Annex A control you exclude due to shared responsibility:
- Reference the AWS/Azure ISO 27001 certificate (download from their compliance page)
- Document the shared responsibility model in the SoA exclusion rationale
- Show you've reviewed and understood what's on your side of the line

Auditors have seen "AWS handles security" as a blanket excuse. Show you know the line.

---

## 9. KMS Key Sprawl is a Real Problem

Startups often create KMS keys for everything, then forget about them. Before audit:
```bash
aws kms list-keys | jq '.Keys[].KeyId' | xargs -I {} aws kms describe-key --key-id {} \
  --query 'KeyMetadata.{ID:KeyId,Status:KeyState,RotationEnabled:KeyRotationStatus,Desc:Description}'
```

Audit findings: keys without rotation enabled, keys used for wrong purpose (no tagging), keys accessible to too many principals. Fix before audit.

---

## 10. DevSecOps Evidence for Auditors

Keep CI/CD pipeline scan results as evidence artifacts. This is easy:
- GitHub Actions: SARIF uploads to GitHub Security tab — permanent record
- Trivy: save scan output as artifact per build
- Semgrep: upload SARIF; Security tab shows finding history

When the auditor asks "how do you know your code is free of known vulnerabilities?", you open the GitHub Security tab and show the last 30 days of scan results + closed findings. That's A.8.28 evidenced in 60 seconds.
