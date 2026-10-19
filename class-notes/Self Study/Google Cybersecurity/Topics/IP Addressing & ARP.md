---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🏷️ IP Addressing & ARP

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-16|2026-10-16]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 📍 IP Addresses

An **IP address** is a unique string of characters that identifies a device's location on the internet. Two types:

- **IPv4** — four sets of 1-, 2-, or 3-digit numbers separated by decimals.
- **IPv6** — uses 32 characters to accommodate more devices.

### Public vs. Private IPs

- **Public IP** — assigned by your ISP; shared by all devices on your network when communicating outside the local network.
- **Private IPs** — used for communication between devices **within the same local network**; not visible to the rest of the internet.

**Private address ranges:**
- `10.0.0.0` – `10.255.255.255`
- `172.16.0.0` – `172.31.255.255`
- `192.168.0.0` – `192.168.255.255`

**Public address ranges (examples):**
- `1.0.0.0` – `9.255.255.255`
- `11.0.0.0` – `126.255.255.255`
- `128.0.0.0` – `172.15.255.255`
- `172.32.0.0` – `192.167.255.255`

### NAT (Network Address Translation)

Replaces the **private source IP address** with its **public IP address**. NAT is part of **layer 2 (internet layer)** and **layer 3 (transport layer)** of the TCP/IP model.

### DHCP (Dynamic Host Configuration Protocol)

Application-layer protocol that **assigns unique IP addresses** and provides each device with the addresses of the appropriate **DNS server** and **default gateway**:
- DHCP servers → **port 67**
- DHCP clients → **port 68**

---

## 🔌 MAC Addresses

A **MAC address** is a unique alphanumeric identifier assigned to each physical device on a network. **Switches** use MAC addresses to direct data packets to the correct device by maintaining a **MAC address table**.

---

## 📣 How ARP Works (Address Resolution Protocol)

Operates on **layer 2** and has **no specific port number** (ports are a layer 7 concept).

- **The Shouting Method** — when a computer knows a local IP address but lacks its physical MAC address, it sends an **ARP Request** as a **Layer 2 Broadcast** (`FF:FF:FF:FF:FF:FF`).
- **Switch Behavior** — the switch is forced to forward the broadcast out to **every single connected device** on that network segment.
- **The Response** — the device that owns the requested IP responds with its MAC address, which the original computer saves into its **ARP cache**.

### 🕳️ The Security Vulnerability

- **Zero authentication** — designed in 1982, ARP has **no verification mechanism**; devices blindly accept any ARP response.
- **ARP Spoofing / Poisoning** — a malicious actor on the same network replies to the broadcast with a lie: *"I am the router, send your data to my MAC address!"*
- **Man-in-the-Middle (MitM)** — the victim updates its ARP table with the attacker's MAC address, letting the attacker intercept, steal, or alter all outgoing internet traffic.

### 🛡️ Modern Defense

- **Dynamic ARP Inspection (DAI)** — enterprise-grade switches intercept ARP replies, cross-reference them against a trusted database of known **IP-to-MAC bindings**, and instantly **drop fraudulent packets**.

---

## 📬 IPv4 Packet Breakdown

An IPv4 packet = **Data Section** (message body, up to 65,535 bytes) + **Header Section** (20–60 bytes, **13 fields**):

- **Version (VER)** — identifies the IP version (e.g. IPv4).
- **IP Header Length (HLEN/IHL)** — specifies where the header ends and data begins.
- **Type of Service (ToS)** — helps routers prioritize traffic (QoS).
- **Total Length** — total byte size of header + data.
- **Identification, Flags, & Fragmentation Offset** — track, manage, and reassemble fragmented packets.
- **Time to Live (TTL)** — countdown decreased by 1 at each router hop; at 0 the packet is discarded to prevent infinite loops.
- **Protocol** — the protocol used inside the data section (TCP, UDP…).
- **Header Checksum** — verifies the header wasn't corrupted in transit; bad packets are dropped.
- **Source & Destination IP Addresses** — routing coordinates for sender and receiver.
- **Options** — optional field (0–40 bytes) for special security settings.

---

## ⚖️ IPv4 vs IPv6

- **Address shortage** — IPv6 was created to resolve **IPv4 address exhaustion**.
- **Size & format**
    - **IPv4**: 4 bytes (32 bits), 4 decimal numbers separated by periods (`198.51.100.0`) → **4.3 billion** addresses.
    - **IPv6**: 16 bytes (128 bits), 8 hexadecimal groups separated by colons (`2002:0db8::ff21:0023:1234`) → **340 undecillion** addresses.
- **Header complexity** — IPv6 is vastly streamlined: drops complex IPv4 fields (Identification, Flags) and introduces a **Flow Label** field for packets needing specialized router handling.
- **Security & routing** — IPv6 has efficient routing structures and inherently prevents private address collisions that commonly disrupt IPv4 networks.

---

## 🔗 Related

- [[TCPIP & OSI Models]]
- [[Network Fundamentals]]
- [[Network Protocols]]
- [[Network Attacks]]
