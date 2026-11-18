---
course: Coursera Google Cybersecurity
topic: Incident Response
tags:
  - google
  - cybersecurity
---

# 🏗️ Business Continuity

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

When containment ends and eradication begins, the question shifts from *"how do we stop this?"* to *"how do we keep operating?"*

---

## 🔧 Containment, Eradication & Recovery

| Phase | Goal | Typical actions |
| :--- | :--- | :--- |
| **Containment** | Limit and prevent further damage | Execute the IR plan; isolate affected devices — disconnect a compromised computer to stop malware spreading |
| **Eradication** | Remove **all** components and traces of the incident | Run vulnerability tests; patch the identified flaws |
| **Recovery** | Restore systems, services and operations to normal | Reimage systems, reset passwords, update firewall and network configurations |

> [!NOTE] Order matters
> Isolate **before** you investigate (containment), remove the root cause **before** you restore (eradication), and only then bring services back (recovery).

---

## 📋 Business Continuity Planning (BCP)

A **BCP** documents procedures to maintain or quickly restore essential operations during significant disruption — minimizing legal, financial and reputational harm.

> [!IMPORTANT] BCP ≠ DRP
> A **disaster recovery plan** focuses specifically on restoring **information systems** after major technical failures or natural disasters. A **BCP** covers the whole business — people, processes, facilities, and critical operations.

### Why it matters

Severe incidents like ransomware can encrypt critical records and undermine operations across vital infrastructure — healthcare being the obvious example. BCPs reduce service interruption to safeguard essential functions, public safety, and national or economic security.

---

## 🏢 Site Resilience

**Site resilience** keeps infrastructure available through alternate operational locations.

| Site type | Readiness | Trade-off |
| :--- | :--- | :--- |
| **Hot site** | Fully operational duplicate, ready immediately | Highest cost, fastest recovery |
| **Warm site** | Configured environment that can be quickly prepared | Balanced cost and recovery time |
| **Cold site** | Backup facility requiring significant setup to function | Cheapest, slowest |

---

## 🔗 Related

- [[Incident Response Lifecycle]]
- [[Post-Incident Activity]]
- [[Security Plans & Policies]]
- [[Asset Management & Classification]]
- [[Risk Management]]
- [[Common Vulnerabilities]]
