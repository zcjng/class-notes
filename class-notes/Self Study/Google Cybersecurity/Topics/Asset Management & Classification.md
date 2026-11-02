---
course: Coursera Google Cybersecurity
topic: Assets
tags:
  - google
  - cybersecurity
---

# 🗃️ Asset Management & Classification

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

> [!QUOTE] Core philosophy
> *"You can only protect what you know you have."*

**Asset management** is the systematic process of **identifying, tracking, and managing the risks** affecting organizational assets. An accurate inventory is what makes an effective security perimeter possible without creating operational friction.

---

## 🗂️ Key Asset Types

| Category | Description | Examples |
| :--- | :--- | :--- |
| **Digital assets** | Electronic data and records | Customer PII, financial ledgers, transactional logs |
| **Information systems** | Processing environments & infrastructure | Networks, cloud infrastructure, enterprise software |
| **Physical assets** | Tangible property and gear | Data centers, office facilities, laptops, servers |
| **Intangible assets** | Non-physical organizational value | Intellectual property (IP), brand reputation, trade secrets |

---

## 🔍 Asset Classification

**Asset classification** = labeling assets based on their **sensitivity** and **criticality** to the organization.

### The 4 core questions

1. **What do you have?** — identity & description
2. **Where is it?** — physical location, host, or cloud region
3. **Who owns it?** — accountability and governance
4. **How important is it?** — business impact / criticality

> [!TIP] Business benefits
> - **Risk prioritization** — spend high-cost defenses on the most critical targets.
> - **Cost optimization** — don't overspend protecting low-risk assets.
> - **Regulatory compliance** — satisfies audits and legal frameworks (ISO/IEC 27001, SOC 2, HIPAA).

---

## 🏷️ Standard Classification Scheme

```
▲ Restricted      (Need-to-know / highest sensitivity)
│ Confidential    (Significant business damage if leaked)
│ Internal-Only   (Employees & trusted business partners)
▼ Public          (Zero impact if disclosed)
```

| Level | Sensitivity | Access scope | Impact of breach |
| :--- | :--- | :--- | :--- |
| 🔴 **Restricted** | Highest | Strictly need-to-know | Catastrophic damage or extreme legal liability |
| 🟠 **Confidential** | High | Authorized personnel | Significant financial, reputational, or operational harm |
| 🟡 **Internal-only** | Moderate | Employees & authorized partners | Minor operational disruption |
| 🟢 **Public** | Lowest | General public | Negligible |

> [!NOTE] Sector variability
> Commercial naming differs from government schemes. In US DoD frameworks, **Confidential** is often the *lowest* classified tier (Confidential → Secret → Top Secret), not an intermediate one.

---

## ⚠️ Classification Challenges

### A. Ambiguous asset ownership
- Physical infrastructure ownership is clear, but personal devices blur the lines.
- *Example:* a corporate laptop assigned to a remote worker containing both company code and personal photos.

### B. Mixed-classification data (data aggregation)
- Single documents often combine sensitivity levels.
- *Example:* a billing statement — recipient name (public) vs. address/account balance (confidential).

---

## 🔗 Related

- [[Security Risk Planning]]
- [[Data Governance]]
- [[Security Controls]]
- [[Compliance & Regulations]]
