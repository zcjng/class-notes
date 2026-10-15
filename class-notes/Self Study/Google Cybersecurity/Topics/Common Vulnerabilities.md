---
course: Coursera Google Cybersecurity
topic: Vulnerabilities
tags:
  - google
  - cybersecurity
---

# 🕳️ Common Vulnerabilities

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-14|Related day: 2026-10-14]]

Named vulnerabilities and vulnerability classes highlighted in the course.

---

## 🧨 Named Vulnerabilities

| Name | Affects | What it does |
| :--- | :--- | :--- |
| **ProxyLogon** | Microsoft Exchange server | A **pre-authenticated** vulnerability — a threat actor can complete the user-authentication process and deploy malicious code remotely. |
| **ZeroLogon** | Microsoft's **Netlogon** authentication protocol | A vulnerability in the service that verifies a user's identity before allowing access to a website's location. |
| **Log4Shell** | Java logging (Log4j) | Lets attackers run Java code on someone else's computer or leak sensitive information; enables a remote attacker to take control of internet-connected devices and run malicious code. |
| **PetitPotam** | Windows **NTLM** (LAN Manager) | A theft technique that lets a LAN-based attacker initiate an authentication request. |

---

## ⚠️ Vulnerability Classes

- **Security logging and monitoring failures** — insufficient logging and monitoring capabilities that let attackers exploit vulnerabilities **without the organization knowing**.
- **Server-side request forgery (SSRF)** — lets attackers manipulate a server-side application into accessing and updating **backend resources**; can let threat actors **steal data**.

---

## 🔗 Related

- [[Threat Actors]]
- [[Security Audits]]
- [[Risk Management]]
