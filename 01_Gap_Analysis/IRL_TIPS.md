# IRL Tips — Gap Analysis

---

## 1. The Gap Analysis Sets the Project Budget — Don't Undersell It

The gap report is typically the first thing the client shows their board or CFO to justify the investment. Be honest but frame it constructively:
- "You're at 35% — here's the path to 100%" beats "you're failing in 12 areas"
- Quantify the effort realistically in the roadmap section
- Flag if the timeline is unrealistic given the gaps found

If you sugarcoat the findings to keep the client happy, you'll overpromise and underdeliver on the certification timeline.

---

## 2. "We Do It But Haven't Documented It" Scores as Partial (1), Not Implemented (2)

Auditors need evidence. If a practice isn't documented, it's either:
- Not happening consistently
- Not verifiable

Score it as 1 (partial). Then help the client document it — that's often faster than building something new.

---

## 3. Shadow IT Will Derail Your Gap Analysis

Almost every startup uses SaaS tools that the IT team doesn't know about. During interviews, ask:
- "What tools do you use that aren't on the official approved list?"
- "What do you use for [specific task] when the official tool is slow?"
- Ask developers separately from managers — they're often more candid

Shadow IT = unmanaged access = unmanaged supplier = gap in A.5.9, A.5.19, A.5.23.

---

## 4. Check GitHub / GitLab Settings Yourself

The #1 technical gap I find in startups is not in the cloud console — it's in the source code host:
- Are secrets exposed in the repo history? (`git log --all -S "SECRET"` or use `trufflehog`)
- Is branch protection enabled on main?
- Are outside collaborators (contractors) still active from old projects?
- Is 2FA enforced at the org level?

Get a read-only admin invite to their GitHub org and run through the org settings. Takes 30 minutes and surfaces real findings every time.

---

## 5. IAM Credential Report is Your Best Friend

For AWS accounts, generate the IAM credential report immediately:

```bash
aws iam generate-credential-report
aws iam get-credential-report --output text --query Content | base64 --decode
```

This shows every IAM user, when they last logged in, whether MFA is enabled, whether they have active access keys, and when keys were last rotated. Export to CSV. This alone will fill your A.8.2, A.8.5, and A.5.18 findings.

---

## 6. The "We're a Startup, We Move Fast" Defense

Clients sometimes push back on controls with "we're a startup, we can't afford to slow down." Acknowledge it, then reframe:
- "ISO 27001 doesn't require you to slow down — it requires you to know what you're doing and why"
- Change management policy ≠ change approval committee for every line change. It means having a defined process, even if it's "deploy with a PR + 1 reviewer."
- Access control ≠ everyone needs read-only. It means "least privilege + documented exceptions + review cycle."

---

## 7. Physical Controls Gap Analysis for Remote-First Startups

If the startup is fully remote, A.7 physical controls become interesting:
- A.7.1-7.4 (physical perimeters, entry, monitoring): covered by cloud provider for servers; for home offices, covered by AUP and A.6.7 (remote working) — document this explicitly in the SoA
- A.7.7 (clear desk/screen): enforce via MDM screen lock policy — this satisfies the intent
- A.7.14 (secure disposal): need an actual procedure — usually covered by IT asset management

Don't exclude A.7 controls entirely because "we're cloud." The physical office still exists. Employee home offices still exist. Document how you address them.

---

## 8. Annex A Controls that Always Have Gaps at Startups

Based on real engagements, these are the most common gaps found at startups regardless of their technical maturity:

| Control | Why Always Gaps |
|---------|----------------|
| A.5.9 Asset inventory | Startups grow fast; inventory is always behind |
| A.5.19 Supplier security | No one has time to questionnaire 30 SaaS vendors |
| A.5.24-5.27 Incident management | No IRP, no testing, no post-mortems |
| A.6.3 Training | Informal at best, no records |
| A.8.8 Vulnerability management | Devs know about Trivy, but no formal programme |
| A.8.9 Configuration management | IaC exists but no drift detection |
| A.8.15 Logging | CloudTrail not centralized; logs expire after 90 days |
| A.8.25-8.28 Secure SDLC | No SAST/SCA, no security requirements in stories |

---

## 9. Score Conservatively — Auditors Will Check

Resist the urge to score things generously to make the report look better. If the control is informal, score 1. If it's implemented but has no records, score 2. If you score something as 3 (managed) and the auditor can't find evidence of regular review, that becomes a finding — and it reflects poorly on your assessment.

---

## 10. Deliver the Report in a Meeting, Not by Email

Don't just send a PDF. Walk the CTO/CEO through the findings in a call:
- They'll push back on some scores — that's valuable; you might be wrong
- Some findings will surprise them and need immediate escalation
- The remediation roadmap needs buy-in, not just acknowledgement
- You establish yourself as the expert guiding the project, not just a report-writer
