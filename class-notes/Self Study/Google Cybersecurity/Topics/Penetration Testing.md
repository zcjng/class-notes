---
course: Coursera Google Cybersecurity
topic: Vulnerabilities
tags:
  - google
  - cybersecurity
---

# 🥷 Penetration Testing

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

A **penetration test** (pen test) is a **simulated attack** that identifies vulnerabilities in systems, networks, websites, applications, and processes — using the same tools and techniques as a real attacker.

Because it is **authorized**, it is a form of **ethical hacking**. Where a [[Vulnerability Assessment|vulnerability assessment]] *finds* weaknesses, a pen test *exploits* them.

> [!IMPORTANT] Compliance
> Organizations regulated by **PCI DSS, HIPAA, or GDPR** must routinely perform penetration testing to stay compliant.

---

## 👥 Red, Blue & Purple Teams

| Team | Role |
| :--- | :--- |
| **Red team** | Simulates attacks to identify vulnerabilities |
| **Blue team** | Focuses on defence and incident response; validates existing controls |
| **Purple team** | Collaborative — combines red and blue to improve overall security posture |

---

## 📖 Three Testing Strategies

| Strategy | Knowledge level | Also known as |
| :--- | :--- | :--- |
| **Open-box** | Full privileged access — architecture, data flow, network diagrams | Internal · full knowledge · white-box · clear-box |
| **Closed-box** | Little to no internal access — like a real outside hacker | External · black-box · zero knowledge |
| **Partial knowledge** | Limited access, e.g. the visibility a customer service rep has | Gray-box |

---

## 💰 Bug Bounty Programs

Organizations offer freelance testers **financial rewards** for finding and reporting vulnerabilities in their products. Great entry point for amateurs building skills and reputation.

> [!TIP] HackerOne is a community of ethical hackers where active bug bounties are listed.

---

## 🧭 Where It Fits

Pen testing is the **validation** step in the vulnerability management cycle — proof that a discovered weakness is actually exploitable under current defences. See also [[Threat Modeling]] for the modelling that tells you *what* to test.

---

## 🔗 Related

- [[Vulnerability Management]]
- [[Vulnerability Assessment]]
- [[Threat Modeling]]
- [[Web Exploits & Injection]]
- [[Security Tools]]
- [[Security Audits]]
