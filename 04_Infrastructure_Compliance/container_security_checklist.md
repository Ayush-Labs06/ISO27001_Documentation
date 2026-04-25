# Container Security Checklist (Docker / Kubernetes)
**ISO 27001:2022 — A.8.9, A.8.2, A.8.8, A.8.20, A.8.22 | Version 1.0**
**Owner:** DevOps Lead / Engineering Lead

---

## 1. Docker Image Security

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 1.1 | Use minimal base images (distroless, alpine, slim variants) | `FROM gcr.io/distroless/python3` | | A.8.9 |
| 1.2 | Images pinned by digest, not just tag | `FROM alpine@sha256:abcdef...` | | A.8.9 |
| 1.3 | No secrets in Dockerfile or image layers | `docker history --no-trunc [image]` | | A.5.17 |
| 1.4 | Images scanned for CVEs before push (Trivy in CI) | See CI pipeline | | A.8.8 |
| 1.5 | Images signed (Docker Content Trust / cosign) | `DOCKER_CONTENT_TRUST=1` | | A.8.9 |
| 1.6 | No unnecessary packages in image | `apt-get clean && rm -rf /var/lib/apt/lists/*` in Dockerfile | | A.8.9 |
| 1.7 | Multi-stage builds to exclude build tools from production image | Dockerfile uses multi-stage | | A.8.9 |
| 1.8 | Non-root user defined in Dockerfile | `USER appuser` in Dockerfile | | A.8.2 |
| 1.9 | COPY preferred over ADD; no URLs in ADD | Dockerfile review | | A.8.9 |
| 1.10 | Private ECR/registry used; no public Docker Hub images without review | Registry policy | | A.5.21 |

```dockerfile
# Secure Dockerfile example
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM gcr.io/distroless/python3-debian12 AS production
WORKDIR /app
COPY --from=builder /app /app
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
USER nonroot:nonroot
EXPOSE 8080
CMD ["app.py"]
```

```bash
# Scan image with Trivy
trivy image --severity HIGH,CRITICAL --exit-code 1 [image:tag]

# Sign image with cosign
cosign sign --key cosign.key [image@digest]

# Verify image signature
cosign verify --key cosign.pub [image@digest]
```

---

## 2. Container Runtime Security

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 2.1 | Containers run as non-root (securityContext.runAsNonRoot: true) | K8s pod spec | | A.8.2 |
| 2.2 | Root filesystem read-only (readOnlyRootFilesystem: true) | K8s pod spec | | A.8.9 |
| 2.3 | Privilege escalation disabled (allowPrivilegeEscalation: false) | K8s pod spec | | A.8.2 |
| 2.4 | Privileged containers prohibited | K8s pod spec; admission controller | | A.8.2 |
| 2.5 | Capabilities dropped to minimum (drop ALL; add only required) | `securityContext.capabilities.drop: ["ALL"]` | | A.8.2 |
| 2.6 | seccomp profile applied (RuntimeDefault or custom) | `securityContext.seccompProfile.type: RuntimeDefault` | | A.8.9 |
| 2.7 | AppArmor or SELinux profile active | Cluster-level policy | | A.8.9 |
| 2.8 | Resource limits set (CPU and memory) | `resources.limits.cpu`, `resources.limits.memory` | | A.8.6 |
| 2.9 | Health checks configured (liveness + readiness probes) | K8s deployment spec | | A.8.6 |
| 2.10 | No hostPID, hostIPC, hostNetwork unless explicitly justified | Pod spec review | | A.8.22 |

```yaml
# Secure pod security context
apiVersion: v1
kind: Pod
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: app
    image: myapp@sha256:abcdef...
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      capabilities:
        drop: ["ALL"]
    resources:
      limits:
        cpu: "500m"
        memory: "256Mi"
      requests:
        cpu: "250m"
        memory: "128Mi"
```

---

## 3. Kubernetes Cluster Security

### Access Control (RBAC)
| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 3.1 | RBAC enabled (default in modern K8s) | `kubectl api-versions | grep rbac` | | A.8.2 |
| 3.2 | No ClusterRoleBindings with wildcards (*) for verbs or resources | `kubectl get clusterrolebindings -o json | jq '...'` | | A.8.2 |
| 3.3 | ServiceAccount tokens not auto-mounted unless needed | `automountServiceAccountToken: false` in pod spec | | A.8.2 |
| 3.4 | Default ServiceAccount has no permissions | `kubectl describe clusterrolebinding` | | A.8.2 |
| 3.5 | Developers have namespace-scoped access only (no cluster-admin) | RBAC role review | | A.8.2 |
| 3.6 | Production namespace access requires justification (JIT or approval) | RBAC + PAM procedure | | A.8.2 |

