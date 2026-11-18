---
course: Coursera Google Cybersecurity
topic: Threats
tags:
  - google
  - cybersecurity
---

# 🎭 Threat Actors

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-14|Related day: 2026-10-14]]

A **threat actor (TA)** is any person or group who presents a security risk.

---

## 🚪 External vs. Internal

| Type | Who they are | Typical behavior |
| :--- | :--- | :--- |
| **External threat** | Someone outside the organization | Trying to gain unauthorized access |
| **Internal threat** | Current/former employees, external vendors, or trusted partners | *Accidental:* clicking a phishing email or compromised link · *Intentional:* abusing systems or unauthorized access |

---

## 🎯 Advanced Persistent Threats (APTs)

**APTs** have significant expertise in accessing an organization's network **without authorization**. They operate stealthily over long periods.

## 🐍 Insider Threats

**Insider threats** abuse their authorized access to obtain data. Common motives/actions:

- Sabotage
- Corruption
- Espionage
- Unauthorized data access or leaks

---

---

## 🎯 Five Categories by Motivation

| Category | Description |
| :--- | :--- |
| **Competitors** | Rival companies that benefit from leaked information |
| **State actors** | Government intelligence agencies |
| **Criminal syndicates** | Organised groups making money from criminal activity |
| **Insider threats** | Anyone with (or with past) authorised access — accidental or deliberate |
| **Shadow IT** | People using technology that lacks IT governance — e.g. sending work email from a personal account |

---

## 💻 Types of Hackers

A **hacker** is anyone who uses computers to gain unauthorised access to systems, networks or data. Alone, the term says nothing about intent — which is why it's split three ways:

| Type | Intent | Example |
| :--- | :--- | :--- |
| **Unauthorised (malicious)** | Commit crimes | Includes **script kiddies** — low skill, using pre-written code from others |
| **Authorised (ethical)** | Improve security | Internal security teams, external vendors, freelance **bug bounty** hunters |
| **Semi-authorised** | Neither clearly malicious nor sanctioned | **Hacktivists** pursuing a political goal, often exposing risk "for the public good" |

> [!NOTE] APTs
> An **advanced persistent threat** maintains unauthorised access over a long period, mostly associated with nation states. Typically they surveil a target to gather intelligence, then use it to manipulate government, defence, financial and telecom services.

---

## 🚪 Attack Vectors — How They Get In

| Vector | Notes |
| :--- | :--- |
| **Direct access** | Physical access to a system |
| **Removable media** | USB drives and other portable hardware |
| **Social media** | Communication and content-sharing platforms |
| **Email** | Personal and business accounts |
| **Wireless networks** | On-premises Wi-Fi |
| **Cloud services** | Usually third-party hosted |
| **Supply chains** | Third-party vendors acting as a backdoor |

---

## 🧠 The Attacker Mindset

Thinking like an attacker is a **proactive** practice: identify, evaluate and test weaknesses before they're exploited. Applied in four ordered steps:

1. **Identify a target** — the asset, system, person or group to compromise.
2. **Determine access** — what public or internal information could get you to it.
3. **Evaluate attack vectors** — map and test the entry points.
4. **Find tools and methods** — the exploits and techniques to carry it out.

### Simulating threats

- **Proactive (red team)** — exploit vulnerabilities and break through defences.
- **Reactive (blue team)** — defend and respond to an attack.

### Defending the vectors

- **User education** — continuous, event-driven training.
- **Principle of least privilege** — minimum rights, which closes several gaps at once.
- **Security controls and tools** — antivirus, email filters acting as a safety net for human error.
- **Diverse security teams** — varied backgrounds to predict creative attacks.

---

## 🔗 Related

- [[Introduction to Cybersecurity]]
- [[Phishing & Social Engineering]]
- [[Common Vulnerabilities]]
- [[Threat Modeling]]
- [[Network Attacks]]
- [[Indicators of Compromise]]
- [[Penetration Testing]]
