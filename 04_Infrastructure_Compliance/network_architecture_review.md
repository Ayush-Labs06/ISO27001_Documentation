# Network Architecture Security Review
**ISO 27001:2022 — A.8.20, A.8.21, A.8.22 | Version 1.0**
**Owner:** DevOps Lead

---

## 1. AWS VPC Architecture Requirements

### Recommended Production VPC Layout

```
INTERNET
    │
    ▼
[CloudFront + WAF]
    │
    ▼
[Internet Gateway]
    │
    ▼
┌───────────────────────────────────────────────────┐
│                    VPC (10.0.0.0/16)               │
│                                                   │
│  Public Subnets (10.0.1.0/24, 10.0.2.0/24)       │
│  ┌─────────────────────────────────────────────┐  │
│  │  ALB (Application Load Balancer)            │  │
│  │  NAT Gateway                                │  │
│  │  Bastion / SSM endpoint (if needed)         │  │
│  └─────────────────────────────────────────────┘  │
│                         │                         │
│  Private App Subnets (10.0.10.0/24, 10.0.11.0/24)│
│  ┌─────────────────────────────────────────────┐  │
│  │  ECS Tasks / EKS Nodes                      │  │
│  │  Lambda functions                           │  │
│  └─────────────────────────────────────────────┘  │
│                         │                         │
│  Private Data Subnets (10.0.20.0/24, 10.0.21.0/24│
│  ┌─────────────────────────────────────────────┐  │
│  │  RDS (Multi-AZ)                             │  │
│  │  ElastiCache                                │  │
│  └─────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────┘
    │
    ▼ (via NAT Gateway for outbound only)
INTERNET (package downloads, external API calls)
```

---

## 2. Network Architecture Checklist

### VPC Design
| # | Control | Check | Status | Annex A |
|---|---------|-------|--------|---------|
| 1.1 | Custom VPC used (not default VPC) | `aws ec2 describe-vpcs --query 'Vpcs[?IsDefault]'` should be empty | | A.8.20 |
| 1.2 | Public/private subnet separation | App and data tiers in private subnets | | A.8.22 |
| 1.3 | Multi-AZ deployment (2+ AZs) | Subnets in at least 2 AZs | | A.8.14 |
| 1.4 | NAT Gateway for private subnet outbound | Private subnets route 0.0.0.0/0 to NAT GW | | A.8.20 |
| 1.5 | VPC Flow Logs enabled | `aws ec2 describe-flow-logs` | | A.8.16 |
| 1.6 | DNS resolution enabled in VPC | VPC settings | | A.8.20 |
| 1.7 | VPC endpoints for AWS services (S3, SSM, Secrets Manager) | Reduces internet exposure | | A.8.20 |

```bash
# Create VPC endpoints (private connectivity to AWS services)
# S3 endpoint (gateway type — free)
aws ec2 create-vpc-endpoint \
  --vpc-id [vpc-id] \
  --service-name com.amazonaws.eu-west-1.s3 \
  --route-table-ids [private-rtb-id]

# Secrets Manager endpoint (interface type)
aws ec2 create-vpc-endpoint \
  --vpc-id [vpc-id] \
  --vpc-endpoint-type Interface \
  --service-name com.amazonaws.eu-west-1.secretsmanager \
  --subnet-ids [private-subnet-1] [private-subnet-2] \
  --security-group-ids [endpoint-sg-id] \
  --private-dns-enabled
```

### Security Groups
| # | Control | Check | Status | Annex A |
|---|---------|-------|--------|---------|
| 2.1 | No inbound 0.0.0.0/0 on SSH (22) or RDP (3389) | `aws ec2 describe-security-groups` query | | A.8.20 |
| 2.2 | ALB security group: only 443 and 80 from internet | SG inbound rules | | A.8.20 |
| 2.3 | App tier SG: only from ALB SG (not CIDR) | Referencing SG instead of IP ranges | | A.8.22 |
| 2.4 | Database SG: only from app tier SG on DB port | SG inbound rules | | A.8.22 |
| 2.5 | No outbound "allow all" on database SGs | SG outbound rules | | A.8.22 |
| 2.6 | Security groups have descriptive names and descriptions | SG naming convention | | A.8.9 |
| 2.7 | Unused security groups identified and removed | `aws ec2 describe-security-groups` | | A.8.9 |

