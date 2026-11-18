---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 🏹 Threat Hunting & Intelligence

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

Automated tools miss things. Threat hunting and threat intelligence are how humans fill the gap.

---

## 🕵️ Threat Hunting

A **proactive, human-driven search** across a network to discover malicious activity and advanced threats that automated detection tools miss.

Combines:

- Active human analysis
- **Threat intelligence**
- **Indicators of compromise**
- Machine learning

**Why it matters:** it uncovers **evasive** threats — fileless malware, living-off-the-land techniques — that signature-based tooling never sees.

---

## 🧠 Threat Intelligence

Evidence-based context about existing or emerging attacker **tactics, techniques and procedures (TTPs)**.

**Sources:** industry reports, government advisories, data feeds.

**Threat Intelligence Platforms (TIPs)** aggregate, centralize and analyse large volumes of threat data — prioritizing risks and adding context to detections.

> [!NOTE] Information vs. intelligence
> **Information** is raw data or facts. **Intelligence** is the *analysis* of that information to produce insight that supports decisions.
> *Example:* a new OS update ships (information) → research shows new threats are linked to it (analysis) → you decide whether to deploy it now (intelligence).

### OSINT

**Open-source intelligence** — analysis of publicly available information. Used to:

- Provide insight into cyber attacks
- Detect potential data exposures
- Evaluate existing defences
- Identify unknown vulnerabilities

**Common tools:**

| Tool | Use |
| :--- | :--- |
| **VirusTotal** | Analyse suspicious files, domains, URLs and IPs |
| **MITRE ATT&CK** | Knowledge base of adversary tactics and techniques from real-world observation |
| **OSINT Framework** | Interface to find OSINT tools by source/platform |
| **Have I Been Pwned** | Search breached email accounts |

---

## 🍯 Cyber Deception

Deliberate **decoy** strategies that mislead attackers, raise detection rates and let you study attacker behaviour.

- **Honeypots** — decoy systems or files that look legitimate and vulnerable. They alert the team the moment anyone touches them, and waste the attacker's time.
- Because no legitimate user should ever access a honeypot, **any hit is high-signal** — a rare commodity in detection.

---

## 🔁 Hunting in Practice

1. Form a hypothesis from intelligence (e.g. "a new loader is targeting our sector").
2. Search for evidence across logs, telemetry and traffic ([[Logs & SIEM]], [[Network Traffic Analysis]]).
3. Validate findings against [[Indicators of Compromise]].
4. Convert confirmed patterns into new detections and [[Playbooks & Incident Response|playbooks]].

---

## 🔗 Related

- [[Indicators of Compromise]]
- [[IDS & IPS]]
- [[Network Traffic Analysis]]
- [[Logs & SIEM]]
- [[CSIRT & SOC Teams]]
- [[Malware Types & Behaviors]]
