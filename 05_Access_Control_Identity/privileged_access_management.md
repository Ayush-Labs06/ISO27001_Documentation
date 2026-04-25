# Privileged Access Management (PAM)
**ISO 27001:2022 — A.8.2, A.5.18 | Version 1.0**
**Owner:** IT Lead / DevOps Lead

---

## 1. What Counts as Privileged Access

| Access Type | Examples | Controls Required |
|------------|---------|-----------------|
| AWS root account | Account root login | Hardware MFA; no API keys; emergency use only |
| AWS admin IAM | AdministratorAccess IAM policy | MFA; JIT access; session logging |
| Production database admin | RDS superuser, `GRANT ALL` roles | JIT via SSM; session logging; no standing access |
| Kubernetes cluster-admin | `cluster-admin` ClusterRoleBinding | JIT; audit logging; Entra ID/OIDC auth |
| Git org owner | GitHub organization owner | SSO + MFA; limited to 2-3 people |
| IdP Global Administrator | Entra ID Global Admin | PIM JIT; hardware MFA; audit logs |
| Production server root/sudo | Linux root or sudo-capable user | SSM Session Manager; sudo logging |

---

## 2. Privileged Access Principles

1. **No standing privilege** — Admin access is not always-on; it is elevated for specific tasks and then revoked
2. **Just-in-time (JIT)** — Access granted for a time-limited window (e.g., 1-4 hours) with documented reason
3. **Just-enough access** — Only the permissions required for the specific task, not full admin
4. **Every session logged** — All privileged sessions must generate an immutable audit trail
5. **Break-glass last resort** — Emergency accounts exist but their use triggers an immediate security review

---

## 3. AWS Privileged Access

### Option A: AWS SSO (Preferred for Console + CLI)

```bash
# Users assume temporary credentials via AWS SSO (no long-lived access keys)
aws configure sso

# Login
aws sso login --profile prod-admin

# Assume role for production access (granted for 1-4 hours)
aws sts assume-role \
  --role-arn arn:aws:iam::[account]:role/ProdAdminRole \
  --role-session-name "DevOpsLead-2024-01-15-DB-migration" \
  --duration-seconds 3600
```

### Option B: JIT Access via AWS Identity Center (Recommended)

Configure time-bounded permission sets:
```
Admin requests elevated access via: [ticketing system / Slack command]
  → ISMS Owner approves (auto-approval for on-call for P1 incidents)
  → AWS Identity Center grants permission set for 2 hours
  → Session ends automatically; access revoked
  → CloudTrail records all actions during session
```

### Break-Glass AWS Account

```bash
# Break-glass account: used ONLY when SSO/IdP is unavailable
# Controls:
# 1. Hardware MFA key stored in physical safe (not personal vault)
# 2. Password stored in sealed envelope / physical HSM
# 3. CloudWatch alarm on any use:
aws cloudwatch put-metric-alarm \
  --alarm-name "CRITICAL-break-glass-account-used" \
  --metric-name "BreakGlassUsage" \
  --namespace "SecurityAlerts" \
  --statistic Sum --threshold 1 --period 300 \
  --evaluation-periods 1 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --alarm-actions [critical-sns-arn] \
  --treat-missing-data notBreaching

# CloudTrail metric filter for break-glass usage
aws logs put-metric-filter \
  --log-group-name "/aws/cloudtrail/management-events" \
  --filter-name "BreakGlassLogin" \
  --filter-pattern '{ $.userIdentity.arn = "*break-glass*" }' \
  --metric-transformations metricName=BreakGlassUsage,metricNamespace=SecurityAlerts,metricValue=1
```

---

## 4. Production Database Access

