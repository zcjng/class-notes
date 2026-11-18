---
course: Coursera Google Cybersecurity
topic: Cryptography
tags:
  - google
  - cybersecurity
---

# 🔑 Encryption & PKI

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

Encryption is the process of converting readable data into an encoded format. **Public Key Infrastructure (PKI)** is the framework that makes encrypted communication *trustworthy* — not just unreadable.

---

## 🔀 The Two Main Types

| | **Symmetric** | **Asymmetric** |
| :--- | :--- | :--- |
| **Keys** | One shared secret key for both lock and unlock | A **public** key pair — public encrypts, private decrypts |
| **Speed** | Fast | Slow |
| **Security** | Lower — anyone holding the key can decrypt | Higher — private key never leaves the owner |
| **Use case** | Bulk data transfer, sessions | Key exchange, small high-value blocks, signatures |

> [!NOTE] They work as a team
> Web apps typically open a connection with **asymmetric** encryption (to safely exchange a key / verify identity), then switch to **symmetric** for the session — security for the handshake, speed for the payload.

---

## 📏 Key Length — The Brute-Force Trade-off

**Brute force** tries every possible key until one works.

- Longer key = exponentially more possibilities = harder to crack.
- Longer key = **slower processing**.
- Short keys are fast but weak; the whole discipline is balancing speed against safety.

---

## 🧮 Approved Algorithms

### Symmetric

| Algorithm | Key sizes | Notes |
| :--- | :--- | :--- |
| **Triple DES (3DES)** | 3 × 56-bit → effective **168-bit** | Block cipher; applies DES three times. Legacy/backwards-compatibility only — limited data volume. |
| **AES** | **128 / 192 / 256-bit** | One of the most secure symmetric algorithms today. Brute-forcing AES-128 is estimated at billions of years. |

### Asymmetric

| Algorithm | Key sizes | Notes |
| :--- | :--- | :--- |
| **RSA** | **1,024 / 2,048 / 4,096-bit** | One of the first public/private key algorithms (MIT). Used for highly sensitive data. |
| **DSA** | **2,048-bit** | Introduced by NIST in the early 1990s; complements RSA in PKI. |

### Generating keys

**OpenSSL** is the open-source CLI tool used to generate public/private keys and verify digital certificates.

> [!WARNING] Heartbleed (2014)
> A vulnerability in OpenSSL that exposed sensitive data held in the memory of websites and apps. Patched the same year — a textbook case for keeping software up to date.

---

## 🏛️ Kerckhoff's Principle — Obscurity Is Not Security

A cipher must be **provably unbreakable**, not merely secret. All details of an algorithm — *except the private key* — should be public without sacrificing security.

- AES is fully documented online and still unbreakable.
- Custom "secret" algorithms have repeatedly been cracked once published.
- If a system's security depends on hiding **how it works**, it is not secure.

---

## 🪪 Trust: Digital Certificates & CAs

Encryption alone doesn't answer *"is this key really owned by the person I think it is?"*

- **Digital certificates** act as **digital ID badges** binding a public key to a verified identity.
- A trusted **Certificate Authority (CA)** verifies the organization's details, then **signs** the certificate with its own private key.
- The client trusts the CA → therefore trusts the signed key → therefore trusts the server.

> [!TIP] The trust chain
> Root CA → intermediate CA → server certificate. Break any link and the browser throws a warning.

---

## ⚖️ Compliance Drivers

Encryption is increasingly mandated, not optional:

- **FIPS 140-3** — US federal standard for cryptographic modules.
- **GDPR** — requires effective protection of personal data.

---

## 🔗 Related

- [[Cryptography]] — core terms, Caesar cipher, key management
- [[Hashing & Password Storage]]
- [[Access Control & AAA]]
- [[VPNs]]
- [[Wi-Fi Security]]
- [[Compliance & Regulations]]
- [[Cloud Security]]
