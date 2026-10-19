---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🌐 Network Fundamentals

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-16|Related day: 2026-10-16]]

---

## 📡 What is a Network?

- A **network** is essentially a **collection of connected devices** — laptops, cell phones, smart devices at home, and workstations, printers, and servers in an office.
- Devices communicate via **cables or wireless connections**, using unique identifiers — **IP and MAC addresses** — to locate each other.

### Types of Networks

| Type | Scope | Example |
| :--- | :--- | :--- |
| **LAN** (Local Area Network) | Small geographical area — home or office | Devices connected to your home Wi-Fi |
| **WAN** (Wide Area Network) | Large geographical area — city or country | The internet itself is a prime example of a WAN |

---

## 🧱 Core Network Infrastructure

- **Firewalls** — first line of defense; monitor and restrict incoming and outgoing traffic based on organizational security rules.
- **Modems** — connect a local network to an Internet Service Provider (ISP) by converting digital signals into formats compatible with the ISP's physical infrastructure. Usually has only **one outgoing port** because you get only one public IP address.
- **Routers** — operate at the **network layer** to connect different networks and direct traffic using **IP addresses**; can include built-in firewall features.
- **Wireless Access Points (WAPs)** — emit radio waves using Wi-Fi standards to allow wireless devices to connect.

### Traffic Direction

- **Switches** *(wired)* — the preferred choice for modern local networks; analyze **MAC addresses** to forward data packets *only* to the specific intended device — the "port multiplier." Connects multiple wired devices **within the same network**.
- **Hubs** *(wired)* — outdated; blindly **repeat all incoming data to every connected port**, making them highly vulnerable to eavesdropping.
- **Client-Server Model** — an architecture where end-user devices (**clients**) send requests for information or services, and centralized systems (**servers** — DNS, file, or mail servers) fulfill them.

> [!TIP] The box at home
> The ISP box at home is a **3-in-1 combo device**: a **router** brain to talk to the modem, a built-in **switch** (the Ethernet ports on the back), and a **WAP** (the antennas) for Wi-Fi. This is why both standalone switches and home routers have Ethernet ports.

### Router Operation Modes

| Mode | What it does |
| :--- | :--- |
| **Wireless Router Mode** | Default. Acts as the main network boss — creates the network, assigns IPs, handles firewall security. |
| **Access Point (AP) Mode** | Turns off the router brain; takes a wired cable from the main router and simply emits Wi-Fi. |
| **Repeater / Range Extender Mode** | Connects to another router's Wi-Fi *wirelessly* and rebroadcasts it (cuts internet speed in half). |
| **Client / Media Bridge Mode** | Turns off Wi-Fi broadcasting; acts purely as a wireless receiver to feed internet into wired-only devices (e.g. an old desktop PC). |
| **WISP Mode** | Connects wirelessly to a public hotspot (like a hotel) but builds a private, safe firewall network inside your room. |

---

## 🗺️ Network Diagrams

Crucial **visual maps** that security analysts and administrators use to study network architecture, identify potential structural vulnerabilities, and plan defense strategies.

---

## 🔗 Related

- [[TCP/IP & OSI Models]]
- [[IP Addressing & ARP]]
- [[Network Protocols]]
- [[Firewalls]]
- [[Cloud Computing]]
- [[Network Segmentation]]
