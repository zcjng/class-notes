---
course: Coursera Google Cybersecurity
topic: Application Security
tags:
  - google
  - cybersecurity
---

# 🧠 Threat Modeling

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

**Threat modeling** is a structured, proactive process: identify assets, recognise vulnerabilities, and anticipate how each could be exploited — **before** flaws reach production.

It bridges vulnerability management, threat intelligence and incident response.

---

## 🎯 Core Objective

Rather than reacting to patches after the fact, threat modeling asks you to locate critical assets, profile adversaries, analyse exposure paths and engineer defences up front.

---

## 🔀 Risk Treatment Strategies

Once a risk is modelled, you choose how to handle it:

| Strategy | What it means | Example |
| :--- | :--- | :--- |
| **Avoid** | Redesign or remove the risky feature entirely | Drop a feature that requires storing raw card data |
| **Reduce / mitigate** | Add controls to lower likelihood or impact | Parameterised queries, WAF rules |
| **Transfer** | Offload the risk to a third party | Cyber insurance; outsourcing payments to Stripe |
| **Accept** | Formally acknowledge residual risk where mitigation costs more than the loss | Accept transient caching of non-sensitive metadata |

*(Same framework as [[Risk Management]] — this is where risk decisions meet engineering.)*

---

## 🌳 Scoping Artefacts

| Concept | Purpose |
| :--- | :--- |
| **Asset inventory & classification** | Categorise data, hardware and services by sensitivity |
| **Threat actor profiling** | Catalogue adversaries, motives and capability — from opportunistic script kiddies to organised syndicates |
| **Attack trees** | Hierarchical diagrams mapping explicit paths from entry point to target |

---

## 🕡 The Seven-Stage (PASTA) Lifecycle

1. **Define objectives** — anchor security to business priorities, risk appetite and legal drivers (GDPR, HIPAA, PCI DSS).
2. **Define technical scope** — boundaries plus every asset: clients, endpoints, microservices, databases, third-party dependencies.
3. **Application decomposition** — data flow diagrams (DFDs), **trust boundaries**, and an inventory of existing controls.
4. **Threat analysis** — adopt an attacker mindset; feed in threat intelligence and past incident logs; define likely attack vectors.
5. **Vulnerability & flaw analysis** — SAST/DAST, code review for logic defects, map findings to **CVEs** and CWEs.
6. **Attack modelling & simulation** — build attack trees and test whether found flaws can actually be **chained** under current defences.
7. **Risk & impact analysis** — quantify residual risk, discard false positives, and deliver **prioritised** recommendations.

**Worked example (fitness app):** GPS/biometrics never exposed → BLE + HTTPS/REST scope → DFD from wearable to database → credential-stuffing bots and gym-Wi-Fi sniffing → SQL assembly in `/api/v1/user/workouts?search=` → simulate APK reverse-engineering to SQL injection → critical priority: parameterised queries + WAF rules.

---

## 📚 The Frameworks

| Framework | Core approach | Best suited for |
| :--- | :--- | :--- |
| **STRIDE** | Attack-vector categorisation | Granular, feature-level software threats |
| **PASTA** | Risk-centric, evidence-based simulation (7 stages) | Aligning technical vulnerabilities with business objectives |
| **Trike** | Security- and defence-centric | Access control, permissions, privilege models |
| **VAST** | Visual, agile, automated | Enterprise scale, automated pipelines, fast agile teams |

### STRIDE (Microsoft) — mapped to the CIA triad

| Letter | Threat | Violates |
| :--- | :--- | :--- |
| **S** | Spoofing — pretending to be someone else | Authentication |
| **T** | Tampering — unauthorised modification of code or data | Integrity |
| **R** | Repudiation — denying an action without proof | Non-repudiation |
| **I** | Information disclosure — exposing confidential data | Confidentiality |
| **D** | Denial of service — degrading availability | Availability |
| **E** | Elevation of privilege — gaining unearned authorisation | Authorization |

---

## ❓ The Five Guiding Questions

1. **What are we working on?** — scope, components, data flows
2. **What can go wrong?** — threats and attack vectors
3. **What are we doing about it?** — controls and mitigations
4. **Have we addressed everything?** — coverage and residual risk
5. **Did we do a good job?** — retrospective and process improvement

> [!TIP] You don't need decades of architecture experience
> Value starts with the **attacker mindset** — critically inspecting assumptions about how data is received, processed and stored.

---

## 🔗 Related

- [[Threat Actors]] — the adversaries you model against
- [[Risk Management]]
- [[OWASP Top 10]]
- [[Penetration Testing]]
- [[Defense in Depth]]
- [[Vulnerability Management]]
- [[Risk Management]]
