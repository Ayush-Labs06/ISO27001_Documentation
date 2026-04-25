# Patch Management Procedure
**ISO 27001:2022 — A.8.8, A.8.19 | Version 1.0**
**Owner:** DevOps Lead

---

## 1. Patch Classification and SLAs

| Severity | CVSS | Definition | SLA |
|----------|------|-----------|-----|
| **Emergency** | 9.0–10.0 + actively exploited (CISA KEV) | Known exploit in the wild; immediate risk | **48 hours** |
| **Critical** | 9.0–10.0 | Remote code execution; privilege escalation; auth bypass | **7 days** |
| **High** | 7.0–8.9 | Significant risk; likely exploit exists | **30 days** |
| **Medium** | 4.0–6.9 | Moderate risk; complex exploitation | **90 days** |
| **Low** | 0.1–3.9 | Minimal risk | **Next planned maintenance** |

---

## 2. Scope

| Asset Type | Patching Method | Owner |
|-----------|----------------|-------|
| EC2 (Amazon Linux / Ubuntu) | AWS Systems Manager Patch Manager | DevOps |
| Container base images | Rebuild + redeploy on new base image | DevOps |
| Application dependencies (npm, pip, go modules) | Dependabot / Renovate PRs | Engineering |
| Employee laptops (macOS, Windows) | Intune / Jamf MDM auto-update | IT Lead |
| RDS (minor versions) | Auto minor version upgrade enabled | DevOps (automated) |
| EKS cluster version | Managed upgrade via `aws eks update-cluster-version` | DevOps |
| Network equipment | Per-vendor guidance | IT Lead |

---

## 3. EC2 Patch Management (AWS SSM Patch Manager)

```bash
# Create patch baseline — Amazon Linux 2
aws ssm create-patch-baseline \
  --name "ProductionSecurityBaseline" \
  --operating-system AMAZON_LINUX_2 \
  --approval-rules '{"PatchRules":[
    {
      "PatchFilterGroup": {
        "PatchFilters": [
          {"Key": "SEVERITY", "Values": ["Critical","Important"]},
          {"Key": "CLASSIFICATION", "Values": ["Security"]}
        ]
      },
      "ApproveAfterDays": 3,
      "ComplianceLevel": "CRITICAL"
    },
    {
      "PatchFilterGroup": {
        "PatchFilters": [
          {"Key": "SEVERITY", "Values": ["Medium","Low"]},
          {"Key": "CLASSIFICATION", "Values": ["Security"]}
        ]
      },
      "ApproveAfterDays": 30,
      "ComplianceLevel": "HIGH"
    }
  ]}'

# Create maintenance window — Weekly Sunday 02:00-04:00 UTC
aws ssm create-maintenance-window \
  --name "WeeklyPatchWindow" \
  --schedule "cron(0 2 ? * SUN *)" \
  --duration 2 \
  --cutoff 1 \
  --allow-unassociated-targets

# Register targets (all EC2 with tag Environment=production)
aws ssm register-target-with-maintenance-window \
  --window-id [window-id] \
  --resource-type INSTANCE \
  --targets '[{"Key":"tag:Environment","Values":["production"]}]'

# Register patch task
aws ssm register-task-with-maintenance-window \
  --window-id [window-id] \
  --task-arn "arn:aws:ssm:::automation-definition/AWS-RunPatchBaseline:$DEFAULT" \
  --task-type AUTOMATION \
  --targets '[{"Key":"tag:Environment","Values":["production"]}]' \
  --task-invocation-parameters '{"AutomationParameters":{"Operation":["Install"],"RebootOption":["RebootIfNeeded"]}}'

# Check patch compliance
aws ssm describe-instance-patch-states-for-patch-group \
  --patch-group "production" \
  --query 'InstancePatchStates[*].{Instance:InstanceId,Missing:MissingCount,Failed:FailedCount,InstalledPending:InstalledPendingRebootCount}'
```

---

## 4. Container Image Patching

Container images must be rebuilt and redeployed when:
- A Critical/High CVE is found in the base image (Trivy / Inspector scan)
- The base image publisher releases a security update

```bash
# Weekly: rebuild all production images with latest base
# Example Makefile target
rebuild-all:
  docker pull python:3.11-slim  # Pull latest tag
  docker build --no-cache -t myapp:$(git rev-parse --short HEAD) .
  trivy image --severity CRITICAL,HIGH --exit-code 1 myapp:$(git rev-parse --short HEAD)
  docker push [ecr-url]/myapp:$(git rev-parse --short HEAD)

# Schedule weekly rebuilds via CI/CD (cron trigger)
# GitHub Actions:
# on:
#   schedule:
#     - cron: '0 3 * * MON'  # Monday 03:00 UTC
```

---

## 5. EKS Version Management

| Check | Frequency | Action |
|-------|-----------|--------|
| EKS cluster version vs. available | Monthly | Upgrade if more than 2 minor versions behind |
| Node group AMI | Monthly | Upgrade node group AMI after cluster upgrade |
| Add-on versions (CoreDNS, kube-proxy, VPC CNI) | Monthly | Update add-ons after cluster upgrade |

```bash
# Check current EKS version and available updates
aws eks describe-cluster --name [cluster-name] --query 'cluster.version'
aws eks describe-addon-versions --query 'addons[].{Name:addonName,Versions:addonVersions[0].addonVersion}'

# Upgrade cluster (test in dev first)
aws eks update-cluster-version --name [cluster-name] --kubernetes-version 1.29

# Update managed node group
aws eks update-nodegroup-version \
  --cluster-name [cluster-name] \
  --nodegroup-name [nodegroup-name] \
  --release-version latest
```

---

## 6. Dependency Patching (Application Code)

Dependabot (GitHub) handles automated dependency PRs:

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 10
    groups:
      security-updates:
        applies-to: security-updates
        patterns: ["*"]
    reviewers:
      - "devops-team"
    labels:
      - "security"
      - "dependencies"

  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 5

  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"

  - package-ecosystem: "terraform"
    directory: "/terraform"
    schedule:
      interval: "weekly"
```

---

## 7. Patch Compliance Reporting

Monthly patch compliance report to ISMS Owner:

| Metric | Target | Current |
|--------|--------|---------|
| EC2 instances with 0 critical missing patches | 100% | |
| EC2 instances with 0 high missing patches | 100% | |
| Avg days to patch critical vulnerabilities | ≤ 7 days | |
| Dependabot PRs merged within SLA | 100% | |
| Container images rebuilt within 7 days of base image CVE | 100% | |
| Employee laptops on current OS major version | ≥ 95% | |

```bash
# Generate patch compliance summary
aws ssm describe-instance-patch-states \
  --query 'InstancePatchStates[*].{ID:InstanceId,Missing:MissingCount,Failed:FailedCount,NotApplicable:NotApplicableCount}' \
  --output table

# Find instances with missing critical patches
aws ssm describe-instance-patches \
  --instance-id [instance-id] \
  --filters "Key=State,Values=Missing" "Key=Severity,Values=Critical" \
  --query 'Patches[*].[Title,Severity,KBId]' \
  --output table
```

---

## 8. Patch Exceptions

If a patch cannot be applied within the SLA (compatibility issue, requires extended testing):

1. Raise exception to ISMS Owner with:
   - CVE ID and severity
   - Reason for exception
   - Proposed alternative mitigation (WAF rule, network isolation, monitoring)
   - Target remediation date
2. Document in risk register as temporary accepted risk
3. Review exception monthly until resolved
4. No exception granted for Critical vulnerabilities with public exploits
