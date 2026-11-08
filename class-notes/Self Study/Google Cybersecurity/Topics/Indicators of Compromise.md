---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 🧩 Indicators of Compromise

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

Forensic clues that tell you an attack happened — or is happening.

---

## ⚖️ IoC vs. IoA

| | **Indicator of Compromise (IoC)** | **Indicator of Attack (IoA)** |
| :--- | :--- | :--- |
| **Evidence type** | Observable forensic artefacts — file hashes, IP addresses | Real-time **behavioural** evidence — e.g. an unknown process opening a network connection |
| **Answers** | **Who** and **what** — an attack that already occurred | **Why** and **how** — an attack in progress |
| **Timing** | After the fact | Live |

---

## 🔺 The Pyramid of Pain

Developed by **David J. Bianco**. It maps indicators to how much **pain** an adversary feels when defenders block them.

| Level | Indicator | Attacker effort to change |
| :--- | :--- | :--- |
| 🔻 Base | **Hash values** | Trivial — one character change |
| | **IP addresses** | Easy |
| | **Domain names** | Moderate |
| | **Network artefacts** | Harder |
| | **Host artefacts** | Harder still |
| 🔺 Peak | **Tools** | Significant — must find or build new tooling |
| 🔺 Peak | **TTPs** | Hardest — must change how they operate |

> [!TIP] The strategic takeaway
> Blocking hashes feels productive but barely inconveniences an attacker. Detecting and blocking **TTPs** inflicts the most lasting disruption — which is why behavioural detection and [[Threat Hunting & Intelligence|threat intelligence]] matter more than blocklists.

---

## 🔬 Investigating IoCs

### Why context matters

A single IoC — an isolated IP address — misses the broader picture. Attackers adapt and evade quickly.

**Threat intelligence** supplies evidence-based context that connects related artefacts into a coherent narrative that informs response.

### Crowdsourcing

The global security community — including **ISACs** (Information Sharing and Analysis Centers) — shares threat data so attackers can't reuse the same tactics across many targets.

### Investigative tools

| Tool | Purpose |
| :--- | :--- |
| **VirusTotal** | Vendor detection verdicts, static details, related artefacts, sandbox behaviour, community notes — for files, URLs and IPs |
| **Jotti's malware scan** | Multi-engine file scanning |
| **Urlscan.io** | URL analysis |
| **MalwareBazaar** | Malware sample repository |

---

## 🚦 CI/CD-Specific IoCs

Pipelines produce their own signals:

- **Code anomalies** — unexpected commits, late-night modifications, unreviewed large deletions.
- **Deployment anomalies** — unexpected pipeline failures, compromised dependencies, attempts to expose hardcoded secrets.
- **Detection sources** — execution/commit logs set the baseline; access and deployment logs track who released what; SIEM rules and continuous vulnerability scanning flag secret exposure and exploitable CVEs.

See [[CICD Security]].

---

## 🔗 Related

- [[Threat Hunting & Intelligence]]
- [[IDS & IPS]]
- [[Network Traffic Analysis]]
- [[CICD Security]]
- [[Hashing & Password Storage]]
- [[Vulnerability Management]]
