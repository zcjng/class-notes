---
course: Coursera Google Cybersecurity
topic: Tools
tags:
  - google
  - cybersecurity
---

# 🦈 Wireshark

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-04|Related day: 2026-11-04]]

**Wireshark** is an open-source network protocol analyzer with a **graphical interface** — the GUI counterpart to [[tcpdump]]. It makes network communications easy to visualize and inspect.

---

## 🔎 Display Filters

Display filters apply to an already-captured file. You can filter on protocols, IP addresses, ports, and essentially **any property found in a packet**.

### Comparison operators

| Operator type | Symbol | Abbreviation |
| :--- | :--- | :--- |
| Equal | `==` | `eq` |
| Not equal | `!=` | `ne` |
| Greater than | `>` | `gt` |
| Less than | `<` | `lt` |
| Greater than or equal | `>=` | `ge` |
| Less than or equal | `<=` | `le` |

`ip.src == 8.8.8.8` and `ip.src eq 8.8.8.8` are identical. Combine with the Boolean logical operators `and` / `or`.

### Special operators

| Operator | Use |
| :--- | :--- |
| **contains** | Filter packets containing an exact match of a string of text |
| **matches** | Filter packets based on a specified **regular expression** |

### Filtering by protocol

The simplest filter is just the protocol name — type `dns` in the filter toolbar:

```
dns   http   ftp   ssh   arp   telnet   icmp
```

> [!EXAMPLE] Common patterns
> - `ip.addr == 10.0.0.5` — any traffic to or from one host
> - `tcp.port == 443` — HTTPS conversations only
> - `http.request` — every HTTP request in the capture

---

## 🌊 Follow Streams

Wireshark can filter for packets belonging to one protocol conversation and **reassemble the stream** into readable form.

A **stream** (or conversation) is the exchange of data between devices using a protocol. "Follow → TCP/HTTP Stream" turns a wall of packets into the actual request and response text.

> [!TIP] This is how you read a plaintext credential, an HTTP download, or an attacker's C2 exchange without decoding hex by hand.

---

## 🧭 When to Use Which Tool

| Situation | Tool |
| :--- | :--- |
| Live capture on a remote/headless Linux box | [[tcpdump]] |
| Deep interactive analysis of a saved `.pcap` | Wireshark |
| Automating capture in a script | `tcpdump` |
| Reading a reassembled conversation | Wireshark follow-stream |

---

## 🔗 Related

- [[tcpdump]]
- [[Packet Analysis]]
- [[Network Traffic Analysis]]
- [[Network Attacks]] — packet sniffing
- [[Network Protocols]]
