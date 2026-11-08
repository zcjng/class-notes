---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 🛡️ IDS & IPS

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-03|Related day: 2026-11-03]]

An **intrusion detection system (IDS)** monitors system and network activity and **alerts** on possible intrusions. An **intrusion prevention system (IPS)** does the same but also **acts** to stop them.

The analogy: a home sensor that pings your phone detects an intrusion; a system that automatically drops a steel shutter in response prevents it.

---

## 📊 Capability Comparison

| Capability | IDS | IPS | EDR |
| :--- | :--- | :--- | :--- |
| Detects malicious activity | ✓ | ✓ | ✓ |
| Prevents intrusions | — | ✓ | ✓ |
| Logs activity | ✓ | ✓ | ✓ |
| Generates alerts | ✓ | ✓ | ✓ |
| Performs behavioural analysis | — | — | ✓ |

Many tools do both IDS and IPS: **Snort**, **Zeek**, **Suricata**, **Sagan**, **Kismet**.

---

## 🚨 Four Detection Outcomes

Every alert an IDS produces lands in one of four buckets:

| Outcome | Meaning | Cost |
| :--- | :--- | :--- |
| **True positive** | Alert correctly detects an attack | Correct — investigate |
| **True negative** | No malicious activity, no alert | Correct — silent |
| **False positive** | Alert fires but there was no threat | Wastes team time and resources |
| **False negative** | Malicious activity happens but is **not** detected | **Dangerous** — the team is blind to a real attack |

> [!WARNING] Tune for false negatives last
> High alert volumes usually come from misconfigured rules producing false positives. Hunt deliberately for **false negatives** — they're the ones that hurt.

---

## 🖥️ HIDS vs. NIDS

| Type | Where it sits | What it sees |
| :--- | :--- | :--- |
| **HIDS** (host-based) | Installed as an agent on individual endpoints — laptops, servers | Local traffic, file systems, resource usage, user activity |
| **NIDS** (network-based) | Deployed at specific network points | Traffic across many devices, inbound and outbound, via sensors |

Pairing both creates a **layered defence** — see [[Defense in Depth]].

---

## 🧠 Detection Methods

### Signature-based analysis

Matches activity against known patterns (signatures) of malicious code.

- **Low false-positive rate** for known attacks.
- **Cannot** detect unknown threats, zero-days, or modified code.
- Requires constant signature-database updates.

### Anomaly-based analysis

Establishes a **baseline of normal behaviour** during a training phase, then alerts on deviations.

- **Catches new and evolving threats.**
- **Higher false-positive rate.**
- Fails if an attacker is already present during the training phase — their behaviour becomes "normal".

---

## 📝 Logging

Detected events are written as **IDS logs**, then forwarded to a central repository such as a [[Logs & SIEM|SIEM]] for correlation.

---

## 🔗 Related

- [[Security Tools]]
- [[Network Traffic Analysis]]
- [[Threat Hunting & Intelligence]]
- [[Logs & SIEM]]
- [[Malware Types & Behaviors]]
- [[Incident Triage]]
