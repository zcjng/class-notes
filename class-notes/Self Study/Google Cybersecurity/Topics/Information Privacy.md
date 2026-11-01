---
course: Coursera Google Cybersecurity
topic: Privacy
tags:
  - google
  - cybersecurity
---

# 🕵️ Information Privacy

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

**Information privacy** is the protection against **unauthorized access to and distribution of data**. At its heart it is about **the right to choose** — the ability of individuals and organizations to determine **when, how, and to what extent** their private data is shared.

---

## ⚖️ The Principle of Least Privilege (PoLP)

Users and services get only the **minimum permissions** necessary to do their job, and only for the **duration required**.

> [!EXAMPLE] Airline booking
> A customer service rep may access payment and booking records **temporarily** while helping a passenger — marketing staff have no business need (and no permission) to view credit card details.

---

## 🔏 Privacy vs. Security

| | 🔏 Information privacy | 🔒 Information security (InfoSec) |
| :--- | :--- | :--- |
| **Focus** | Giving people **control** over their personal info and how it's shared | Keeping data in **all states** away from unauthorized users |
| **Concerned with** | Unauthorized access and distribution | Protecting people's choices from threats |

> [!EXAMPLE] Retail example
> A retailer collects age, gender, and location for marketing.
> 1. **Privacy** — disclose how the data will be used *before* collecting; allow **opt-out**.
> 2. **Security** — once consent is given, apply controls against unauthorized access/use/disclosure and respect those who opted out.

> [!WARNING] Key insight
> The more data is collected, stored, and used, the more vulnerable it is to **breaches and threats**.

---

## 📚 Notable Privacy Regulations

| Regulation | Origin | Purpose |
| :--- | :--- | :--- |
| **GDPR** | European Union | Gives data owners **total control** of personal info (name, address, phone, financial, medical) |
| **PCI DSS** | Major financial industry orgs | Secures **credit and debit card** transactions against theft and fraud |
| **HIPAA** | United States law | Protects sensitive **patient health info**; no disclosure without knowledge and consent |

> [!INFO] Scope of GDPR
> GDPR applies to **any business handling data of EU citizens or residents**, wherever it operates — e.g. a US company with EU website visitors.

---

## ✅ Audits vs. Assessments

| | 🔍 Security audit | 📊 Security assessment |
| :--- | :--- | :--- |
| **Definition** | Review of controls, policies, and procedures against a set of expectations | Check of how resilient current security is against threats |
| **Frequency** | ~Once per year | ~Every 3 to 6 months |
| **Performed by** | Internal **and** external third parties | Usually internal employees |
| **Role** | Verifies compliance (e.g. GDPR) | Often preparation for an audit |

> [!EXAMPLE] MFA scenario
> **Regulation:** MFA required on admin accounts → **Audit** checks those accounts → **Assessment** finds many weak passwords → team enables MFA on **all** accounts.

> [!DANGER] Don't ignore results
> Overlooking or delaying fixes from an assessment can lead to **fines or data breaches**.

---

## 🔗 Related

- [[Data Governance]]
- [[Compliance & Regulations]]
- [[Security Ethics]]
- [[Security Audits]]
- [[Security Controls]]
