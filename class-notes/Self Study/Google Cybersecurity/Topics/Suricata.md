---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 🐗 Suricata

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-08|Related day: 2026-11-08]]

**Suricata** is an open-source network analysis and threat detection engine. It can run as a **network- or host-based IDS**, as an **IPS** that blocks malicious traffic, or as a **network security monitoring (NSM)** tool that captures network logs.

It can analyse **live traffic or packet captures**, which makes it useful for incident response, forensics, and testing your own signatures.

---

## 🧩 Anatomy of a Signature

Every rule has three parts, in this order:

| Part | Contents |
| :--- | :--- |
| **Action** | What the IDS does when conditions match — **alert**, **pass**, **drop**, or **reject**. Always the first element |
| **Header** | Traffic attributes: protocol, source and destination IPs, ports, and direction of flow |
| **Rule options** | Extra parameters in parentheses, separated by semicolons — this is where you inspect payload content |

### Key option keywords

| Keyword | Purpose |
| :--- | :--- |
| `msg` | Human-readable alert description |
| `sid` | Unique signature ID |
| `rev` | Signature revision number |
| `flow` | Connection state, e.g. `established,to_server` |
| `content` | Literal bytes/string to match in the packet |

> [!EXAMPLE] A rule in the wild
> ```text
> alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"GET on wire"; flow:established,to_server; content:"GET"; sid:100001; rev:1;)
> ```
> Action `alert` · protocol `http` · internal host to external host, any port · match an established outbound `GET`.

> [!IMPORTANT] Rules must be tuned
> Generic rules produce false positives. Analysts refine headers and content to fit their own network — that's the difference between a useful alert and noise.

---

## 📁 Configuration & Rule Files

- Configuration lives in **`/etc/suricata`**; pre-written rule templates sit in the **`rules`** folder.
- Navigate with `cd`, list with `ls`, read with `less` — lines starting with `#` are **comments** (disabled rules).
- **`suricata.yaml`** defines environment settings and the **rule evaluation order**, which defaults to: **pass → drop → reject → alert**.

---

## 📜 Logs

| File | Contents |
| :--- | :--- |
| **`eve.json`** | **EVE JSON** (Extensible Event Format, JSON) — key-value pairs, metadata-rich, designed for searching and **SIEM ingestion** |
| **`fast.log`** | Basic, legacy alert records |

### Alert vs. telemetry

- **Alert logs** record security-relevant events — a signature fired against suspicious traffic, e.g. potential malware. Key fields: event type (`alert`), IPs involved, protocol, signature message and ID.
- **Network telemetry logs** record ordinary flow activity whether or not anything is suspicious — e.g. an `http` event with the accessed hostname, client user agent, and returned content type.

> [!TIP] Why both matter
> Alerts tell you *something fired*. Telemetry tells you *what else happened around it*. Combining them reconstructs the full sequence of events during an investigation — see [[Network Traffic Analysis]].

---

## 🔗 Related

- [[IDS & IPS]]
- [[Log Management & Formats]]
- [[Logs & SIEM]]
- [[SIEM Queries]]
- [[Packet Analysis]]
- [[Network Traffic Analysis]]
