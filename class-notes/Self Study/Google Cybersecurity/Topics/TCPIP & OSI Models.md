---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🧱 TCP/IP & OSI Models

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-16|Related day: 2026-10-16]]

---

## 📦 Data Packets

- **Data packets** are the fundamental units of information transferred across a network — like a physical letter.
- Each packet contains a **header** (destination and source IP/MAC addresses, protocol number), the **message body**, and a **footer** to signal completion.

### Network Performance & Security

- Performance is measured by **bandwidth** (data received per second) and **speed** (rate of packet reception).
- **Irregularities in bandwidth or speed can indicate a network attack**, making these metrics crucial for security personnel.
- **Packet sniffing** — capturing and inspecting data packets to monitor network communication; used to analyze activity and identify threats. → see [[Network Attacks]]

---

## 🌐 The TCP/IP Model

- **TCP (Transmission Control Protocol)** — establishes connections between devices and organizes data for transmission; ensures packets reach their correct destination.
- **IP (Internet Protocol)** — provides standards for **routing and addressing** data packets; IP addresses act as unique device identifiers.
- **Ports** — software-based locations within an operating system that organize data sending and receiving; port numbers let devices prioritize and process network traffic.

### The 4 Layers

1. **Network Access Layer** — handles the creation of data packets and their transmission; involves hardware devices, physical cables, and switches.
2. **Internet Layer** — IP addresses are attached to packets here to specify sender and receiver; determines how networks connect.
    - **IP** — sends data packets to the correct destination and relies on TCP/UDP to deliver them to the corresponding service; IP packets allow communication *between two networks*. TCP retransmits lost or corrupt data.
    - **ICMP (Internet Control Message Protocol)** — shares **error information and status updates** about packets; useful for detecting/troubleshooting network errors (dropped packets, connectivity issues, packets redirected to other routers).
3. **Transport Layer** — manages the flow of network traffic, including error control.
    - **TCP** — connection-oriented; two devices form a connection and stream data. Contains the destination **port number** in the TCP header; reliably transmits data.
    - **UDP (User Datagram Protocol)** — **connectionless**; does not establish a connection before transmission and tracks data less extensively. Used by performance-sensitive real-time applications (e.g. video streaming).
4. **Application Layer** — defines how data packets interact with receiving devices: HTTP, SMTP, SSH, FTP, DNS.

![[Pasted image 20261016085430.png]]

---

## 📶 The OSI Model

The **OSI model** is a standardized concept describing the **seven layers** computers use to communicate and send data over the network. Network and security professionals use it to communicate about potential sources of problems or security threats.

![[Pasted image 20261016090022.png]]

| Layer | Name | Role |
| :--- | :--- | :--- |
| **7** | **Application** | The user-facing interface. HTTP/HTTPS, SMTP, DNS — connects software applications to the internet. |
| **6** | **Presentation** | The translator. Data **formatting, encryption (SSL), compression**, character translation. |
| **5** | **Session** | The manager. Opens, maintains, checkpoints, and terminates **communication sessions**. |
| **4** | **Transport** | The delivery service. **Segmentation** of large data, transfer speeds, TCP and UDP. |
| **3** | **Network** | The traffic router. Moves packets **between different networks** using **IP addresses** and routers. |
| **2** | **Data Link** | Local delivery. Moves data **within a single LAN** using **MAC addresses**, NICs, and switches. |
| **1** | **Physical** | The raw hardware. Translates data into **binary (0s and 1s)** for cables, wires, hubs, modems. |

---

## ⚖️ Layer 2 vs Layer 3 — the Core Difference

The fundamental difference comes down to **scope**: **Layer 2 stays local, Layer 3 goes global**.

- **Data Link Layer (L2)**
    - Scope: communication **inside the same local network (LAN)** only.
    - Addressing: **MAC addresses** — the permanent physical hardware address.
    - Main device: **switches** (can't talk to the outside world).
- **Network Layer (L3)**
    - Scope: communication **between multiple separate networks** across the globe.
    - Addressing: **IP addresses** — logical coordinates assigned to a network location.
    - Main device: **routers**.

---

## 🔗 Related

- [[Network Fundamentals]]
- [[IP Addressing & ARP]]
- [[Network Protocols]]
- [[Network Attacks]]
