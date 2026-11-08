---
course: Coursera Google Cybersecurity
topic: Detection
tags:
  - google
  - cybersecurity
---

# 📦 Packet Analysis

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-04|Related day: 2026-11-04]]

Data travelling across a network is broken into **packets**. Reading them is the foundation of network forensics — and of understanding what analysis tools are actually showing you.

---

## 🧱 Anatomy of a Packet

| Section | Contents |
| :--- | :--- |
| **Header** | Metadata for routing and processing. Often **layered** (Ethernet, IP, TCP/UDP). Fields: source/destination IP, ports, sequence numbers, packet length |
| **Payload** | The actual user data — e.g. the bytes of an image being uploaded |
| **Footer / Trailer** | At the very end of the frame; used at the **Data Link layer** (Ethernet's Frame Check Sequence) for error checking. Most Layer 3+ protocols (IP) have **no footer** |

---

## 🌐 IPv4 Header Fields

IP operates at the **Internet layer** of the TCP/IP model and acts like a courier, routing packets between hosts.

| Category | Field | Purpose |
| :--- | :--- | :--- |
| **Sizing** | Version | IPv4 vs. IPv6 |
| | IHL (Internet Header Length) | Length of the header itself, including options |
| | Total Length | Entire packet size in bytes (header + payload) |
| **Addressing** | Source Address | Sender's IP |
| | Destination Address | Intended recipient's IP |
| **Delivery** | Type of Service (ToS / DiffServ) | Priority and QoS requirements |
| | Time to Live (TTL) | Hop counter decremented by each router; packet is dropped at 0 — prevents endless loops |
| | Protocol | Next-level protocol in the payload (ICMP, TCP, UDP) |
| **Fragmentation** | Identification | Unique ID grouping fragments of the same original packet |
| | Flags | Don't Fragment, More Fragments |
| | Fragment Offset | Where this fragment belongs in the datagram |
| **Integrity** | Header Checksum | Verifies header integrity; dropped on error |

---

## 🌐 IPv6 Header Fields

Adoption keeps rising because of IPv6's enormous address space. It has **eight** fields:

**Version** · **Traffic Class** (≈ ToS, priority) · **Flow Label** (identifies packets in a flow) · **Payload Length** · **Next Header** (type of following header, e.g. TCP) · **Hop Limit** (≈ TTL) · **Source Address** · **Destination Address**

---

## 📥 Capturing Traffic

### How protocol analyzers work

1. **Ingestion via the NIC** — by default a NIC only processes frames addressed to it or to broadcast/multicast groups. To capture everything, the interface must be put into a capture mode:
    - **Promiscuous mode** — captures **all wired frames** visible on the network segment.
    - **Monitor mode** — captures **wireless frames** without associating with an access point.
2. **Placement** — the analyzer must sit on the right segment, switch port (**SPAN / port mirroring**) or network tap to see the target traffic.
3. **Decoding** — raw binary is parsed into structured protocol fields for filtering and inspection.

> [!WARNING] Legality
> Promiscuous mode on a shared network captures credentials and confidential data. **Unauthorised packet interception is illegal in most jurisdictions.**

---

## 🗃️ PCAP Libraries & Formats

A **packet capture (pcap)** file stores raw traffic for offline inspection.

| Library / format | Notes |
| :--- | :--- |
| **Libpcap** | Standard portable C/C++ capture library for Unix-like systems; used natively by `tcpdump` |
| **WinPcap** | Older Windows port of libpcap — largely deprecated |
| **Npcap** | Modern Windows capture architecture from the Nmap project; supports loopback and raw 802.11 captures |
| **PCAPng** | Next-generation format — packets **plus** metadata, interface statistics, decryption secrets and comments in one file |

---

## 🔬 Using Captures in an Investigation

pcap files are an **evidentiary source** used to:

- **Detect IoCs** — unencrypted credentials, C2 traffic, anomalous beacons, port scans.
- **Reconstruct timelines** — walk the attack chain to establish exactly what was accessed or exfiltrated.
- **Filter targeted artefacts** — narrow millions of frames down to specific communications by IP, port or protocol.

> [!NOTE] Evidence handling
> Captures used as evidence must follow the [[Post-Incident Activity|chain of custody]] — write-protect and hash them before analysis.

---

## 🔗 Related

- [[tcpdump]]
- [[Wireshark]]
- [[Network Traffic Analysis]]
- [[TCPIP & OSI Models]]
- [[IP Addressing & ARP]]
- [[Network Protocols]]
