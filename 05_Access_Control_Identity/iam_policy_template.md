# IAM Policy — Formal Document
**ISO 27001:2022 — A.5.15, A.5.16, A.5.18, A.8.2, A.8.3 | Version 1.0**

This is the formal IAM governance policy. The technical implementation guide is in `access_control_policy.md`.

---

## AWS IAM Role Architecture

### Production Account Role Hierarchy

```
AWS Organization Root
└── Management Account
    └── Production Account (Separate)
        ├── Roles (no long-lived IAM users)
        │   ├── ProductionReadOnly      → developers, support
        │   ├── ProductionDeployRole    → CI/CD pipeline (OIDC)
        │   ├── ProductionAdminRole     → DevOps (JIT, session-based)
        │   ├── ECSTaskExecutionRole    → ECS tasks (IAM role)
        │   ├── LambdaExecutionRole     → Lambda (function-specific)
        │   └── RootBreakGlass          → emergency only (hardware MFA)
        └── Dev Account (Separate)
            ├── DeveloperFullAccess     → engineers
            └── DevDeployRole           → CI/CD for dev/staging
```

### Least-Privilege Role Definitions

```json
// ProductionReadOnly — developers and support
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:GetLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "ecs:DescribeTasks",
        "ecs:ListTasks",
        "rds:DescribeDBInstances",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:logs:eu-west-1:[account]:log-group:/application/myapp/*",
        "arn:aws:s3:::myapp-assets/*",
        "arn:aws:ecs:eu-west-1:[account]:cluster/production"
      ]
    },
    {
      "Effect": "Deny",
      "Action": [
        "iam:*",
        "s3:DeleteObject",
        "rds:DeleteDBInstance",
        "ec2:TerminateInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

```json
// CI/CD Deploy Role — assumed via OIDC (GitHub Actions)
// Trust policy allows only the specific repo/branch
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Federated": "arn:aws:iam::[account]:oidc-provider/token.actions.githubusercontent.com"
    },
    "Action": "sts:AssumeRoleWithWebIdentity",
    "Condition": {
      "StringEquals": {
        "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
        "token.actions.githubusercontent.com:sub": "repo:myorg/myapp:ref:refs/heads/main"
      }
    }
  }]
}
```

---

## Service Control Policies (SCPs) — Org Level

Applied to production account to prevent dangerous actions:

```json
// SCP: Prevent public S3 + protect security services + enforce MFA
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyS3Public",
      "Effect": "Deny",
      "Action": ["s3:PutBucketPublicAccessBlock"],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "s3:PublicAccessBlockConfiguration/BlockPublicAcls": "false"
        }
      }
    },
    {
      "Sid": "ProtectCloudTrail",
      "Effect": "Deny",
      "Action": [
        "cloudtrail:DeleteTrail",
        "cloudtrail:StopLogging",
        "cloudtrail:UpdateTrail"
      ],
      "Resource": "*",
      "Condition": {
        "ArnNotLike": {
          "aws:PrincipalARN": "arn:aws:iam::*:role/BreakGlass"
        }
      }
    },
    {
      "Sid": "ProtectGuardDuty",
      "Effect": "Deny",
      "Action": [
        "guardduty:DeleteDetector",
        "guardduty:DisassociateMembers",
        "guardduty:StopMonitoringMembers"
      ],
      "Resource": "*"
    },
    {
      "Sid": "DenyRootNoMFA",
      "Effect": "Deny",
      "NotAction": ["sts:GetSessionToken"],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        },
        "StringLike": {
          "aws:PrincipalArn": "arn:aws:iam::*:root"
        }
      }
    },
    {
      "Sid": "DenyOutsideEU",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": ["eu-west-1", "eu-west-2", "eu-central-1"]
        }
      }
    }
  ]
}
```

---

## IAM Audit Queries

```bash
# Find all IAM users with console access (should be 0 in prod)
aws iam get-account-authorization-details --filter User \
  --query 'UserDetailList[?LoginProfile!=null].[UserName,CreateDate]'

# Find IAM roles with trust to all AWS accounts (overly permissive)
aws iam get-account-authorization-details --filter Role \
  --query 'RoleDetailList[?AssumeRolePolicyDocument.Statement[?Principal==`{"AWS":"*"}`]].RoleName'

# Find all inline policies (should be minimal)
aws iam get-account-authorization-details \
  --query 'RoleDetailList[?RolePolicyList!=null && RolePolicyList[0]!=null].[RoleName,RolePolicyList[*].PolicyName]'

# Access Analyzer — find external access to your resources
aws accessanalyzer list-analyzers
aws accessanalyzer list-findings --analyzer-arn [analyzer-arn] \
  --filter '{"status":{"eq":["ACTIVE"]}}' \
  --query 'findings[*].{Resource:resource,Type:findingType,Principal:principal}'
```

---

## IAM Policy Review Calendar

| Review Type | Frequency | Owner | Evidence |
|------------|-----------|-------|---------|
| Full IAM audit (all users, roles, policies) | Quarterly | IT Lead | Exported access analysis report |
| Unused permissions review | Quarterly | IT Lead | IAM Access Analyzer + last-accessed data |
| Service account key rotation | Annual (or on compromise) | DevOps | Key rotation log |
| SCP review | Annual | ISMS Owner | SCP version history |
| Cross-account access review | Quarterly | IT Lead | Trust policy audit |
