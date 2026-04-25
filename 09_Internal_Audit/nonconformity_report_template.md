# Nonconformity Report (NCR)
**ISO 27001:2022 — Clause 10.1 | Version 1.0**
**Use one form per nonconformity. File in: `Records/Audits/NCR-[YYYY]-[NNN].md`**

---

## Part A — Nonconformity Identification

| Field | Value |
|-------|-------|
| **NCR Number** | NCR-[YYYY]-[NNN] |
| **Audit Reference** | [Audit ID / date] |
| **Date Raised** | YYYY-MM-DD |
| **Raised By (Auditor)** | [Name] |
| **Process / Area Audited** | [e.g., Access Control / Incident Response / Supplier Management] |
| **Grade** | ☐ Major NC  ☐ Minor NC  ☐ Observation |

### ISO 27001:2022 Reference

| Field | Value |
|-------|-------|
| **Clause / Annex A Control** | [e.g., A.5.15 — Access Control] |
| **Requirement Text (verbatim)** | [Copy the relevant sentence from the standard] |

---

## Part B — Description of Nonconformity

**What was observed:**
> [Factual description of what was found. Include evidence reference — e.g., "IAM credential report dated YYYY-MM-DD shows 3 users have not enabled MFA."]

**Evidence reviewed:**
- [Document / record / system checked]
- [Sampling: e.g., "10 of 47 user accounts reviewed; 3 found non-compliant"]

**Objective evidence (attach or reference):**
> [Screenshot reference / log file / record ID]

---

## Part C — Root Cause Analysis

*(To be completed by the process owner within 10 business days of NCR issue)*

**Root cause:**
> [Not the symptom — the underlying reason this nonconformity exists. Use 5-Why if needed.]

**5-Why Analysis (if Major NC):**
1. Why did the nonconformity occur? ___
2. Why did [answer 1] happen? ___
3. Why did [answer 2] happen? ___
4. Why did [answer 3] happen? ___
5. Why did [answer 4] happen? ___

**Root cause conclusion:** ___

**Process owner:** [Name] | **Completed:** YYYY-MM-DD

---

## Part D — Corrective Action Plan

*(Agreed between process owner and ISMS Owner within 15 business days)*

| Action | Owner | Due Date | Evidence of Completion |
|--------|-------|---------|----------------------|
| Immediate fix: [specific action] | [Name] | YYYY-MM-DD | [What will prove it's done] |
| Systemic fix: [process/control change] | [Name] | YYYY-MM-DD | [Updated procedure / policy version] |
| Verification: [how effectiveness will be confirmed] | Internal Auditor | YYYY-MM-DD | |

**Agreed by (ISMS Owner):** ___________________ **Date:** ___________________

---

## Part E — Closure

*(To be completed by Internal Auditor after verifying corrective actions)*

**Verification date:** YYYY-MM-DD
**Evidence of completion reviewed:**
- [ ] Action 1 verified: [notes]
- [ ] Action 2 verified: [notes]
- [ ] Effectiveness confirmed: [notes — did the fix actually resolve the root cause?]

**Closure decision:**
- [ ] **Closed** — corrective actions complete and effective
- [ ] **Not closed** — actions incomplete or ineffective; follow-up date: [date]

**Closed by (Auditor):** ___________________ **Date:** ___________________
**Reviewed by (ISMS Owner):** ___________________ **Date:** ___________________

---

## Example NCRs for Reference

### Example 1 — Minor NC

**Area:** Access Control
**Clause:** A.5.18 (Access rights) / Clause 9.1
**Observed:** Quarterly access review for the production AWS environment was not completed in Q3 [year]. The access review procedure requires completion by the last day of each quarter. When requested, no record of Q3 review was provided.
**Evidence:** Access review tracker (last entry: Q2 [year]); confirmed by IT Lead in interview.
**Grade:** Minor NC (review occurred in prior quarters; isolated lapse)

---

### Example 2 — Major NC

**Area:** Cryptography
**Clause:** A.8.24 (Use of cryptography)
**Observed:** Review of the production RDS PostgreSQL instance (`prod-db-[id]`) shows storage encryption is disabled. The cryptography policy requires all data stores containing personal data to be encrypted at rest.
```bash
aws rds describe-db-instances --db-instance-identifier prod-db \
  --query 'DBInstances[0].StorageEncrypted'
# Returns: false
```
**Evidence:** AWS CLI output (see attached); customer PII confirmed in this database per asset register.
**Grade:** Major NC (clear policy requirement; personal data unencrypted)

---

### Example 3 — Observation

**Area:** Incident Response
**Observation:** The incident response plan references the ISMS Owner as the primary contact for P1/P2 incidents, but no backup contact is specified. During the tabletop exercise, the ISMS Owner was unavailable (simulated), and the team was uncertain who should take the Incident Commander role.
**Grade:** Observation (backup not required by the standard but is a material risk)
**Recommendation:** Define a formal backup Incident Commander in the IRP.
