# DevSecOps Integration Checklist
**ISO 27001:2022 — A.8.25, A.8.26, A.8.28, A.8.29, A.8.30, A.8.31 | Version 1.0**
**Owner:** Engineering Lead / DevOps Lead

Shift security left. Every stage of the pipeline is a security gate.

---

## 1. Pre-Commit Controls (Developer Workstation)

| # | Control | Tool | Config | Status | Annex A |
|---|---------|------|--------|--------|---------|
| 1.1 | Secret detection pre-commit hook | gitleaks / detect-secrets | `.pre-commit-config.yaml` | | A.8.4 |
| 1.2 | Local SAST linting (security-focused rules) | semgrep / bandit (Python) / eslint-plugin-security (JS) | `.semgrepignore` | | A.8.28 |
| 1.3 | Commit signing enabled | GPG / SSH signing | `git config gpg.format ssh` | | A.8.9 |
| 1.4 | Branch protection: no direct push to main | GitHub branch protection rules | | | A.8.32 |

```yaml
# .pre-commit-config.yaml
repos:
- repo: https://github.com/gitleaks/gitleaks
  rev: v8.18.0
  hooks:
  - id: gitleaks

- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v4.5.0
  hooks:
  - id: detect-private-key
  - id: check-json
  - id: check-yaml

- repo: https://github.com/returntocorp/semgrep
  rev: v1.52.0
  hooks:
  - id: semgrep
    args: ['--config', 'p/security-audit', '--error']
```

---

## 2. Pull Request Controls

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 2.1 | Minimum 1 required reviewer (2 for security-critical changes) | GitHub branch protection | | A.8.32 |
| 2.2 | Status checks must pass before merge | GitHub required status checks | | A.8.32 |
| 2.3 | PR template includes security checklist | `.github/pull_request_template.md` | | A.8.25 |
| 2.4 | CODEOWNERS for sensitive files (IAM, Terraform security configs) | `.github/CODEOWNERS` | | A.8.4 |
| 2.5 | Dependabot alerts reviewed before merge | GitHub Dependabot settings | | A.8.8 |

```markdown
<!-- .github/pull_request_template.md -->
## Security Checklist
- [ ] No secrets or credentials in this PR
- [ ] Dependencies updated (Dependabot alerts reviewed)
- [ ] Does this PR change IAM policies, network config, or auth logic? (If yes, request ISMS Owner review)
- [ ] Does this PR process new types of user data? (If yes, data classification reviewed)
- [ ] Logging added for new security-relevant events
```

```
# .github/CODEOWNERS
# Security-sensitive files require security team approval
/terraform/iam/          @security-team
/terraform/networking/   @security-team
/.github/workflows/      @security-team
/k8s/rbac/              @security-team
```

---

## 3. CI/CD Pipeline Security Gates

### Required Gates (Block on Failure)

| # | Gate | Tool | Failure Action | Annex A |
|---|------|------|---------------|---------|
| 3.1 | Secret scanning | Trufflehog / gitleaks | Block merge | A.8.4 |
| 3.2 | SAST (static analysis) | Semgrep / CodeQL / Bandit | Block on HIGH+ | A.8.28 |
| 3.3 | SCA — dependency vulnerabilities | Snyk / OWASP Dependency-Check | Block on CRITICAL | A.8.8 |
| 3.4 | Container image scan | Trivy (in CI) / Amazon Inspector | Block on CRITICAL | A.8.8 |
| 3.5 | IaC security scan | Checkov / Terrascan / tfsec | Block on HIGH+ | A.8.9 |
| 3.6 | License compliance | Fossa / TLDR Legal | Alert on GPL copyleft | A.5.32 |

### Advisory Gates (Alert, Don't Block)

| # | Gate | Tool | Annex A |
|---|------|------|---------|
| 3.7 | DAST (dynamic analysis) in staging | OWASP ZAP / Nuclei | A.8.29 |
| 3.8 | API security testing | Postman security collection / schemathesis | A.8.26 |
| 3.9 | Infrastructure drift detection | Terraform plan diff | A.8.9 |

