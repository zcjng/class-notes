---
course: Coursera Google Cybersecurity
topic: Cloud
tags:
  - google
  - cybersecurity
---

# 🔐 Cloud Security

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-18|2026-10-18]] · [[2026-10-23|Related day: 2026-10-23]]

While [[Cloud Computing|cloud computing]] delivers speed, cost savings, and scalability, it introduces distinct architectural and security complexities.

```mermaid
graph TD
    A[Cloud Security Challenges] --> B[Loose IAM Configurations]
    A --> C[Complex Configurations]
    A --> D[Expanded Attack Surface]
    A --> E[Rapid CSP Updates]
```

---

## 🆔 Identity & Access Management (IAM)

- **Definition:** the framework of processes and technology used to manage **digital identities** and authorize resource permissions.

> [!WARNING] The Principle of Least Privilege
> A primary cloud vulnerability is the **loose configuration of user roles**. Over-permissioned roles permit unauthorized actions, dramatically widening an organization's risk profile.

---

## ⚙️ Configuration & Attack Surface

- **Complexity management** — every individual cloud service requires precise configuration to maintain compliance; migration phases are historically prone to **misconfiguration** vulnerabilities.
- **Attack surface dynamics** — deploying numerous applications increases potential entry points for malware.

> [!TIP] Good news on infrastructure
> If designed correctly, multi-service architectures do **not** inherently yield more entry points. CSPs generally enforce **default configurations** that are under much greater security scrutiny than traditional on-premises setups.

---

## 🕒 Zero-Day Attacks & Visibility

- **Zero-day advantages** — CSPs operate at a scale that lets them discover zero-day exploits **before** private enterprises do; they patch hypervisors and migrate active workloads away from threats.
- **The visibility trade-off:**
    - Cloud networks provide deep visibility via **flow logs** and **packet mirroring**.
    - Because CSPs block direct inspection of physical backend servers, they pay for **independent third-party audits** to verify systemic compliance and reveal on-premise integration gaps.

---

## 🤝 The Shared Responsibility Model

An explicit boundary defining where the **provider's** security tasks end and the **customer's** begin.

```
┌───────────────────────────────────────────────────────────┐
│               CUSTOMER: Security IN the Cloud             │
│  (Data, IAM Configurations, Apps, Operating Systems)      │
├───────────────────────────────────────────────────────────┤
│                  CSP: Security OF the Cloud               │
│  (Physical Data Centers, Hypervisors, Host Infrastructure)│
└───────────────────────────────────────────────────────────┘
```

> [!CAUTION] The assumptions trap
> Breaches frequently happen when organizations assume a CSP is securing an asset that actually falls under the **customer's** jurisdiction — e.g. leaving a database publicly exposed due to default setup oversight.

---

## 🚨 Key Cloud Security Challenges

1. **Misconfiguration** — the **primary threat vector**: default "out-of-the-box" settings leaving public read/write permissions enabled (e.g. exposed storage buckets).
2. **Cloud-native breaches** — exploits targeting **misconfigured IAM roles**, API gateways, and cloud service metadata endpoints.
3. **Visibility & access monitoring** — distributed workloads make unified log ingestion and behavioral auditing hard across hybrid/multi-cloud setups.
4. **Regulatory & compliance demands** — auditing cross-border data residency and privacy mandates (**GDPR**, **HIPAA**, **PCI DSS**) in multi-tenant environments.

> [!NOTE] Three states of data
> Protect data **in use** (actively accessed), **in transit** (moving between points), and **at rest** (stored on a device). → [[Data Governance]]

---

## 🛠️ Cloud Security Hardening Framework

A blended approach of administrative governance, physical boundary protection, and data security controls:

- **IAM enforcement** — tighten permission scopes and audit logs.
- **Hypervisor security** — rely on the provider's isolated virtual engine boundaries.
- **Baselining** — establish a trusted state of "normal" configuration and network behavior metrics to identify anomalies early.
- **Cryptography & cryptographic erasure** — encrypt active data pools; use cryptographic erasure (**destroying encryption keys**) to permanently sanitize cloud storage when decommissioning resources.

---

## 🔗 Related

- [[Cloud Computing]]
- [[Logs & SIEM]]
- [[Security Controls]]
- [[Risk Management]]
