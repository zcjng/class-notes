---
course: Coursera Google Cybersecurity
topic: Application Security
tags:
  - google
  - cybersecurity
---

# 🔁 CI/CD Security

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

**CI/CD** automates the software release process from code commit to deployment. It's what makes modern teams fast — and it's a high-value target, because **insecure automation automates the introduction of vulnerabilities at scale**.

---

## 🏗️ The Three Stages

| Stage | What it does | Control point |
| :--- | :--- | :--- |
| **Continuous Integration (CI)** | Frequently merges code from many developers into a central location, triggering automated builds and tests | Immediate feedback — integration problems surface the moment they occur |
| **Continuous Delivery (CD)** | After tests pass, code auto-deploys to **staging** or is packaged for release | Typically a **manual approval** gate before production |
| **Continuous Deployment (CD)** | Everything that passes checks goes **straight to production**, no manual approval | Speed and efficiency — security checks must be *inside* the pipeline |

---

## 🛡️ Security Checks Built Into the Pipeline

- **SAST** — static analysis of source code for vulnerabilities, before it runs.
- **DAST** (Dynamic Application Security Testing) — tests a **running** application in a realistic staging environment.
- **SCA** (Software Composition Analysis) — inventories third-party dependencies and flags known CVEs.
- **Security compliance checks** — automated enforcement of organizational rules and policies.
- **Infrastructure security validations** — confirms the hosting systems themselves are secure.

---

## ✅ Why a Secure Pipeline Is Non-Negotiable

- **Secure automation** eliminates manual error — but insecure automation scales mistakes.
- **Better code quality** — automated security tests catch weaknesses before release.
- **Faster security patching** — rapid releases mean rapid delivery of *fixes*, not just features.
- **Collaboration** — dev, security, testing and ops share fast feedback loops (DevSecOps).
- **Lower risk per release** — frequent small releases make problems easier to isolate and limit the blast radius of any single flaw.

---

## 🚨 Common Pipeline Vulnerabilities

| Vulnerability | Risk | Action |
| :--- | :--- | :--- |
| **Insecure dependencies** | Third-party libraries with known CVEs get pulled into builds automatically | Continuously scan and update dependencies (Dependabot, Snyk) |
| **Misconfigured permissions** | Weak access on repos and CI tools lets attackers modify code or configs | Enforce **RBAC** and least privilege |
| **No automated security testing** | Vulnerabilities ship undetected and are far costlier to fix later | Integrate **SAST + DAST** as core pipeline stages |
| **Exposed secrets** | Hardcoded API keys, passwords and tokens in code or settings | Never hardcode — use a vault / secrets manager |
| **Unsecured build environments** | A compromised runner can alter builds or steal data | Harden with secure containers/VMs |

---

## 🔒 Building It Right — DevSecOps

1. **Security from the start** — embed checks in *every* stage, not as a final gate.
2. **Strong access controls** — least privilege, MFA, RBAC on repos and pipelines.
3. **Automate testing everywhere** — SAST, SCA, DAST are not optional extras.
4. **Keep dependencies current** — maintain an inventory and patch CVEs promptly.
5. **Secure secrets management** — HashiCorp Vault, AWS Secrets Manager; rotate regularly.

> [!NOTE] Monitoring the pipeline
> Pipelines need **runtime** monitoring too — see [[Indicators of Compromise]] for CI/CD-specific IoCs like unexpected commits and anomalous deployments.

---

## 🔗 Related

- [[Web Exploits & Injection]]
- [[OWASP Top 10]]
- [[Indicators of Compromise]]
- [[Defense in Depth]]
- [[Vulnerability Management]]
