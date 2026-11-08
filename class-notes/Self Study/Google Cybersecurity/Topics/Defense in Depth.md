---
course: Coursera Google Cybersecurity
topic: Security Architecture
tags:
  - google
  - cybersecurity
---

# 🏰 Defense in Depth

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

**Defense in depth** is a multi-layered security approach: place **redundant protective controls** around critical assets so that if one layer fails, the next one still holds.

---

## 🗝️ The Castle Approach

Medieval castles didn't rely on a single wall. Moats, outer walls, inner walls and watchtowers each added a barrier — and each had its **own unique controls**.

The security translation: every layer should use *different* controls, detect *different* weaknesses, and still stop the attack when a neighbouring layer is breached.

---

## 🧱 The Five Layers

| Layer | What it covers | Example controls |
| :--- | :--- | :--- |
| **Perimeter** | The outer boundary | Authentication of external users (username/password, badges at doors) |
| **Network** | Traffic between systems | Firewalls, authorization rules, segmentation |
| **Endpoint** | Network-connected devices | Antivirus, EDR, device hardening |
| **Application** | The interfaces users touch | MFA, input validation, secure design |
| **Data** | The information itself | Asset classification, encryption, access controls |

> [!TIP] Attacker's-eye view
> Each of these five layers is also an **attack surface**. See [[Threat Actors]] and [[Packet Analysis]] for how each gets probed.

---

## 🎯 Attack Surface & Hardening

- **Physical attack surface** — people and physical devices. Vulnerable to external threats (a competitor reading an unattended laptop) and internal ones (a disgruntled employee leaking data).
- **Digital attack surface** — everything connected online. Cloud computing exploded this from one tightly-managed office network into countless global entry points.
- **Security hardening** — deliberately shrinking the attack surface by reducing vulnerabilities and eliminating unnecessary entry points. Physical hardening uses policy and strict access controls.

---

## ⚠️ Why Layers Beat Single Controls

- No single control stops every attack.
- Redundancy means an attacker must defeat multiple *different* mechanisms.
- Each layer provides its own **detection** opportunity, not just prevention.

---

## 🔗 Related

- [[OWASP Security Principles]]
- [[Security Controls]]
- [[Firewalls]]
- [[Network Segmentation]]
- [[IDS & IPS]]
- [[Threat Modeling]]
