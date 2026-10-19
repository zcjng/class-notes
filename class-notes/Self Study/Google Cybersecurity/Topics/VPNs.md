---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🛡️ VPNs

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 🌐 The Privacy Risk of Standard Traffic

- **ISP exposure** — when connecting to the internet normally, your ISP receives and forwards **all** network requests directly to destination servers.
- **Data leakage** — standard requests include your **public IP address, private data, and physical location**.
- **Interception threat** — if unencrypted traffic is intercepted, malicious actors can tie your online activity to your physical location and steal sensitive information like banking details and credit card numbers.

---

## 📌 What is a VPN?

A **network security service** that creates an **encrypted tunnel** between your local device and a remote VPN server.

- **Anonymization** — masks your public IP address and hides your true virtual location.
- **Confidentiality** — encrypts your data as it travels the public internet; unreadable without the correct cryptographic key.

---

## 📦 Data Encapsulation

A routing paradox: encrypting an entire packet secures the information, but routers can no longer read the destination IP/MAC addresses to deliver it. VPNs solve this with **encapsulation**:

1. **Encryption** — the VPN software encrypts your original data packet (including your real IP and payload).
2. **Encapsulation** — it wraps that encrypted packet inside a **new, unencrypted outer packet** containing the destination address of the VPN server.
3. **Routing** — routers read the outer packet and forward it across the internet.
4. **Decryption** — at the VPN server, the outer layer is stripped, the inner packet is processed, and it is safely forwarded to the final destination **using the VPN's IP address instead of yours**.

---

## 🗺️ VPN Deployments

| Type | Audience | Function |
| :--- | :--- | :--- |
| **Remote Access VPN** | Individual users / remote employees | Secure connection between a single personal endpoint and a corporate or provider VPN server |
| **Site-to-Site VPN** | Enterprises with multiple locations | Extends a corporate network to link separate remote networks (e.g. branch office ↔ HQ); seamless but significantly more complex to configure and manage |

---

## 🔏 VPN Protocols: WireGuard vs. IPSec

A **VPN protocol** is a dedicated set of rules determining how data moves and establishes a secure tunnel between endpoints.

### ⚡ WireGuard
- Relatively new, **open-source** protocol engineered for **high speed, clean deployment, and simple maintenance**.
- Lightweight codebase (fewer lines of code) → drastically better download speeds.
- Flexible: client-server (remote access) *and* site-to-site.
- Open-source → easier to deploy, audit, and debug.
- **Best for:** high-bandwidth processes like massive downloads or video streaming.

### 🏛️ IPSec
- One of the earliest, most established **cryptographic network suites**.
- Built into foundational network layers to encrypt and authenticate individual packets.
- **Compatibility:** native support baked into most modern operating systems.
- **Maturity:** favored by conservative enterprise environments (long track record, extensive vetting, global adoption).
- **Disadvantage:** considerably older, larger, and more complex to configure and optimize than WireGuard.

---

## 🔗 Related

- [[Firewalls]]
- [[Proxy Servers]]
- [[Network Attacks]]
- [[Network Protocols]]
- [[Security Controls]]
