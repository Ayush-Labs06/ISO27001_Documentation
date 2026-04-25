# Secrets Management Guide
**ISO 27001:2022 — A.5.17, A.8.24, A.8.4 | Version 1.0**
**Owner:** DevOps Lead

Secrets = API keys, database credentials, TLS private keys, OAuth tokens, JWT signing keys, encryption keys, SSH private keys, webhook secrets.

---

## 1. The Golden Rule

> **Secrets never exist in plaintext outside of a secrets manager.**

No exceptions for:
- Environment variable files (`.env`) committed to Git
- Hardcoded in source code
- In Docker environment variables in the image
- In Kubernetes ConfigMaps
- In chat (Slack, Teams, email)
- In CI/CD environment variable UI (use secrets store integration instead)

---

## 2. Secrets Classification

| Type | Storage | Rotation | Access |
|------|---------|---------|--------|
| Database credentials | AWS Secrets Manager | Automatic (60 days) | App IAM role only |
| API keys (third-party) | AWS Secrets Manager | Annual (or on compromise) | Specific IAM role |
| Encryption/KMS keys | AWS KMS (never exported) | Automatic (annual) | Scoped KMS policy |
| TLS certificates/private keys | AWS Certificate Manager | Auto-renewal | ACM; never exported |
| JWT signing keys | AWS Secrets Manager | 90 days | Auth service IAM role |
| SSH keys (service) | AWS Secrets Manager | Annual | DevOps IAM role |
| OAuth client secrets | AWS Secrets Manager | On rotation | App IAM role |
| Webhook secrets | AWS Secrets Manager | Annual | Service IAM role |

---

## 3. AWS Secrets Manager — Setup and Usage

### Storing a Secret

```bash
# Create a database secret
aws secretsmanager create-secret \
  --name "prod/myapp/db-credentials" \
  --description "Production RDS credentials for myapp" \
  --secret-string '{"username":"dbuser","password":"[generated-password]","host":"[rds-endpoint]","port":5432,"dbname":"myapp"}' \
  --kms-key-id [kms-key-arn] \
  --tags '[{"Key":"Environment","Value":"production"},{"Key":"Service","Value":"myapp"}]'

# Enable automatic rotation (built-in Lambda for RDS)
aws secretsmanager rotate-secret \
  --secret-id "prod/myapp/db-credentials" \
  --rotation-lambda-arn [rotation-lambda-arn] \
  --rotation-rules AutomaticallyAfterDays=60
```

### Retrieving in Application Code

```python
# Python — boto3
import boto3
import json

def get_secret(secret_name: str) -> dict:
    client = boto3.client('secretsmanager', region_name='eu-west-1')
    response = client.get_secret_value(SecretId=secret_name)
    return json.loads(response['SecretString'])

# Usage
db_creds = get_secret('prod/myapp/db-credentials')
db_conn = connect(
    host=db_creds['host'],
    user=db_creds['username'],
    password=db_creds['password']
)
```

```javascript
// Node.js
const { SecretsManagerClient, GetSecretValueCommand } = require('@aws-sdk/client-secrets-manager');

async function getSecret(secretName) {
  const client = new SecretsManagerClient({ region: 'eu-west-1' });
  const response = await client.send(new GetSecretValueCommand({ SecretId: secretName }));
  return JSON.parse(response.SecretString);
}
```

### IAM Policy — Least Privilege for Secrets Access

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["secretsmanager:GetSecretValue"],
      "Resource": "arn:aws:secretsmanager:eu-west-1:[account-id]:secret:prod/myapp/*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "eu-west-1"
        }
      }
    }
  ]
}
```

---

## 4. Kubernetes Secrets — External Secrets Operator

Don't rely on Kubernetes Secrets alone — etcd must be encrypted AND you need to manage rotation. Use External Secrets Operator (ESO) to sync from AWS Secrets Manager:

```yaml
# ExternalSecret — pulls from AWS Secrets Manager and creates K8s Secret
apiVersion: external-secrets.io/v1beta1
kind: ExternalSecret
metadata:
  name: db-credentials
  namespace: production
