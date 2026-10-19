---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 📡 Network Protocols

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

**Network protocols** are sets of rules that govern the **order of delivery and structure of data** exchanged between devices on a network. Port numbers for some protocols are assigned by the **Internet Assigned Numbers Authority (IANA)**.

---

## 🔑 Key Protocols

- **TCP (Transmission Control Protocol)** — establishes connections and streams data between devices, verifying them through a **"handshake"** before communication.
- **ARP (Address Resolution Protocol)** — determines the MAC address of the next router or device in the data's path (layer 2, **no port number**). → [[IP Addressing & ARP]]
- **HTTPS** — secure communication between web browsers and servers, encrypting data using **SSL/TLS**. Uses **port 443** (HTTP uses port 80).
- **DNS (Domain Name System)** — translates internet domain names (like *yummyrecipesforme.org*) into **IP addresses** used as destination addresses for packets. Uses **port 53**.
- **DHCP** — assigns unique IPs plus DNS server and default gateway. Server: **port 67**, client: **port 68**. → [[IP Addressing & ARP]]

---

## 🗂️ Protocol Categories

| Category | Protocols | Purpose |
| :--- | :--- | :--- |
| **Communication** | TCP, UDP, HTTP, DNS | Data transmission and communication between devices |
| **Management** | SNMP, ICMP | Monitoring, managing, and troubleshooting network activity |
| **Security** | HTTPS, SFTP | Secure data transmission via encryption (but they don't conceal IP addresses) |

> [!NOTE] SFTP
> **Secure File Transfer Protocol (SFTP)** transfers files securely over a network using **SSH**, typically through **TCP port 22**. SSH uses **AES** encryption and other types so unintended recipients cannot intercept transmissions.

---

## 🖧 Common Application-Layer Protocols & Ports

### Telnet
Application-layer protocol used to connect with a local or remote system. **Not as secure as SSH.** Uses **TCP port 23**.

### SSH (Secure Shell)
Creates a **secure connection** with a remote system; alternative for secure authentication and encrypted communication. Operates over **TCP port 22**.

### POP / POP3 (Post Office Protocol)
Application-layer protocol (layer 4 of the TCP/IP model) to **manage and retrieve email** from a mail server; POP3 is the most common version.
- Unencrypted plaintext authentication → **TCP/UDP port 110**
- Encrypted via SSL/TLS → **TCP/UDP port 995**

### IMAP (Internet Message Access Protocol)
Used for **incoming email**; downloads headers and message content, which **remain on the server** — allowing access from multiple devices and syncing across them. Users can partially read email before it finishes downloading.
- Unencrypted → **TCP port 143**
- Over TLS → **TCP port 993**

### SMTP (Simple Mail Transfer Protocol)
**Transmits and routes email** from sender to recipient. Works with Message Transfer Agent (MTA) software, which searches DNS servers to resolve email addresses to IP addresses.
- Unencrypted (often used by high-volume spam) → **TCP/UDP port 25**; SMTP helps filter spam by regulating how many emails a source can send at a time.
- Encrypted via TLS → **TCP/UDP port 587**

![[Pasted image 20261017073657.png]]

---

## 🔐 HTTPS & Encryption

HTTPS encrypts data using **SSL (Secure Sockets Layer) / TLS (Transport Layer Security)**, protecting sensitive information from malicious actors during transmission.

---

## 🔗 Related

- [[TCP/IP & OSI Models]]
- [[IP Addressing & ARP]]
- [[Network Fundamentals]]
- [[Wi-Fi Security]]
- [[VPNs]]
