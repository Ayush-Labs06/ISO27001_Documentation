# IRL Tips — Access Control & Identity

---

## 1. IAM Drift Happens Every Sprint

Developers create IAM roles for quick testing, then forget them. Infra added via console (not Terraform) doesn't appear in state. Contractors finish and their access lingers. The quarterly access review catches the drift, but between reviews, use:

```bash
# IAM Access Analyzer: finds unintended external access
aws accessanalyzer list-findings --analyzer-arn [arn] \
  --filter '{"status":{"eq":["ACTIVE"]}}'

# Last-accessed data: identify permissions that haven't been used in 90+ days
aws iam generate-service-last-accessed-details --arn [role-arn]
sleep 5
aws iam get-service-last-accessed-details --job-id [job-id] \
  --query 'ServicesLastAccessed[?LastAuthenticated==null || LastAuthenticated < `2023-10-01`]'
```

Remove unused permissions. It reduces attack surface and shows auditors you practice least-privilege.

---

## 2. The Offboarding Gap That Always Bites

The most common "oops" in startups: an ex-employee's GitHub account is removed from the org but their local clone of the private repo is still there, and they still have NPM/PyPI package publish rights (separate from GitHub).

Checklist items that get missed:
- npm org membership (check `npmjs.com/org/[org]`)
- Google Cloud (separate from AWS)
- Old CI/CD service tokens (Jenkins, CircleCI)
- Slack guest workspaces they were added to
- Domain registrars, hosting panels
- Shared API keys they "personally" had documented

The fix: maintain a system-by-system offboarding list that grows as you add tools.

---

## 3. OIDC for GitHub Actions is Non-Negotiable

If the client has long-lived AWS access keys in GitHub Actions secrets, flag it immediately. It's a A.5.17 gap and a real risk (GitHub secrets can be exfiltrated via workflow injection). OIDC takes 30 minutes to set up and eliminates the need for long-lived keys entirely.

For auditors: show them the GitHub workflow file with `permissions: id-token: write` and the AWS role trust policy with the OIDC condition. That's clean evidence for A.8.5 and A.5.17.

---

## 4. SSO Gaps in SaaS Tooling

Most SaaS tools support SSO (SAML 2.0 / OIDC) with enterprise plans. But startups often have:
- Free/startup tier on tools that don't support SSO
- Teams where someone signed up personally before the company account existed
- Tools where SSO was configured but some accounts were created before SSO was enforced

Audit this by checking each tool's member list against your IdP user list. Any account not federated through SSO is an offboarding gap waiting to happen.

---

## 5. Access Reviews — Get Managers to Actually Certify

The most common evidence weakness: IT Lead does the access review themselves without involving managers. Auditors want to see that the person who knows the business need (the manager) confirmed that access is still appropriate — not just that IT looked at a list.

Practical approach:
- Send a Notion/Confluence table per manager once a quarter: "Please confirm your team's access below"
- Screenshot the manager's comment or email approval
- That's your evidence of management confirmation

Takes 20 minutes per manager. Worth it.

---

## 6. Entra ID PIM is Evidence Gold

If the client uses Entra ID PIM, the PIM activation logs are perfect audit evidence for A.8.2 and A.5.18. Every privileged access request, the justification provided, who approved it, and when it was revoked — all in one place with timestamps. Export this as a PDF before Stage 2 and have it ready.
