---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 📶 Wi-Fi Security

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 🔹 Overview of IEEE 802.11

- **Definition:** commonly known as **Wi-Fi** — a suite of communication protocols that defines standards for **Wireless Local Area Networks (WLANs)**.
- **IEEE** = the **Institute of Electrical and Electronics Engineers**, the organization responsible for maintaining these wireless standards.
- **Wi-Fi** is a marketing term commissioned by the **Wi-Fi Alliance** (formerly WECA); it refers to the IEEE 802.11 family of standards.

---

## 🔒 Wireless Security Protocols — Evolution

### 1. WEP (Wired Equivalent Privacy)
- **Launch:** 1999 — the oldest wireless standard.
- **Goal:** give wireless connections the same privacy level as wired networks.
- **Status:** outdated and **high-risk**; encryption flaws let attackers easily break it.
- **Analyst note:** still encountered on legacy hardware or unconfigured router defaults.

### 2. WPA (Wi-Fi Protected Access)
- **Launch:** 2003, as a transitional measure compatible with older hardware.
- **Improvements over WEP:** introduced **TKIP** (Temporal Key Integrity Protocol), larger secret keys, and a **Message Integrity Check** to reject altered transmissions.
- **Core vulnerability:** susceptible to **KRACK (Key Reinstallation Attacks)** — an attacker intercepts the authentication handshake and forces an encryption key of all zeros.

### 3. WPA2
- **Launch:** 2004; widely considered the current baseline standard.
- **Improvements over WPA:** replaced TKIP with **AES** (Advanced Encryption Standard) and introduced **CCMP** for strong encapsulation and message integrity.
- **Core vulnerability:** remains vulnerable to KRACK attacks.

**WPA2 deployment modes:**
- **Personal Mode** — a global passphrase manually applied to each device; easy to set up, ideal for home networks, but unmanageable for corporations.
- **Enterprise Mode** — individualized, centralized control over access via an **authentication server**; users never see encryption keys, preventing attackers from recovering keys from lost devices.

### 4. WPA3
- **Launch:** 2018; steadily growing in adoption.
- **Improvements over WPA2:**
    - Fixes the handshake vulnerability to **prevent KRACK attacks**.
    - Introduces **SAE (Simultaneous Authentication of Equals)** — a password-authenticated key agreement that blocks offline data decryption attempts.
    - Upgrades baseline protection to **128-bit encryption**, with an optional **192-bit encryption** tier for Enterprise mode.

---

## 🔗 Related

- [[Network Protocols]]
- [[Network Fundamentals]]
- [[Firewalls]]
- [[Threat Actors]]
