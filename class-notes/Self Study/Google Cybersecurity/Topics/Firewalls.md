---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🧱 Firewalls

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 📌 What is a Firewall?

A **network security device** that monitors **incoming and outgoing network traffic**.

- **Core function:** block or allow traffic based on a predefined set of **security rules** determined by an organization's security policy.
- **Port filtering:** limits unwanted communication by explicitly opening or closing specific ports (e.g. allowing **port 443** for HTTPS or **port 25** for email while dropping everything else).

---

## 🎛️ Types by Deployment

### 1. Hardware Firewall
- A **physical, standalone appliance**.
- Inspects every individual data packet before it is permitted to physically enter the internal network.
- The most foundational line of defense against network perimeter threats.

### 2. Software Firewall
- A software application installed directly onto existing hardware.
    - **Computer deployment:** analyzes traffic strictly received by that specific local machine.
    - **Server deployment:** protects all secondary devices connected to and relying on that server.
- **Trade-offs:** costs less than physical hardware and saves space, but adds a processing/resource burden to host devices.

### 3. Cloud-Based Firewall / Firewall as a Service (FaaS)
- Software firewalls hosted entirely by a **third-party cloud provider**.
- Rules are configured via the provider's management interface.
- **Traffic flow:** scrubs and filters incoming traffic **in the cloud** *before* it reaches the onsite network.
- **Scope:** secures local networks as well as existing cloud-native assets and processes.

---

## 🧠 State Tracking Architecture

| Type | Behavior | Protection |
| :--- | :--- | :--- |
| **Stateful** | Proactively tracks the **connection state and information history** of passing packets; dynamically analyzes characteristics and behavioral context to flag suspicious trends | Higher |
| **Stateless** | Operates strictly on **static, preconfigured rules**; does not store packet information or track connection states — blind to broader behavioral anomalies | Lower |

---

## 🚀 Next-Generation Firewalls (NGFW)

- Deliver significantly higher security than standard stateful configurations.
- **Deep packet inspection (DPI)** and active **intrusion protection**.
- **Threat intelligence:** connect directly to cloud-based services for real-time definition updates against zero-day and emerging exploits.

---

## 🔗 Related

- [[Network Fundamentals]]
- [[Network Segmentation]]
- [[VPNs]]
- [[Security Controls]]
- [[Network Attacks]]
