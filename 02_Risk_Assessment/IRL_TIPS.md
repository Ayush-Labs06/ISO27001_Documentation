# IRL Tips — Risk Assessment

---

## 1. The SoA Is the Auditor's First Ask — Get It Right

The Statement of Applicability is the most important document in the ISMS. Auditors will open with it and use it to map everything else. Common SoA failures:

- **Exclusion without rationale:** "Not applicable" alone is rejected. You need: "Excluded because [Organization] has no [context], and this risk is covered by [cloud provider / alternative control]."
- **Included but not implemented:** If you mark a control as "included + implemented" but can't show evidence, that's a finding. Use "Partially implemented" or "Planned" status honestly.
- **Version not controlled:** The SoA must be versioned and dated. Auditors compare previous versions to see what changed and why.

---

## 2. Risk Assessment Is Not a One-Time Exercise

Clients often think risk assessment = fill in the register once, done. Educate them early:
- The register must be updated when there are **significant changes** (new product, new cloud region, new major supplier, security incident)
- Annual full review is the minimum
- Each management review must include risk status
- Auditors will ask: "When was your last risk assessment?" and "What changed since then?"

---

## 3. Asset Register Reality at Startups

The asset register will never be complete — startups move too fast. Practical approach:
- Start with **critical assets only** (production database, cloud accounts, source code, IdP) — these are what the auditor will focus on
- Add a "discovery ongoing" note to the register
- Set up a process to add new assets during provisioning (Terraform resource tags → auto-register)
- Use cloud inventory tools: AWS Config, Azure Resource Graph, cloud asset inventory scripts

For AWS:
```bash
# List all RDS instances
aws rds describe-db-instances --query 'DBInstances[*].[DBInstanceIdentifier,DBInstanceClass,Engine,StorageEncrypted]'

# List all S3 buckets and their encryption status
aws s3api list-buckets | jq '.Buckets[].Name' | xargs -I {} aws s3api get-bucket-encryption --bucket {}
```

---

## 4. Don't Assess Every Asset Against Every Threat

The combinatorial explosion of assets × threats × vulnerabilities is a trap. Practical approach:
- Classify assets: Critical / Important / Standard
- Critical assets get full threat modelling
- Important assets get a simplified assessment (top 5 threats)
- Standard assets get categorized into groups (all employee laptops treated as one risk)

A risk register with 200 rows is not better than one with 30 rows — it's just harder to manage and less likely to be maintained.

---

## 5. Risk Acceptance Threshold Must Be Explicit

The biggest mistake: not defining what "acceptable" means before doing the assessment. Then everything ends up as "treat," including things the business can't afford to treat. Set the threshold first:

- "We accept risks scored Low (1-4) without further treatment"
- "We conditionally accept Medium (5-9) with annual review"
- "We will not accept High or Critical without a treatment plan"

Get this signed off by the CEO before the risk assessment. Then the acceptance decisions are policy-driven, not negotiated case-by-case.

---

## 6. Cloud Shared Responsibility Must Be Mapped in the SoA

AWS and Azure cover significant portions of Annex A. Document it. For example:
- A.7.1 Physical perimeters → "Covered by AWS ISO 27001 certification; shared responsibility model documented"
- A.7.11 Supporting utilities → "Excluded: AWS/Azure responsibility"
- A.8.17 Clock synchronization → "Implemented: AWS NTP service, always-on"

This isn't cutting corners — it's accurate documentation of how cloud-native security works. But you must show the auditor the cloud provider's ISO certificate (publicly available) and your shared responsibility understanding.

---

## 7. The Risk Assessment Is Evidence — Format Matters

Your risk register will be directly reviewed by the CB auditor. It needs to:
- Be clearly dated and versioned
- Show the methodology applied (reference the risk methodology doc)
- Have named risk owners (not just "IT Team")
- Show the treatment decision rationale (not just "treat")
- Include a sign-off from top management accepting the risk treatment plan

A spreadsheet exported to PDF with a management signature satisfies this. A markdown file in a private GitHub repo with a documented review PR also satisfies this. What doesn't satisfy: an informal doc with no dates, no owners, no sign-off.

---

## 8. Likelihood Scoring Calibration

Startups often underscore likelihood because they haven't been breached yet. Push back with industry data:
- Credential stuffing: affects >80% of SaaS platforms (use HIBP data)
- Phishing: ~15% of employees click on well-crafted phishes (Proofpoint data)
- Open S3 buckets: tens of thousands discovered annually
- Supply chain attacks: npm/pip package compromise is routine now (Solar Winds, Log4j downstream effects)

The risk register should reflect the **threat landscape**, not just the client's personal experience.
