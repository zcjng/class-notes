---
course: Coursera Google Cybersecurity
topic: Risk
tags:
  - google
  - cybersecurity
---

# 🧮 Security Risk Planning

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

Security risk planning analyzes **three elements**: **assets, threats, and vulnerabilities**.

---

## 🧩 The Three Elements

| Element | Definition | Example |
| :--- | :--- | :--- |
| **Assets** | An item perceived as having **value** to an organization | A cash register and the money inside it |
| **Threats** | Circumstances or events that can **negatively impact** assets | A burglar stealing from the register |
| **Vulnerabilities** | **Weaknesses** that can be exploited by a threat | An unlocked door to a restricted area |

### Threat types
- **Intentional** — a malicious hacker targeting a misconfigured application.
- **Unintentional** — an employee holding the door open for an unknown person.

### Vulnerability categories
- **Technical** — e.g. misconfigured software granting unauthorized access.
- **Human** — e.g. an employee losing their access card in a parking lot.

Either one can lead to risk.

---

## 🎲 What is Risk?

A **risk** is anything that can impact the **confidentiality, integrity, or availability** of an asset. Organizations differ in how they interpret risk because they value different assets.

Two ways to interpret it:
1. Consider the potential **effects** negative events can have on the business.
2. Use the calculation:

$$\text{Likelihood} \times \text{Impact} = \text{Risk}$$

---

## 📊 Risk Matrix Scoring (bank example)

- **Likelihood** — each risk scored **1 (rare), 2 (likely), or 3 (certain)**.
    - Supply-chain attack via natural disaster = **1** (unpredictable).
    - Compromised data events = **2** (likely, given the causes).
- **Severity** — no risk scored below **2**: data breaches (e.g. BEC) can have serious consequences, and regulatory non-compliance could shut down operations.
- **Priority** — a **financial records leak scored 9** (highest): near-certain and crippling. High scores signal the team to **remediate those issues first**.

> [!TIP] Takeaway
> Score risk on **likelihood × impact**, then work the highest scores first — that's prioritization.

---

## 🔗 Related

- [[Risk Management]]
- [[Asset Management & Classification]]
- [[Threat Actors]]
- [[Common Vulnerabilities]]
- [[Security Controls]]
