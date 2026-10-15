---
course: Coursera Google Cybersecurity
topic: Frameworks
tags:
  - google
  - cybersecurity
---

# 🔐 OWASP Security Principles

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-15|Related day: 2026-10-15]]

OWASP — **Open Worldwide Application Security Project®** — security design principles.

---

## 1. Minimizing Attack Surface & Least Privilege

- **Minimize Attack Surface Area** — reduce potential vulnerabilities by disabling unnecessary software features, restricting asset access, and enforcing stronger password policies.
- **Principle of Least Privilege** — grant users only the **minimum access** required for their tasks, limiting potential damage from breaches.

## 2. Defense in Depth & Separation of Duties

- **Defense in Depth** — implement multiple security controls (MFA, firewalls, intrusion detection systems) to create **layered protection**.
- **Separation of Duties** — ensure no single individual has excessive privileges that could be misused, preventing fraudulent activity.

## 3. Keeping Security Simple & Fixing Issues Correctly

- **Keep Security Simple** — avoid overly complex solutions that become unmanageable and hinder collaboration.
- **Fix Security Issues Correctly** — identify the **root cause**, correct vulnerabilities, and test to confirm the repair.

## 4. Establish Secure Defaults

The optimal security state of an application should also be its **default** state; it should take *extra work* to make the application insecure.

## 5. Fail Securely

When a control fails or stops, it should default to its **most secure option**. *Example:* when a firewall fails it should **close all connections** and block all new ones — not accept everything.

## 6. Don't Trust Services

Many organizations work with third-party partners whose security policies may differ. **Do not blindly trust** external services.

---

## 🏗️ Related Design Principles (Security Architecture & Engineering)

Additional InfoSec design principles covered alongside OWASP:

- **Threat modeling**
- **Least privilege**
- **Defense in depth**
- **Fail securely**
- **Separation of duties**
- **Keep it simple**
- **Zero trust**
- **Trust but verify**

> [!INFO] Common InfoSec design processes
> Incident response · vulnerability management · application security · cloud security · infrastructure security.

---

## 🔗 Related

- [[Security Controls]]
- [[CIA Triad]]
- [[CISSP Eight Domains]]
