---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 🧾 Log Management & Formats

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

Logs record **what, where and when** events happen, letting analysts build timelines and detect malicious activity. This note covers the operational side — collection, retention, and the formats you'll actually parse.

> [!NOTE] For the basics of log types and SIEM tools, see [[Logs & SIEM]].

---

## 📥 Collection

- **Log forwarders** automatically collect data from multiple sources and forward it to a central repository.
- **SIEM tools** aggregate, normalize and process large volumes of diverse formats in real time.
- **Common sources** — network devices, operating systems, applications, security tools (IDS/IPS), authentication systems.
- **Key entry fields** — action taken (e.g. `ALLOW`), source IP, timestamp.

---

## ⚠️ Overlogging Is a Real Cost

**Log management** covers the full lifecycle: collecting, storing, analysing and **disposing** of log data. You must deliberately decide what to record.

Overlogging causes:

- High storage costs
- Degraded system performance
- Difficulty finding the events that matter
- **Compliance risk** — logs may contain PII you're not permitted to retain

---

## 🗄️ Retention & Protection

- Retention requirements are usually **regulatory** — **FISMA** (public sector), **HIPAA** (healthcare), **PCI DSS** (finance).
- Send logs **directly to a centralized log server**. Logs left on local machines can be tampered with by an attacker covering their tracks.

---

## 📐 Log Formats

| Format | Structure | Where you see it |
| :--- | :--- | :--- |
| **JSON** | Key-value pairs, arrays and objects in curly brackets | Modern apps, cloud services; lightweight and readable |
| **Syslog** | Header + structured data in key-value pairs + message | Unix/Linux native; also a forwarding **protocol** |
| **XML** | Hierarchical tags, elements and attributes | Windows event logs |
| **CSV** | Comma-separated values, positional rather than named | Exports, spreadsheet-friendly |
| **CEF** | Pipe-separated fields: vendor, product, version, severity, plus optional key-value extensions | Security appliances; usually transported via Syslog |

**Syslog severity** and priority fields are what let a SIEM decide which events deserve an alert versus a quiet line in a file.

> [!EXAMPLE] Same event, different shapes
> `{"src_ip":"218.124.14.105","user":"nuhara","action":"FAILED_LOGIN"}` (JSON)
> `Apr 3 11:01:21 server sshd[1088]: Failed password for user nuhara from 218.124.14.105 port 5023` (Syslog)

---

## 📡 Telemetry

**Telemetry** is the collection and transmission of data — such as packet captures — used alongside logs as evidence. Detection technologies turn raw device activity into structured logs that can be analysed.

---

## 🔗 Related

- [[Logs & SIEM]]
- [[IDS & IPS]]
- [[Packet Analysis]]
- [[Indicators of Compromise]]
- [[Data Governance]]
- [[Compliance & Regulations]]
