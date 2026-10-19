---
course: Coursera Google Cybersecurity
topic: Threats
tags:
  - google
  - cybersecurity
---

# ⚔️ Network Attacks

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|2026-10-17]] · [[2026-10-18|Related day: 2026-10-18]]

---

## 🗡️ Types of Cyber Attacks

### 1. Network Interception Attacks
- **Definition:** intercepting data **in transit** to steal information or alter transmissions.
- **Packet sniffing:** using software or hardware tools to capture and inspect network traffic.
- **Risks:** attackers can read sensitive data, inject malicious code, or manipulate active transactions (e.g. altering bank transfer routing details).
- **Key variations:** on-path attacks and replay attacks.

### 2. Backdoor Attacks
- **Definition:** bypassing normal access controls via intentionally or maliciously created entry points.
- **Origin:** originally left by developers for troubleshooting, or installed by hackers to maintain **persistent access** after an initial breach.
- **Risks:** quietly deploy malware, steal corporate data, or launch a **Denial of Service (DoS)** attack.

---

## 💥 Organizational Impacts

- **💰 Financial** — halts revenue-generating systems via DoS, creates massive infrastructure repair bills, triggers expensive legal litigation or ransomware payouts.
- **🤝 Reputation** — destroys consumer trust when a breach becomes public, causing customers to flee to competitors.
- **🛡️ Public safety** — threatens physical infrastructure (power grids, water systems, military networks), potentially causing physical harm to citizens.

---

## 🚨 Denial of Service (DoS & DDoS)

- **DoS** — targets a network or server by **flooding it with network traffic** to disrupt operations; the attacker wins if *any* part of the network is overloaded (raw traffic volume or crafted packets forcing excessive processing).
- **DDoS (Distributed DoS)** — a DoS attack utilizing **multiple devices or servers across different locations**, making it far more likely to overwhelm the target.

### Common Network-Level DoS Attacks

| Attack | Protocol | Mechanism | Core impact |
| :--- | :--- | :--- | :--- |
| **SYN Flood** | TCP | Exploits the TCP three-way handshake (`SYN` → `SYN/ACK` → `ACK`): floods the server with `SYN` requests without completing the final step | Leaves ports open, exhausts available server ports, blocks legitimate connections |
| **ICMP Flood** | ICMP | Repeatedly sends ICMP packets, forcing the server to generate corresponding replies | Consumes all available inbound/outbound bandwidth, crashes the server |
| **Ping of Death** | ICMP | Sends an oversized ICMP packet exceeding the standard **64 KB** maximum | Overloads vulnerable systems during packet reassembly/processing, crashing the target |

---

## 🔍 Packet Sniffing & Protocol Analyzers

Packets contain a **header** (source/destination IPs) and a **body** (payload — personal info, credentials, financial data). Sniffing = intercepting and inspecting packets with software or hardware tools — legitimate for analysts investigating/troubleshooting, malicious when used to spy.

### Operational Modes

- **Passive sniffing** — reads packets in transit without altering them (like a mail carrier reading mail inside an envelope); common on **hub** networks where all traffic is visible to all hosts.
- **Active sniffing** — **alters** packets in transit, modifies payload data (e.g. bank details), or injects protocols to reroute traffic to unintended ports.
- **Promiscuous mode** — by default a NIC only accepts packets destined for its own MAC address; in promiscuous mode it accepts **all** network traffic passing through the medium.

### Protocol Analyzer Tools
- **tcpdump** — popular, lightweight, **command-line** analyzer built on the open-source `libpcap` library; preinstalled on many Linux distributions, available on macOS/Unix.
    - Standard output fields: **timestamp**, **source IP & port**, **destination IP & port**; by default resolves IPs into hostnames and ports into service names.
    - Use cases: establishing network baselines, identifying unauthorized traffic/access points, detecting malicious payloads, configuring custom alerts.
- Also: **Wireshark**, SolarWinds NetFlow Traffic Analyzer, ManageEngine OpManager, Azure Network Watcher.

---

## 🎭 IP Spoofing & Related Attacks

**IP spoofing** = altering the **source IP** in a packet header to impersonate a trusted/authorized system — used to bypass firewall rules filtering external traffic and to hide the attacker's origin.

```
                  ┌──► On-path Attack (MitM / data intercept / DNS spoofing)
                  │
IP Spoofing ──────┼──► Replay Attack (delaying or repeating captured auth traffic)
                  │
                  └──► Smurf Attack (hybrid DoS / broadcast amplification via ICMP)
```

### 1. On-Path Attack (Meddler-in-the-Middle)
- Positions themselves between two communicating endpoints (e.g. browser ↔ web server).
- Sniffs packets to learn IPs and MACs, then impersonates either endpoint.
- **DNS spoofing risk:** intercepts DNS queries and returns **falsified IP records**, redirecting users to malicious domains.

### 2. Replay Attack
- Intercepts valid transmissions and **delays or repeats** them later.
- Desynchronizes sessions or impersonates an authorized entity by replaying past authentication exchanges.

### 3. Smurf Attack
- Hybrid of **IP spoofing** + **DDoS amplification**.
- Attacker spoofs the victim's IP and sends ICMP Echo requests to the network's **broadcast address**.
- Every active device sends an ICMP Echo Reply back to the spoofed victim simultaneously, exhausting bandwidth and processing resources.

---

## 💣 Brute Force Attacks

A **brute force attack** is a trial-and-error process to discover private credentials — tedious manually, but attackers scale it with automated tools.

- **Simple brute force** — guess login credentials by entering username/password combinations until one works.
- **Dictionary attacks** — use a pre-compiled list of commonly used passwords and stolen credentials from previous breaches. *Historical note: named because they used literal dictionary word lists before complex password rules became standard.*

---

## 🛡️ Prevention — Defense-in-Depth

No single control stops all network attacks; defenses must be **layered** across protocols, infrastructure, and endpoints.

- **Data-in-transit encryption** — implement **HTTPS (SSL/TLS)** and **VPNs**; even if packets are intercepted, the payload stays unreadable.
- **Firewall ingress filtering** — reject incoming traffic from external interfaces claiming an **internal/private source IP**; deploy **NGFW** with anomaly detection to spot broadcast storms and Smurf signatures.
- **Network hygiene** — avoid unencrypted open public Wi-Fi unless an encrypted **VPN tunnel** is established first.

---

## 🔗 Related

- [[TCP/IP & OSI Models]]
- [[IP Addressing & ARP]]
- [[Firewalls]]
- [[VPNs]]
- [[Threat Actors]]
- [[Common Vulnerabilities]]