```bash
# Find SGs with open SSH from internet
aws ec2 describe-security-groups \
  --filters "Name=ip-permission.from-port,Values=22" \
            "Name=ip-permission.cidr,Values=0.0.0.0/0" \
  --query 'SecurityGroups[*].[GroupId,GroupName,Description]' \
  --output table

# Find security groups not attached to any ENI (candidates for cleanup)
aws ec2 describe-security-groups --query 'SecurityGroups[].GroupId' --output text | tr '\t' '\n' | \
  while read sg; do
    count=$(aws ec2 describe-network-interfaces \
      --filters "Name=group-id,Values=$sg" \
      --query 'length(NetworkInterfaces)' --output text)
    [ "$count" = "0" ] && echo "Unused: $sg"
  done
```

### Web Application Firewall (WAF)
| # | Control | Config | Status | Annex A |
|---|---------|--------|--------|---------|
| 3.1 | WAF attached to CloudFront or ALB | `aws wafv2 list-web-acls` | | A.8.20 |
| 3.2 | AWS Managed Rules Core Rule Set (CRS) enabled | WAF rule groups | | A.8.26 |
| 3.3 | Rate limiting rule: per-IP limits | WAF rate-based rule | | A.8.20 |
| 3.4 | AWS Managed Rules for Known Bad Inputs | WAF rule groups | | A.8.26 |
| 3.5 | WAF logging enabled to S3/CloudWatch | WAF logging config | | A.8.15 |
| 3.6 | IP block list for known malicious IPs | WAF IP set | | A.8.20 |

```bash
# Create WAF Web ACL with managed rules and rate limiting
aws wafv2 create-web-acl \
  --name production-waf \
  --scope CLOUDFRONT \  # or REGIONAL for ALB
  --region us-east-1 \  # CloudFront WAF must be in us-east-1
  --default-action Allow={} \
  --visibility-config SampledRequestsEnabled=true,CloudWatchMetricsEnabled=true,MetricName=production-waf \
  --rules '[
    {
      "Name": "AWSManagedRulesCommonRuleSet",
      "Priority": 1,
      "OverrideAction": {"None": {}},
      "Statement": {
        "ManagedRuleGroupStatement": {
          "VendorName": "AWS",
          "Name": "AWSManagedRulesCommonRuleSet"
        }
      },
      "VisibilityConfig": {"SampledRequestsEnabled": true, "CloudWatchMetricsEnabled": true, "MetricName": "CommonRuleSet"}
    },
    {
      "Name": "RateLimitPerIP",
      "Priority": 10,
      "Action": {"Block": {}},
      "Statement": {
        "RateBasedStatement": {
          "Limit": 1000,
          "AggregateKeyType": "IP"
        }
      },
      "VisibilityConfig": {"SampledRequestsEnabled": true, "CloudWatchMetricsEnabled": true, "MetricName": "RateLimit"}
    }
  ]'
```

---

## 3. DNS Security

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 4.1 | DNSSEC enabled for public hosted zones | Route 53 DNSSEC | | A.8.20 |
| 4.2 | DNS query logging enabled | Route 53 resolver query logging | | A.8.15 |
| 4.3 | DNS filtering for endpoints (malware/C2 blocking) | Route 53 Resolver DNS Firewall / Cloudflare Teams | | A.8.7 |
| 4.4 | SPF, DKIM, DMARC records for all sending domains | DNS TXT records | | A.8.20 |

```bash
# Enable Route 53 Resolver DNS Firewall
aws route53resolver create-firewall-rule-group \
  --name production-dns-firewall

# Add AWS managed domain list (malware)
aws route53resolver create-firewall-rule \
  --firewall-rule-group-id [group-id] \
  --firewall-domain-list-id [aws-managed-malware-list-id] \
  --priority 1 \
  --action BLOCK \
  --block-response NXDOMAIN \
  --name block-malware-domains
```

---

## 4. Zero Trust Considerations

For mature ISMS or post-certification improvement:

| Control | Tool | Benefit |
|---------|------|---------|
| Zero Trust Network Access (ZTNA) | Cloudflare Access / Tailscale / AWS Verified Access | Replace VPN; identity-aware access |
| Microsegmentation | AWS Security Groups + NACLs; Cilium Network Policy | Lateral movement prevention |
| Device trust | Conditional Access + MDM compliance check | Only compliant devices access production |
| Application-layer mTLS | Service mesh (Istio / AWS App Mesh) | Encrypt + authenticate pod-to-pod |

---

## 5. Penetration Test Findings — Network

Track network-related pen test findings here:

| Date | Finding | Severity | Status | Remediation |
|------|---------|---------|--------|------------|
| | | | | |