```bash
# List all ClusterRoleBindings with cluster-admin
kubectl get clusterrolebindings -o json | \
  jq '.items[] | select(.roleRef.name == "cluster-admin") | .metadata.name, .subjects[]'

# Find RoleBindings that allow * verbs
kubectl get rolebindings,clusterrolebindings -A -o json | \
  jq '.items[] | select(.roleRef | .apiGroup=="rbac.authorization.k8s.io") |
  select(.metadata.name) | .metadata.name'
```

### API Server Security (EKS)
| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 4.1 | EKS API server endpoint not publicly accessible (or restricted to CIDR) | EKS cluster config | | A.8.20 |
| 4.2 | EKS uses Entra ID / OIDC for authentication | EKS identity provider | | A.8.5 |
| 4.3 | EKS control plane logs enabled (API, audit, authenticator) | EKS logging config | | A.8.15 |
| 4.4 | EKS node groups use IMDSv2 | Launch template | | A.8.5 |
| 4.5 | EKS nodes in private subnets only | VPC config | | A.8.20 |
| 4.6 | EKS version up-to-date (within 2 minor versions of latest) | `aws eks describe-cluster` | | A.8.8 |

```bash
# Check EKS cluster endpoint access
aws eks describe-cluster --name [cluster-name] \
  --query 'cluster.resourcesVpcConfig.{Public:endpointPublicAccess,Private:endpointPrivateAccess,CIDR:publicAccessCidrs}'

# Enable EKS control plane logging
aws eks update-cluster-config --name [cluster-name] \
  --logging '{"clusterLogging":[{"types":["api","audit","authenticator","controllerManager","scheduler"],"enabled":true}]}'
```

### Network Policies
| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 5.1 | Default deny NetworkPolicy applied to all namespaces | `kubectl apply -f default-deny.yaml` | | A.8.22 |
| 5.2 | Explicit allow rules for required pod-to-pod communication | NetworkPolicy specs | | A.8.22 |
| 5.3 | Egress restricted: pods cannot access metadata service (169.254.169.254) | NetworkPolicy egress rule | | A.8.22 |
| 5.4 | CNI supports NetworkPolicy (Calico, Cilium, Weave) | EKS CNI config | | A.8.22 |

```yaml
# Default deny all ingress and egress
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
  egress:
  - ports:
    - port: 53
      protocol: UDP  # Allow DNS only
```

---

## 4. Secrets Management in Containers

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 6.1 | No secrets in environment variables in plain text | Secret objects; Secrets Manager | | A.5.17 |
| 6.2 | Kubernetes Secrets encrypted at rest (etcd encryption) | EKS envelope encryption with KMS | | A.8.24 |
| 6.3 | AWS Secrets Manager / Vault preferred over K8s Secrets | External Secrets Operator or CSI driver | | A.5.17 |
| 6.4 | Secrets rotation automated | Secrets Manager rotation; no manual rotation | | A.5.17 |
| 6.5 | No secrets in ConfigMaps | ConfigMap review | | A.5.17 |

```bash
# Enable EKS secret encryption with KMS
aws eks create-cluster \
  --name [cluster-name] \
  --encryption-config '[{"resources":["secrets"],"provider":{"keyArn":"[kms-key-arn]"}}]'

# External Secrets Operator (ESO) to sync from AWS Secrets Manager
kubectl apply -f https://raw.githubusercontent.com/external-secrets/external-secrets/main/deploy/crds/bundle.yaml
```

---

## 5. Image Registry Security

| # | Control | Implementation | Status | Annex A |
|---|---------|---------------|--------|---------|
| 7.1 | Private ECR repository with image scanning enabled | ECR settings | | A.8.8 |
| 7.2 | ECR lifecycle policy: remove untagged images >30 days | ECR lifecycle rules | | A.8.10 |
| 7.3 | ECR repository policy: restrict push to CI/CD role only | ECR resource policy | | A.8.3 |
| 7.4 | Admission controller blocks images not from approved registries | OPA Gatekeeper / Kyverno policy | | A.8.19 |

```bash
# ECR scan results for critical/high
aws ecr describe-image-scan-findings \
  --repository-name [repo] \
  --image-id imageTag=latest \
  --query 'imageScanFindings.findings[?severity==`CRITICAL` || severity==`HIGH`].[name,severity,uri]' \
  --output table
```