```yaml
# GitHub Actions — Full security pipeline
name: Security Gates
on: [pull_request]

permissions:
  contents: read
  security-events: write
  id-token: write

jobs:
  secret-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    - name: TruffleHog scan
      uses: trufflesecurity/trufflehog@main
      with:
        path: ./
        base: ${{ github.event.repository.default_branch }}
        extra_args: --only-verified

  sast:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Semgrep SAST
      uses: returntocorp/semgrep-action@v1
      with:
        config: >-
          p/security-audit
          p/owasp-top-ten
          p/python
        auditOn: push

  sca:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Snyk dependency scan
      uses: snyk/actions/node@master  # or python, java, etc.
      env:
        SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
      with:
        args: --severity-threshold=critical --fail-on=upgradable

  container-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Build image
      run: docker build -t myapp:${{ github.sha }} .
    - name: Trivy image scan
      uses: aquasecurity/trivy-action@master
      with:
        image-ref: myapp:${{ github.sha }}
        format: sarif
        output: trivy-results.sarif
        severity: CRITICAL,HIGH
        exit-code: '1'
    - uses: github/codeql-action/upload-sarif@v3
      with:
        sarif_file: trivy-results.sarif

  iac-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Checkov IaC scan
      uses: bridgecrewio/checkov-action@master
      with:
        directory: terraform/
        framework: terraform
        soft_fail: false
        check: HIGH,CRITICAL
```

---

## 4. Terraform / IaC Security

| # | Control | Tool | Config | Status | Annex A |
|---|---------|------|--------|--------|---------|
| 4.1 | Terraform state in encrypted S3 with DynamoDB locking | `backend "s3"` with `encrypt=true` | | A.8.24 |
| 4.2 | Terraform plan output reviewed before apply | Atlantis / GitHub Actions + `terraform plan` | | A.8.32 |
| 4.3 | No `terraform apply` from developer workstations in prod | CI/CD only; IAM policy restriction | | A.8.32 |
| 4.4 | Sensitive values in Terraform use `sensitive = true` | Terraform variable config | | A.5.17 |
| 4.5 | tfsec / Checkov run on all Terraform PRs | Pre-merge CI gate | | A.8.9 |
| 4.6 | AWS Config detects Terraform drift | AWS Config + drift alerts | | A.8.9 |

```hcl
# Secure Terraform backend
terraform {
  backend "s3" {
    bucket         = "myorg-terraform-state"
    key            = "production/terraform.tfstate"
    region         = "eu-west-1"
    encrypt        = true
    kms_key_id     = "arn:aws:kms:eu-west-1:[account]:key/[key-id]"
    dynamodb_table = "terraform-state-lock"
  }
}

# Sensitive variable
variable "db_password" {
  type      = string
  sensitive = true  # Won't appear in plan output or logs
}
```

---

## 5. Secrets in CI/CD — Do's and Don'ts

| Don't | Do |
|-------|-----|
| Store secrets in GitHub Actions env vars UI | Use OIDC + AWS Secrets Manager |
| Hardcode secrets in workflow files | Use `${{ secrets.NAME }}` (GitHub-encrypted) |
| Print secrets with `echo` | Mask with `::add-mask::` |
| Use long-lived AWS access keys in CI | OIDC role assumption |
| Commit `.env` files | `.gitignore` + pre-commit hook |

---

## 6. Environment Separation (A.8.31)

| Control | Implementation | Status | Annex A |
|---------|---------------|--------|---------|
| Production in separate AWS account | AWS Organizations multi-account | | A.8.31 |
| No production data in dev/staging | Policy + data masking | | A.8.31, A.8.33 |
| Separate IAM roles per environment | Terraform workspaces + account-specific roles | | A.8.31 |
| CD to production requires explicit approval | GitHub Environments: required reviewers | | A.8.32 |
| Production deployments logged | CloudTrail + CI/CD audit logs | | A.8.15 |

```yaml
# GitHub Actions — production deployment with manual approval
jobs:
  deploy-production:
    needs: [deploy-staging, integration-tests]
    runs-on: ubuntu-latest
    environment:
      name: production  # requires manual approval in GitHub settings
      url: https://app.company.com
    steps:
    - name: Deploy to production
      run: ./scripts/deploy.sh production
```

---

## 7. Dependency Management (A.8.8, A.5.21)

| # | Control | Tool | Status | Annex A |
|---|---------|------|--------|---------|
| 7.1 | Dependabot / Renovate enabled for all repos | GitHub Dependabot | | A.8.8 |
| 7.2 | Critical dependency CVEs auto-PRed within 24h | Dependabot security updates: enabled | | A.8.8 |
| 7.3 | `package-lock.json` / `poetry.lock` committed and pinned | Lock file in repo | | A.5.21 |
| 7.4 | Docker base image pinned by digest | Dockerfile `FROM image@sha256:...` | | A.5.21 |
| 7.5 | SBOM generated per release | Syft / Trivy SBOM output | | A.5.21 |

```bash
# Generate SBOM with Syft
syft packages myapp:latest -o spdx-json > sbom.json

# Generate SBOM with Trivy
trivy image --format spdx-json --output sbom.json myapp:latest
```
