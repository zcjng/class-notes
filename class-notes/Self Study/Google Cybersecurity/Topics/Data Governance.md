---
course: Coursera Google Cybersecurity
topic: Governance
tags:
  - google
  - cybersecurity
---

# 🏛️ Data Governance

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

> [!ABSTRACT] TL;DR
> Data must be protected **at rest, in use, and in transit**. Organizations do this through the **data lifecycle**, **governance policies**, **privacy regulations**, **audits/assessments**, and **cryptography**.

**Data governance** is a set of processes defining how an organization manages information — keeping it **private, accurate, available, and secure** throughout its lifecycle.

---

## 🔄 The Data Lifecycle

```
📥 Collect → 💾 Store → ⚙️ Use → 🗄️ Archive → 🗑️ Destroy
```

Each stage shapes the security controls used to maintain the **CIA triad**. Protecting data at every stage keeps it **accessible and recoverable** when something goes wrong.

---

## 👥 Governance Roles

| Role | Responsibility |
| :--- | :--- |
| 👤 **Data owner** | Decides who can access, edit, use, or destroy their information. Can be an individual or a collective group (e.g. IP owned across departments) |
| 🛡️ **Data custodian** | Responsible for safe handling, transport, maintenance, and storage — can be a person, team, or **automated system** (DBAs, cloud providers, backup systems) |
| 📋 **Data steward** | Maintains and implements the governance policies set by the organization |

> [!TIP] Your role
> As a security professional you'll usually be a **data custodian**: ensuring data isn't damaged, stolen, or misused, and enforcing security and privacy rules.

---

## 🧾 Legally Protected Information

Data owners decide whether their data is shared; regulators define what must be protected **by default**.

| Type | Meaning | Examples / notes |
| :--- | :--- | :--- |
| **PII** | Info used to infer, contact, or locate an individual | Name, address, phone |
| **SPII** | Sensitive PII with **stricter handling**, need-to-know access | Bank account numbers, login credentials |
| **PHI** | Info on past, present, or future physical or mental health | Regulated by **HIPAA** (US) and **GDPR** (EU) |

> [!IMPORTANT] Need-to-know
> SPII should only be accessed on a **need-to-know basis**.

---

## 🔐 Three States of Data

| State | Meaning | Example |
| :--- | :--- | :--- |
| **Data in use** | Actively being accessed by users | Checking email on a laptop |
| **Data in transit** | Moving from one point to another | Sending an email |
| **Data at rest** | Not currently being accessed; stored on a device | A closed laptop |

---

## 🔗 Related

- [[Information Privacy]]
- [[Asset Management & Classification]]
- [[Cryptography]]
- [[Compliance & Regulations]]
- [[Security Controls]]
