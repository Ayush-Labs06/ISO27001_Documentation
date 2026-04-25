# Client Kickoff Discovery Questionnaire
**Send before or use during first engagement meeting**

This questionnaire surfaces the information you need to scope the engagement, estimate effort, identify quick wins, and spot landmines early. Send sections 1-3 in advance; work through 4-6 live.

---

## Section 1 — Business & Context

1. What is the primary reason you're pursuing ISO 27001 certification? (customer requirement, regulation, board mandate, other)
2. Is there a hard deadline? (e.g., "must be certified by Q3 for a contract")
3. Which products/services do you want covered under the certification?
4. Do you have existing certifications? (SOC 2, ISO 9001, Cyber Essentials, PCI-DSS)
5. Have you ever had a security audit, penetration test, or security review? If yes, what were the findings?
6. What's your customer profile? (enterprise, SMB, government, healthcare)
7. Are you subject to any data protection regulations? (GDPR, HIPAA, CCPA, NIS2)

---

## Section 2 — Organizational Structure

8. How many employees total? How many have access to production systems?
9. How many contractors / third-party staff have system access?
10. Is there a dedicated security/IT team? If yes, how many people?
11. Who currently "owns" security decisions? (CTO, Engineering Lead, CEO directly?)
12. Is there a CISO, DPO, or equivalent role? (Even part-time)
13. Are there remote employees? Which countries?
14. Do you have a legal/compliance function, or does that sit with the CEO?

---

## Section 3 — Technical Landscape

### Cloud & Hosting

15. Which cloud providers do you use?
    - [ ] AWS — Account IDs / org structure: _______________
    - [ ] Azure — Subscription IDs: _______________
    - [ ] GCP
    - [ ] On-premises / colocation
    - [ ] Other: _______________

16. How are your cloud environments organized? (single account, multi-account org, landing zone?)
17. Do you use infrastructure-as-code? (Terraform, Pulumi, CDK, CloudFormation)
18. What's your current deployment process? (manual, CI/CD pipeline — which tool?)

### Application Stack

19. What programming languages / frameworks power the product?
20. Where is source code stored? (GitHub, GitLab, Bitbucket)
21. Do you have separate development, staging, and production environments?
22. Does production data ever exist in non-production environments?

### Data & Storage

23. What type of data does your product process? (PII, financial data, health data, B2B SaaS data)
24. What databases / data stores do you use? (RDS, DynamoDB, BigQuery, S3, etc.)
25. Is data encrypted at rest? At transit? Do you know with what algorithm?
26. Where are backups stored? When did you last test a restore?

### Identity & Access

27. How do employees authenticate to internal tools? (SSO, per-app logins, Google/Microsoft federated?)
28. What IdP do you use? (Okta, Azure Entra ID, Google Workspace, JumpCloud)
29. Is MFA enforced everywhere? Or just some systems?
30. How do you manage access to production? (direct, bastion host, SSM, break-glass accounts?)
31. When someone leaves, how is their access revoked? Is there a documented process?

### Endpoints

32. What devices do employees use? (company-issued laptops, BYOD, mix)
33. Is there an MDM solution? (Jamf, Intune, Kandji, none)
34. Is antivirus / EDR deployed on endpoints? Which product?
35. Is full-disk encryption enforced on laptops?

---

## Section 4 — Current Security Posture (Live Discussion)

36. Have you ever had a security incident? (breach, ransomware, data leak, unauthorized access)
    - If yes: What happened? Was it documented? What changed afterwards?
37. Do you have a written incident response procedure?
38. Do you currently do any vulnerability scanning? Penetration testing? When was the last pentest?
39. Do you have centralized logging? Where? How long do you retain logs?
40. Do you have any security monitoring / alerting in place? (SIEM, CloudTrail alerts, GuardDuty)
41. Do you conduct security awareness training? How often? Is it tracked?
42. How do you handle security patches? Is there a defined SLA for critical patches?

---

## Section 5 — Supplier / Third-Party Landscape

43. List your top 10 SaaS/cloud vendors that have access to your data or systems:

| Vendor | What they access | Criticality (H/M/L) |
|--------|-----------------|---------------------|
| | | |

44. Do any suppliers have direct access to your production systems or data?
45. Do you review supplier security practices? (questionnaires, certifications reviewed?)
46. Do your supplier contracts include security requirements / DPA clauses?

---

## Section 6 — Documentation & Process Maturity

47. Do you have a written information security policy? When was it last reviewed?
48. Do you have documented procedures for any security processes (access control, incident response, change management)?
49. Do you have an asset register? (knowing what you have is step 1)
50. Do you have a risk register of any kind?
51. How do you handle change management? (is there an approval process for production changes?)
52. Do you conduct any form of business continuity / DR planning? When did you last test it?

---

## Pre-Engagement Red Flags

Note these if they come up — they affect timeline, cost, and approach:

- [ ] "We don't log anything" — Logging is evidence. No logs = no evidence. Major remediation needed.
- [ ] "We use the same account for dev and prod" — Scope and isolation headache.
- [ ] "Everyone has admin access" — IAM remediation is a project in itself.
- [ ] "We share credentials for most tools" — Shared accounts are a finding in every control domain.
- [ ] "Our last pentest was 3+ years ago" — Need fresh pentest before certification.
- [ ] "We've never done a backup restore test" — DR is unproven; flag this immediately.
- [ ] "We don't really have contracts with our main suppliers" — Supplier management gap.
- [ ] "The CEO is the only person who knows X" — Single point of failure; knowledge management risk.
- [ ] "We're planning a major platform migration" — Scope instability. Recommend post-migration.
- [ ] No MFA on email or production — Immediate quick win but also a serious gap.