spec:
  refreshInterval: 1h
  secretStoreRef:
    name: aws-secretsmanager
    kind: SecretStore
  target:
    name: db-credentials
    creationPolicy: Owner
  data:
  - secretKey: DB_PASSWORD
    remoteRef:
      key: prod/myapp/db-credentials
      property: password
  - secretKey: DB_USERNAME
    remoteRef:
      key: prod/myapp/db-credentials
      property: username
```

---

## 5. CI/CD — GitHub Actions Secrets

**Don't** use GitHub Actions environment variables UI for production secrets. Use OIDC + AWS Secrets Manager:

```yaml
# GitHub Actions — OIDC to AWS (no long-lived keys)
name: Deploy
on:
  push:
    branches: [main]

permissions:
  id-token: write
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    
    - name: Configure AWS credentials via OIDC
      uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: arn:aws:iam::[account-id]:role/github-actions-deploy
        aws-region: eu-west-1
    
    - name: Get secret from Secrets Manager
      run: |
        SECRET=$(aws secretsmanager get-secret-value \
          --secret-id prod/myapp/deploy-key \
          --query SecretString --output text)
        echo "::add-mask::$SECRET"
        echo "DEPLOY_KEY=$SECRET" >> $GITHUB_ENV
```

---

## 6. Secret Scanning — Detect Exposed Secrets

### Pre-commit Hook (gitleaks)

```bash
# Install gitleaks
brew install gitleaks  # macOS
# or download binary from github.com/gitleaks/gitleaks

# Run on current repo
gitleaks detect --source . --verbose

# Pre-commit hook
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
gitleaks protect --staged --verbose
if [ $? -ne 0 ]; then
  echo "Secrets detected! Commit blocked."
  exit 1
fi
EOF
chmod +x .git/hooks/pre-commit
```

### GitHub Push Protection

Enable in GitHub org settings:
- Security → Code security and analysis → Secret scanning → Push protection: **Enable**

This blocks pushes containing detected secrets (150+ patterns supported).

### CI/CD — Trufflehog

```yaml
# GitHub Actions step
- name: Scan for secrets
  uses: trufflesecurity/trufflehog@main
  with:
    path: ./
    base: ${{ github.event.repository.default_branch }}
    extra_args: --debug --only-verified
```

---

## 7. Audit and Rotation Schedule

| Secret Type | Rotation Interval | Method | Owner |
|------------|------------------|--------|-------|
| RDS passwords | 60 days (automatic) | Secrets Manager Lambda | DevOps |
| API keys (external) | Annual | Manual + Secrets Manager update | Service owner |
| JWT signing keys | 90 days | Automated Lambda | Engineering |
| SSH service keys | Annual | Manual | DevOps |
| TLS certificates | On expiry (auto-renewal) | ACM / Let's Encrypt ACME | DevOps |
| OAuth client secrets | Annual | Manual | Service owner |

```bash
# List all secrets and their last rotation date
aws secretsmanager list-secrets \
  --query 'SecretList[*].{Name:Name,LastRotated:LastRotatedDate,AutoRotation:RotationEnabled}' \
  --output table

# Alert on secrets not rotated in >90 days
aws secretsmanager list-secrets \
  --query 'SecretList[?LastRotatedDate < `2024-01-01`].[Name,LastRotatedDate]'
```

---

## 8. Emergency Response — Compromised Secret

If a secret is suspected compromised:

1. **Immediately revoke** the compromised secret in the issuing system (API provider, AWS console)
2. **Generate new secret** and update in Secrets Manager
3. **Deploy new secret** to all consuming applications (may require rolling restart)
4. **Audit usage** of the compromised secret (CloudTrail, application logs) for the previous 30 days
5. **Raise an incident** per the Incident Response Plan
6. **Identify source** of exposure (git history, logs, error messages, Slack history)
7. **Document findings** and add to risk register

```bash
# Rotate a secret immediately
aws secretsmanager rotate-secret --secret-id [secret-name] --rotate-immediately

# Check who accessed a secret (CloudTrail)
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=ResourceName,AttributeValue=[secret-arn] \
  --query 'Events[*].{Time:EventTime,User:Username,Action:EventName}'
```
