# Cryptography Policy
**ISO 27001:2022 — A.5.17, A.8.24 | Version 1.0**
**Owner:** IT Lead / ISMS Owner
**Approved by:** [CTO] | [Date]

---

## 1. Purpose

To define the use of cryptography to protect the confidentiality, integrity, and authenticity of information at [Organization Name], and to ensure cryptographic controls are used consistently and appropriately.

---

## 2. Cryptographic Standards

### 2.1 Encryption — Data in Transit

| Use Case | Minimum Standard | Notes |
|----------|-----------------|-------|
| Web / API traffic (external) | TLS 1.2 minimum; TLS 1.3 preferred | TLS 1.0/1.1 disabled |
| Web / API traffic (internal) | TLS 1.2 minimum | Even internal services use TLS |
| Database connections | TLS required for all connections | `require_secure_transport = ON` |
| Email (external) | TLS (opportunistic); S/MIME for sensitive | Google/M365 enforce TLS |
| SSH sessions | ED25519 or RSA 4096-bit keys; SSH protocol 2 only | Key-based auth; no password SSH to prod |
| VPN | IKEv2 / WireGuard; AES-256 | |

### 2.2 Encryption — Data at Rest

| Asset | Encryption Standard | Implementation |
|-------|--------------------|--------------------|
| AWS RDS databases | AES-256 | AWS KMS; enabled at creation |
| AWS S3 buckets | AES-256 | SSE-S3 default; SSE-KMS for sensitive buckets |
| AWS EBS volumes | AES-256 | KMS; enforced by AWS account policy |
| AWS Backup | AES-256 | KMS CMK |
| Employee laptops | AES-256 | FileVault (macOS); BitLocker (Windows); enforced via MDM |
| Mobile devices | AES-256 | OS-level encryption enforced via MDM |
| Backups | AES-256 | AWS Backup encryption |

### 2.3 Hashing — Passwords and Integrity

| Use Case | Algorithm | Notes |
|----------|----------|-------|
| User passwords | Bcrypt (cost ≥12) / Argon2id | Never store plaintext or MD5/SHA-1 |
| File integrity | SHA-256 minimum | SHA-512 for critical assets |
| Code signing | RSA-4096 or ECDSA P-256 | |
| JWT tokens | RS256 / ES256 | Symmetric (HS256) only for internal use |

### 2.4 Algorithms — Prohibited

The following are explicitly prohibited:
- MD5 (for security purposes)
- SHA-1 (for security purposes)
- DES, 3DES
- RC4
- TLS 1.0, TLS 1.1
- RSA keys < 2048 bits
- Elliptic curves: P-192, any non-NIST/non-Curve25519

---

## 3. Key Management (A.8.24)

### 3.1 Key Types and Storage

| Key Type | Storage | Rotation | Owner |
|----------|---------|---------|-------|
| AWS KMS CMKs | AWS KMS (never exported) | Annual or on compromise | DevOps Lead |
| TLS certificates | AWS Certificate Manager | Auto-renewal (90-day Let's Encrypt or 1-yr commercial) | DevOps Lead |
| SSH keys (personal) | Password manager; private key only on personal device | Annual or on device change | Individual |
| SSH keys (service) | AWS Secrets Manager | Annual | IT Lead |
| API keys (external services) | AWS Secrets Manager / Vault | Annual or on compromise | Service owner |
| Code signing keys | Hardware security module (HSM) or KMS | Annual | DevOps Lead |
| JWT signing keys | Secrets Manager | Every 90 days | Engineering Lead |

### 3.2 Key Lifecycle

```
Generate → Distribute → Use → Store → Rotate → Revoke → Destroy
```

- **Generation:** Use platform-provided key generation (AWS KMS, Let's Encrypt) — never generate cryptographic keys with custom code
- **Distribution:** Keys distributed only through approved channels (Secrets Manager, Vault, KMS); never in plain text, email, or Slack
- **Storage:** No keys in source code, environment variable files committed to Git, or unencrypted configuration files
- **Rotation:** All keys rotated per schedule above; rotation tested before old key retired
- **Revocation:** Immediate revocation on suspected compromise; incident raised
- **Destruction:** Superseded keys deleted from all storage locations; KMS keys scheduled for deletion (minimum 7-day waiting period)

### 3.3 Certificate Management

- All TLS certificates tracked in the certificate register (see `05_Access_Control_Identity/`)
- Expiry alerts configured at 60 days and 30 days
- Auto-renewal configured wherever possible (ACM, Let's Encrypt/ACME)
- Wildcard certificates: allowed but require additional monitoring

### 3.4 AWS KMS Usage

```bash
# List all CMKs and their rotation status
aws kms list-keys | jq '.Keys[].KeyId' | xargs -I {} aws kms get-key-rotation-status --key-id {}

# Enable automatic rotation on a CMK
aws kms enable-key-rotation --key-id [key-id]

# AWS account-level setting to require encryption
aws ec2 enable-ebs-encryption-by-default --region [region]
```

---

## 4. PKI and Certificate Authority

- **Internal CA:** [Not in use / HashiCorp Vault PKI / AWS Private CA] — specify
- **External certificates:** Let's Encrypt (ACME) for public-facing; commercial CA for EV certificates if required
- **Certificate pinning:** Not applied at application level (operational risk); rely on CA trust chain

---

## 5. Developer Guidance

| Scenario | Do | Don't |
|----------|-----|-------|
| Encrypting data in app | Use AWS KMS SDK `encrypt/decrypt` | Roll your own encryption |
| Generating random values | `secrets` module (Python) / `crypto.randomBytes` (Node) | `Math.random()` |
| Storing passwords | bcrypt / Argon2id library | SHA-256 + salt, MD5 |
| API communication | HTTPS with cert validation | `verify=False` / `rejectUnauthorized: false` |
| Storing secrets in app | Secrets Manager SDK at runtime | Hardcode in config, env vars in Docker image |

---

## 6. Compliance

Violation of this policy — particularly storing plaintext passwords, disabling TLS certificate verification, or committing keys to source control — is a serious security incident. Report violations to the ISMS Owner.

| Field | Detail |
|-------|--------|
| Document ID | ISMS-CRYPTO-001 |
| Owner | IT Lead / DevOps Lead |
| Review cycle | Annual |
| Next review | [Date] |
