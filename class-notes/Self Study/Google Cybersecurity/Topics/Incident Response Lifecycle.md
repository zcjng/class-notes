---
course: Coursera Google Cybersecurity
topic: Incident Response
tags:
  - google
  - cybersecurity
---

# 🚨 Incident Response Lifecycle

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-03|Related day: 2026-11-03]]

Incident lifecycle frameworks give organizations a **standardized structure** so incidents are handled effectively and consistently. The **NIST Incident Response Lifecycle** is the reference model.

---

## 📖 Event vs. Incident

| Term | Definition |
| :--- | :--- |
| **Event** | An observable occurrence on a network, system or device |
| **Incident** | An occurrence that actually or imminently jeopardizes — without lawful authority — the confidentiality, integrity or availability of information or an information system; **or** constitutes a violation (or imminent threat) of law, security policies, procedures or acceptable use policies |

> [!IMPORTANT] The one rule to remember
> **All security incidents are events, but not all events are security incidents.**

---

## 🔄 The Four Phases

| # | Phase | What happens |
| :--- | :--- | :--- |
| **1** | **Preparation** | Build the capability before anything happens: plans, staffing, tooling, training |
| **2** | **Detection and Analysis** | Determine whether an incident occurred and assess its scope — see [[Incident Triage]] |
| **3** | **Containment, Eradication and Recovery** | Stop the spread, remove the threat, restore operations — see [[Business Continuity]] |
| **4** | **Post-Incident Activity** | Review, document and improve — see [[Post-Incident Activity]] |

> [!NOTE] Relationship to the NIST CSF
> The five CSF functions (Identify, Protect, Detect, Respond, Recover) are the *strategic* view; the IR lifecycle adds the **operational substeps** for handling a live incident. See [[NIST Cybersecurity Framework]].

---

## 🔎 The Five W's

An incident investigation answers: **who** triggered it, **what** happened, **when**, **where**, and **why**.

These five questions drive the **incident handler's journal** — a form of documentation used throughout response — and the structure of the final report.

---

## 📋 The Incident Response Plan

A document outlining the procedures to take at each step of incident response. Built on the same **policies → standards → procedures** hierarchy as [[Security Plans & Policies]].

Common elements of an IR plan:

- **Incident response procedures** — step-by-step instructions for responding.
- **System information** — network diagrams, data flow diagrams, logging, asset inventory.
- **Supporting documents** — contact lists, forms, templates.

### Documentation types you'll see

**Documentation** is any recorded content used for a specific purpose (audio, digital, handwritten, video). There is no universal industry standard — organizations set their own practices. Common forms:

- **Playbooks** — see [[Playbooks & Incident Response]]
- **Incident handler's journals**
- **Policies and plans**
- **Final reports** — see [[Post-Incident Activity]]

---

## 🛡️ Detection Tooling Overview

| Tool | Role |
| :--- | :--- |
| **IDS** | Detects and alerts on possible intrusions |
| **IPS** | Detects, alerts **and blocks** |
| **EDR** | Monitors endpoints with behavioural analysis and automated response |
| **SIEM** | Collects, aggregates, normalizes and analyses logs centrally |
| **SOAR** | Automates response workflows and case management |

Full detail: [[IDS & IPS]] and [[Logs & SIEM]].

---

## 🔗 Related

- [[CSIRT & SOC Teams]]
- [[Incident Triage]]
- [[Post-Incident Activity]]
- [[Business Continuity]]
- [[Playbooks & Incident Response]]
- [[NIST Cybersecurity Framework]]
- [[Indicators of Compromise]]
