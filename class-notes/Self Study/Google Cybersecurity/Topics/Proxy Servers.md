---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🔄 Proxy Servers

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 📌 What is a Proxy Server?

A **dedicated server** that acts as an **intermediary**, fulfilling client requests by forwarding them to other servers.

- **Network position:** sits directly between the public internet and the internal private network.
- **IP anonymization:** operates using a **public IP address completely distinct** from the internal network, hiding private host IPs from external discovery.
- When an external client receives a response, they see a **distorted proxy IP or no IP at all**, obscuring the organization's real backend server.

---

## 🛡️ Core Security & Performance Functions

- **Inbound inspection** — evaluates incoming connection requests to determine if they are safe before letting traffic interact with the network.
- **Content filtering** — enforces web policies by blocking employee access to unsafe, unauthorized, or malicious websites.
- **Caching (temporary memory)** — stores frequently requested data locally, serving common requests directly from cache; minimizes direct external contact with internal servers and **reduces the attack surface**.

---

## 🗂️ Classifications

### 1. Forward Proxy
- **Direction:** regulates **outgoing** traffic *(Internal User ➡️ Internet)*.
- **Objective:** intercepts internal employees' traffic, verifies it against organizational policy, hides the individual user's IP, and forwards approved requests to the internet.

### 2. Reverse Proxy
- **Direction:** regulates **incoming** internet access to internal systems *(Internet ➡️ Internal Server)*.
- **Objective:** accepts external traffic, validates requests, and forwards them to backend servers.
- **Application:** shields internal web servers containing confidential data, ensuring their true IPs are never exposed publicly.

### 3. Email Proxy
- **Direction:** inspects inbound and outbound electronic mail.
- **Objective:** flags and filters **spam and phishing** attempts; validates sender headers to ensure the source address was not forged — mitigating **business email compromise (BEC)** and impersonation attacks.
- **Real-world use:** multi-layered anti-spam architectures deployed on enterprise or ISP email proxies scrub messages before delivery.

---

## 🔗 Related

- [[VPNs]]
- [[Firewalls]]
- [[Network Segmentation]]
- [[Phishing & Social Engineering]]
