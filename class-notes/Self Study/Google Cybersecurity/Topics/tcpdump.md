---
course: Coursera Google Cybersecurity
topic: Tools
tags:
  - google
  - cybersecurity
---

# 🐚 tcpdump

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

**tcpdump** is a command-line packet sniffer. It's pre-installed on many Linux distributions and available for Unix-like systems including macOS. No GUI — just fast, scriptable capture.

---

## ⌨️ Basic Syntax

```bash
sudo tcpdump [-i interface] [option(s)] [expression(s)]
```

- Requires elevated privileges — run with **`sudo`**.
- The **`-i`** flag specifies the network interface (required).

---

## ⚙️ Essential Options

| Flag | Purpose |
| :--- | :--- |
| `-i <interface>` | Capture on a specific interface (e.g. `eth0`) |
| `-w <file>` | **Write** captured packets directly to a `.pcap` file |
| `-r <file>` | **Read** and display a saved capture file |
| `-v` | Verbose output (more header detail) |
| `-c <count>` | Stop after *N* packets |
| `-n` | Don't resolve hostnames (avoids reverse DNS lookups) |
| `-nn` | Don't resolve hostnames **or** port names |

> [!TIP] Why `-nn` matters
> Name resolution generates extra DNS traffic and can leak your queries — and it slows capture down. Use `-nn` for clean, fast output.

---

## 🎯 Filter Expressions

Isolate traffic by protocol, host, or port, and combine targets with Boolean operators.

```bash
sudo tcpdump -i eth0 -nn port 443            # HTTPS only
sudo tcpdump -i eth0 -nn host 10.0.0.5       # one host
sudo tcpdump -i eth0 -nn ip6                 # IPv6 traffic
sudo tcpdump -i eth0 -nn 'src 10.0.0.5 and not port 22'
sudo tcpdump -i eth0 -w capture.pcap -c 100  # first 100 packets to file
sudo tcpdump -r capture.pcap -nn             # read it back
```

Operators: **`and`**, **`or`**, **`not`**.

---

## 📖 Reading the Output

Each line gives you:

- **Timestamp** — for building a timeline
- **Source and destination IP addresses**
- **Source and destination port numbers**
- **IP version**, **TTL**, fragmentation flags, protocol type
- **Checksum verification** and **TCP flags** (`PUSH`, `ACK`, `SYN`…)

> [!EXAMPLE] Typical line
> `12:04:31.882 10.0.0.5.51234 > 93.184.216.34.443: Flags [P.], seq 1:518, ack 1, win 502, length 517`

---

## 🧭 tcpdump vs. Wireshark

Use `tcpdump` when you need to capture **on a server, over SSH, in a script, or with no desktop**. Save to `.pcap` with `-w`, then analyse in [[Wireshark]] with display filters and follow-stream.

---

## 🔗 Related

- [[Wireshark]]
- [[Packet Analysis]]
- [[Network Traffic Analysis]]
- [[Network Attacks]] — packet sniffing & promiscuous mode
- [[Linux Commands]]
