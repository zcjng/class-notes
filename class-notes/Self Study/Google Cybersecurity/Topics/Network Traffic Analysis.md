---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 📈 Network Traffic Analysis

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-03|Related day: 2026-11-03]]

**Network traffic** is the amount of data moving across a network; **network data** is the data transmitted between devices. Analysts need to know what *normal* looks like before they can spot *abnormal*.

---

## 📊 Baselines — Establishing "Normal"

A **baseline** is a standard reference point representing expected, legitimate behaviour across systems, devices and traffic flows. It enables anomaly detection by highlighting deviations.

**Data points analysed:**

- Source and destination IP addresses
- Data transfer volume
- Timestamps and transmission frequency
- Standard port/protocol combinations

---

## 🔍 The Three Monitoring Vectors

### 1. Flow analysis

Examines packet volume, protocols and port numbers.

- **Protocol-to-port alignment** — verify protocols use their expected ports (HTTPS on 443).
- **C2 detection** — attackers hide malicious traffic on unexpected ports (e.g. tunnelling HTTPS over 8088) to talk to compromised internal hosts.

### 2. Packet payload

Examines the actual message body inside the packet.

- **Data inspection** — inspect content, decrypting where legally and technically feasible.
- **Exfiltration detection** — spot sensitive files or database dumps moving outbound to external addresses.

### 3. Temporal patterns

Monitors traffic frequency against time windows.

- **Expected window** — normal operating hours (e.g. 09:00–17:00 local).
- **Deviation** — unscheduled off-hours spikes (e.g. a 02:00 transfer surge) signalling unauthorised access, automated scripts, or an ongoing breach.

---

## 🕵️ Detecting Data Exfiltration

**Data exfiltration** is the unauthorised transfer of data — credentials, intellectual property — out of a system.

1. **Initial access** via phishing and similar tactics.
2. **Lateral movement (pivoting)** to explore the network and locate valuable assets.
3. **Packaging** the target data — compressed or bundled to slip past controls.
4. **Exfiltration** through channels such as compromised email accounts.

**Detection signals:** large internal transfers, external uploads, unexpected file writes, multiple external logins from unfamiliar IPs.

**Response:** investigate the alert, then contain — e.g. block malicious IPs with firewall rules.

> [!TIP] Prevention stack
> MFA and a complete **asset inventory** close the front door; monitoring catches what gets through. See [[Access Control & AAA]] and [[Asset Management & Classification]].

---

## 🆚 SOC vs. NOC

| Dimension | **SOC** | **NOC** |
| :--- | :--- | :--- |
| **Primary mission** | Security, threat detection, incident response | Network stability, performance, availability |
| **Key focus** | Indicators of compromise, intrusion | Outages, latency, packet loss, uptime |
| **Mindset** | Adversarial / defensive | Operational / reliability |

---

## 🧰 Monitoring Toolset

| Tool | Purpose |
| :--- | :--- |
| **IDS** | Automated alerts — continuous scanning against signatures or baseline deviations; flags malware signatures, exploit payloads, phishing traffic |
| **Protocol analyzers / packet sniffers** | Deep packet inspection and forensics — `tcpdump` (CLI), Wireshark (GUI); capture raw traffic to `.pcap` |

> [!TIP] Layer them
> Combine automated alerts (IDS) with granular capture inspection (pcap in Wireshark/tcpdump). See [[Packet Analysis]] and [[Wireshark]].

---

## ✅ Monitoring Checklist

- [ ] Define the baseline — normal volumes, operating hours, authorised ports
- [ ] Inspect mismatches — protocols on non-standard ports (possible C2)
- [ ] Audit off-hours traffic — investigate temporal anomalies
- [ ] Layer tools — alerts first, packet capture for confirmation

---

## 🔗 Related

- [[Packet Analysis]]
- [[Wireshark]]
- [[tcpdump]]
- [[IDS & IPS]]
- [[Indicators of Compromise]]
- [[CSIRT & SOC Teams]]
- [[Network Fundamentals]]
