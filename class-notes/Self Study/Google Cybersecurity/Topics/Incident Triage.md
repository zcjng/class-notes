---
course: Coursera Google Cybersecurity
topic: Incident Response
tags:
  - google
  - cybersecurity
---

# 🚑 Incident Triage

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

Triage works like an emergency room: limited resources, unlimited demand, so the **most critical threats get treated first**.

Incidents are prioritised by their potential impact on **confidentiality, integrity and availability** — ransomware outranks a single phishing email every time.

---

## 🔄 The Triage Workflow

| Step | Action |
| :--- | :--- |
| **1. Receive and assess** | Verify whether the incoming alert (e.g. from an IDS) is a genuine threat or a **false positive** — review alert details, history and known vulnerabilities |
| **2. Assign priority** | Rank valid alerts by urgency and impact |
| **3. Collect and analyse** | Gather multi-source evidence, conduct external research, document the process |
| **4. Escalate** | High-severity or complex issues go to L2 analysts or managers with advanced response capability |

### How priority is scored

- **Functional impact** — what systems or services are affected
- **Information impact** — what data is involved and how sensitive it is
- **Recoverability** — how hard it will be to restore

> [!IMPORTANT] Verify before you escalate
> Step 1 exists to stop the queue drowning in noise. Only **verified** alerts receive deep investigation — that's the whole point of triage.

---

## 🧠 Add Context, Don't Assume

Analysts collect supporting evidence — system logs, authentication history — before deciding. Useful critical questions:

- Are there multiple failed login attempts?
- Is the login time or location unusual for this user?
- Has this host triggered alerts before?

Context prevents the two classic mistakes: dismissing a real incident as noise, and escalating noise as an incident.

---

## ✅ Why Triage Helps

- **Resource management** — stops the team burning time on low-priority noise, shortening response time for severe threats.
- **Standardization** — standard playbooks and iterative process produce consistent handling across shifts and analysts.

---

## 🔗 Related

- [[Incident Response Lifecycle]]
- [[CSIRT & SOC Teams]]
- [[IDS & IPS]]
- [[Playbooks & Incident Response]]
- [[Vulnerability Management]] — CVSS uses the same prioritization logic
- [[Indicators of Compromise]]
