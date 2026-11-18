---
course: Coursera Google Cybersecurity
topic: Application Security
tags:
  - google
  - cybersecurity
---

# 🔟 OWASP Top 10

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

**OWASP** (Open Worldwide Application Security Project) is a non-profit foundation improving software security through an open, collaborative platform.

The **OWASP Top 10** has been published since **2003** as an awareness list of the web's most targeted vulnerabilities. It's updated every few years, ranked by how often issues are discovered and the risk they present.

> [!NOTE] Top 10 vs. CVE list
> The Top 10 guides how businesses **design new software**. The CVE list helps identify fixes for **existing** programs. Auditors also use the Top 10 as a compliance reference.

---

## 📋 The Categories

| # | Category | What goes wrong |
| :--- | :--- | :--- |
| 1 | **Broken access control** | Access restrictions aren't enforced — unauthorized disclosure, modification, or destruction; can pivot into other business apps |
| 2 | **Cryptographic failures** | Sensitive data (PII) unprotected or weakly protected — e.g. hashing passwords with **MD5** |
| 3 | **Injection** | Malicious code inserted into a vulnerable app; a login form can become a backdoor to credentials |
| 4 | **Insecure design** | Missing or poorly implemented security controls that should have been built in from the start |
| 5 | **Security misconfiguration** | Settings not properly set or maintained — classic case: deploying servers with **default settings** |
| 6 | **Vulnerable and outdated components** | Unmaintained open-source libraries carrying known CVEs |
| 7 | **Identification and authentication failures** | The app can't correctly recognise who should have access — e.g. a weak router login form |
| 8 | **Software and data integrity failures** | Updates/patches insufficiently reviewed → **supply chain attacks** (SolarWinds, 2020) |
| 9 | **Security logging and monitoring failures** | No record of events like login attempts → nothing to investigate after a breach |
| 10 | **Server-side request forgery (SSRF)** | Attackers manipulate server operations to read/update other resources on that server |

---

## 🧠 How to Use It

- Reference it **during design** so common mistakes get designed out.
- Pair it with the design principles in [[OWASP Security Principles]].
- Treat injection risks with [[Web Exploits & Injection]] — XSS and SQL injection are the concrete mechanics.

---

## 🔗 Related

- [[OWASP Security Principles]]
- [[Web Exploits & Injection]]
- [[Vulnerability Management]]
- [[CICD Security]]
- [[Common Vulnerabilities]]
- [[Security Tools]]