```bash
# Preferred: AWS SSM Session Manager + RDS Proxy (no direct DB port exposure)
# No port 5432/3306 open in security groups; access only via SSM tunnel

# Step 1: Start SSM tunnel to RDS
aws ssm start-session \
  --target i-[bastion-instance] \  # or use SSM port forwarding
  --document-name AWS-StartPortForwardingSessionToRemoteHost \
  --parameters '{"host":["[rds-endpoint]"],"portNumber":["5432"],"localPortNumber":["5432"]}'

# Step 2: Connect via tunnel (on local machine)
psql -h localhost -p 5432 -U [admin-user] -d [database]

# Log the session reason in the audit log
echo "$(date) | $(whoami) | RDS access | Reason: [ticket-123 DB migration] | Duration: 30 min" >> privileged_access_log.md
```

---

## 5. Kubernetes Privileged Access

```bash
# JIT production access via temporary RBAC RoleBinding
# Create time-limited RoleBinding (auto-delete via kubectl)

cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: temp-admin-$(date +%Y%m%d)-[username]
  namespace: production
  annotations:
    expires: "$(date -d '+2 hours' -u +%Y-%m-%dT%H:%M:%SZ)"
    reason: "[Jira ticket / reason]"
    approved-by: "[ISMS Owner]"
subjects:
- kind: User
  name: [user@company.com]
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: admin
  apiGroup: rbac.authorization.k8s.io
EOF

# Revoke after task complete (or use automated TTL controller)
kubectl delete rolebinding temp-admin-[date]-[username] -n production
```

---

## 6. Entra ID Privileged Identity Management (PIM)

Configure via Azure Portal: Entra ID → Identity Governance → Privileged Identity Management

```powershell
# List currently active privileged role assignments
Get-MgIdentityGovernancePrivilegedAccessGroupAssignmentScheduleInstance `
  | Where-Object {$_.Status -eq "Provisioned"} `
  | Select-Object PrincipalId, RoleDefinitionId, StartDateTime, EndDateTime

# Check PIM activation history for Global Admin role
Get-MgIdentityGovernancePrivilegedAccessGroupEligibilityScheduleRequest `
  | Where-Object {$_.Status -eq "Provisioned"} `
  | Select-Object CreatedDateTime, RequestedByDisplayName, Justification
```

**PIM settings for Global Administrator:**
- Activation requires MFA: ✅
- Activation requires justification: ✅
- Maximum activation duration: 4 hours
- Activation requires approval: ✅ (ISMS Owner approves)
- PIM alerts to: `security@company.com`

---

## 7. Privileged Access Log

All privileged access sessions must be logged:

| Date | User | System Accessed | Reason / Ticket | Duration | Approved By | Actions Taken |
|------|------|----------------|-----------------|---------|------------|--------------|
| | | AWS Prod Console | Incident response P1 | 45 min | Auto (on-call) | Rotated credentials; revoked compromised key |
| | | RDS prod-database | DB migration ticket-456 | 30 min | ISMS Owner | Ran migration script v2.1 |
| | | K8s production | Pod restart after deploy issue | 15 min | Tech Lead | Deleted stuck pod |

---

## 8. Monthly Privileged Access Review

Review monthly: who has privileged access, what they used it for, and whether the access is still needed.

```bash
# AWS: List all users/roles with admin policies
aws iam get-account-authorization-details \
  --filter User \
  --query 'UserDetailList[*].{User:UserName, Policies:AttachedManagedPolicies[*].PolicyName}' | \
  python3 -c "import sys,json; [print(u['User'], u['Policies']) for u in json.load(sys.stdin) if u['Policies']]"

# List IAM roles with AdministratorAccess
aws iam list-entities-for-policy \
  --policy-arn arn:aws:iam::aws:policy/AdministratorAccess \
  --query '[Users[].UserName, Roles[].RoleName]'
```

Review questions:
1. Does every person on this list still require this level of access?
2. Have any privileged accounts been inactive > 30 days? (Disable)
3. Are all break-glass accounts still secured? (Test access to verify credentials work — in a controlled manner)
4. Are PIM role assignments still appropriate?
