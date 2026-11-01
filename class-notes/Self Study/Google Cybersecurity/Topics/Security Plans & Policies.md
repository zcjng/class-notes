---
course: Coursera Google Cybersecurity
topic: Governance
tags:
  - google
  - cybersecurity
---

# 📜 Security Plans & Policies

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

> [!QUOTE] Core philosophy
> **Security is all about people, processes, and technology.** It is not just an IT task — it is an organizational culture built on shared values spanning employees, vendors, and customers.

Security plans prepare an organization for risks affecting the **confidentiality, integrity, and availability** of assets. Operationalizing them relies on three hierarchical elements.

```
┌───────────────────────────────────────────────┐
│                   POLICIES                    │  Strategic: What & Why
└───────────────────────┬───────────────────────┘
                        │ informs
┌───────────────────────▼───────────────────────┐
│                  STANDARDS                    │  Tactical: Points of Reference
└───────────────────────┬───────────────────────┘
                        │ dictates
┌───────────────────────▼───────────────────────┐
│                  PROCEDURES                   │  Operational: Step-by-Step
└───────────────────────────────────────────────┘
```

| Component | Nature | Primary role | Example |
| :--- | :--- | :--- | :--- |
| **Policies** | **Strategic** | Foundational rules setting the scope, goals, and boundaries of what to protect and why | Acceptable Use Policy (AUP) for corporate network and hardware usage |
| **Standards** | **Tactical** | Benchmark requirements and objective references used to shape enforceable policies | Minimum 8-character passwords, referencing NIST SP 800-63B |
| **Procedures** | **Operational** | Step-by-step technical instructions for consistent execution | SOP for resetting a locked user account |

---

## ⚖️ Compliance vs. Frameworks

- **Regulations** — **mandatory** rules created by governments or regulators (HIPAA, GDPR, PCI DSS). Failure brings fines, prosecution, and brand damage.
- **Compliance** — the ongoing act of adhering to both internal standards and external regulatory mandates.
- **Frameworks** — **voluntary**, structured collections of guidelines, baselines, and best practices that help organizations manage risk and reach target compliance levels.

> [!TIP] Framework vs. regulation
> **Regulations** are legally mandatory requirements you **must** follow. **Frameworks** (like NIST CSF) are structured blueprints you **choose** to use to meet those legal and business objectives systematically.

---

## 🔗 Related

- [[Security Frameworks]]
- [[Compliance & Regulations]]
- [[NIST Cybersecurity Framework]]
- [[Security Controls]]
- [[Security Audits]]
