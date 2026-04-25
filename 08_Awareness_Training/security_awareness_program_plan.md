# Security Awareness Program Plan
**ISO 27001:2022 — A.6.3 | Version 1.0**
**Owner:** ISMS Owner / HR
**Review cycle:** Annual

---

## 1. Purpose

This plan defines the annual information security awareness program for all [Organization Name] employees, contractors, and relevant third parties. The program ensures that people understand their security responsibilities and can identify and respond to common threats.

**Regulatory basis:** ISO 27001:2022 A.6.3 (Information security awareness, education and training)

---

## 2. Audience and Segmentation

| Audience | Size (approx.) | Primary Risks | Training Priority |
|----------|---------------|--------------|------------------|
| All employees | [N] | Phishing, social engineering, data handling | Mandatory — all modules |
| Engineering / DevOps | [N] | Secure coding, secrets management, dependency risks | Mandatory + technical track |
| Executive / leadership | [N] | Spear phishing, business email compromise, GDPR decisions | Mandatory + executive track |
| Contractors (long-term) | [N] | Data handling, access control | AUP + GDPR modules mandatory |
| New hires | All new starters | Everything foundational | Day 1 security onboarding |

---

## 3. Training Modules — Annual Program

### Module 1: Information Security Fundamentals (All Staff — Mandatory)

**Delivery:** Video + quiz (30 min) | **Platform:** KnowBe4 / Hoxhunt / internal LMS
**Frequency:** Annual
**Topics:**
- What is information security and why it matters to us
- Our data classification policy (Public / Internal / Confidential / Restricted)
- Password and authentication requirements (MFA, password manager)
- Device security (screen lock, encryption, no personal tools for work data)
- Acceptable use of company systems
- What to do if you suspect a security incident (report to `security@[company.com]`)
- Clean desk / clear screen for remote workers

**Assessment:** 80% pass mark required. Failures: retake within 5 business days.

---

### Module 2: Phishing and Social Engineering (All Staff — Mandatory)

**Delivery:** Video + simulated phishing test | **Frequency:** Semi-annual (plus phishing simulations quarterly)
**Topics:**
- How to recognize phishing emails (sender mismatch, urgency, suspicious links)
- Business Email Compromise (BEC) — fake invoice / wire transfer requests
- Smishing (SMS phishing) and vishing (phone-based attacks)
- Pretexting and social engineering in person
- What to do if you clicked a phishing link (report immediately — no shame)
- How to report suspicious emails (forward to `security@[company.com]` or use report button)

**Simulation:** Quarterly phishing simulation (see `phishing_simulation_plan.md`).
**Failure action:** Employee who clicks simulation link → immediately enrolled in remedial training.

---

### Module 3: Data Protection and GDPR (All Staff — Mandatory)

**Delivery:** Video + quiz (20 min) | **Frequency:** Annual
**Topics:**
- What is personal data and why does it matter
- Our data classification and handling rules (link to policy)
- What you can and cannot do with customer data
- What a personal data breach is — and that you must report it within 1 hour of discovery
- Right of access, erasure — what to do if a customer calls asking
- Data minimisation: collect only what you need
- What happens when we transfer data out of the EEA

**Assessment:** 80% pass mark. Mandatory for all staff handling personal data.

---

### Module 4: Secure Development Practices (Engineering Track — Mandatory for Devs)

**Delivery:** Self-paced course + code review checklist | **Frequency:** Annual
**Topics:**
- OWASP Top 10 — practical examples in our tech stack
- Never hardcode secrets (why, how to use Secrets Manager instead)
- Dependency management and supply chain risks (Dependabot, SCA scanning)
- Secure code review checklist
- How our SAST/DAST pipeline works and what to do with findings
- IaC security (Checkov/tfsec for Terraform)
- Least privilege for service accounts and IAM roles
- Incident reporting for code vulnerabilities (GitHub Security Advisories + internal process)

**Recommended external training:** SANS SEC522, PortSwigger Web Security Academy (free), OWASP WebGoat

---

### Module 5: Executive Security Awareness (Leadership Track)

**Delivery:** Live briefing with ISMS Owner (60 min) | **Frequency:** Annual
**Topics:**
- Threat landscape specific to our industry and size
- Spear phishing and BEC targeting executives — real examples
- Insider threat indicators and what to escalate
- GDPR decision-making responsibilities (breach notification, DPA, DPO)
- Cyber insurance: what's covered, what's excluded, when to call
- Our security posture: current gaps and remediation status
- Board/investor reporting of security incidents

**Format:** No-quiz. Discussion and Q&A format. Document attendance.

---

### Module 6: New Employee Security Onboarding (Day 1 — Mandatory)

See `new_employee_security_onboarding.md` for the full Day 1 checklist.
**Covered separately** — but record completion in the training tracker.

---

## 4. Delivery Calendar

| Month | Activity | Audience | Owner |
|-------|----------|---------|-------|
| January | Annual training refresh launch (Modules 1, 2, 3) | All staff | ISMS Owner |
| February | Phishing simulation #1 | All staff | ISMS Owner |
| March | Developer security track (Module 4) | Engineering | Engineering Lead |
| April | Access review reminder + password hygiene reminder | All staff | IT Lead |
| May | Completion check — chase non-completers | All staff | HR |
| June | Phishing simulation #2 | All staff | ISMS Owner |
| July | Mid-year security update (new threats, policy changes) | All staff | ISMS Owner |
| September | Executive briefing (Module 5) | Leadership | ISMS Owner |
| October | Cybersecurity Awareness Month — themed week (optional) | All staff | ISMS Owner |
| November | Phishing simulation #3 | All staff | ISMS Owner |
| December | Annual completion audit; plan next year's program | All staff | ISMS Owner |

---

## 5. Metrics and KPIs

| Metric | Target | Frequency |
|--------|--------|-----------|
| Training completion rate | ≥95% within 30 days of assignment | Monthly |
| Phishing click rate (simulations) | <10% | Per simulation |
| Phishing report rate | ≥20% of simulated phishing reported | Per simulation |
| Remedial training completion | 100% within 5 days of click | Per simulation |
| Module pass rate (first attempt) | ≥85% | Per module |
| New hire onboarding completion | 100% within first 5 days | Monthly |

Report to ISMS Owner monthly. Include in management review pack (Clause 9.3).

---

## 6. Platform and Tooling

| Tool | Purpose | Notes |
|------|---------|-------|
| KnowBe4 / Hoxhunt | LMS + phishing simulation platform | Preferred — integrates with Entra ID / Google |
| Internal wiki (Notion/Confluence) | Security policy library | Linked from training modules |
| Slack `#security` channel | Ongoing awareness nudges, threat alerts | ISMS Owner posts weekly |
| Email | Assignment notifications, completion reminders | From HR system |
| Google Meet / Zoom | Executive briefings, live sessions | Record for absent participants |

**If budget is constrained:** Free alternatives: Google Forms for quizzes, YouTube for OWASP/security content, KnowBe4 has a free phishing test tool. Document everything for audit evidence even with free tools.

---

## 7. Compliance and Evidence Requirements

For ISO 27001:2022 A.6.3, auditors will ask for:
- [ ] Training records showing who completed what and when (see `training_completion_tracker.md`)
- [ ] Training content (syllabus or module list — screenshots of LMS are fine)
- [ ] Phishing simulation results (click rates, report rates over time)
- [ ] Evidence that new employees are trained before or immediately upon access
- [ ] Policy acknowledgements (AUP, IS Policy) — signed/checked by every employee

**Evidence retention:** 3 years minimum.
