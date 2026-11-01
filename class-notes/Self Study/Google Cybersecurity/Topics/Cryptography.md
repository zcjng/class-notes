---
course: Coursera Google Cybersecurity
topic: Cryptography
tags:
  - google
  - cybersecurity
---

# 🔐 Cryptography

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-23|Related day: 2026-10-23]]

Cryptography keeps **PII and sensitive data private** across open networks.

```
📄 Plaintext --Encryption + Key--> 🔒 Ciphertext
🔒 Ciphertext --Decryption + Key--> 📄 Plaintext
```

---

## 📖 Key Terms

| Term | Definition |
| :--- | :--- |
| **Plaintext** | Readable, unencrypted data |
| **Ciphertext** | Scrambled, unreadable data |
| **Encryption** | Converts plaintext → ciphertext |
| **Decryption** | Converts ciphertext → plaintext |
| **Cipher** | The **algorithm** used to encrypt data |
| **Key** | The value/mechanism needed to decrypt (e.g. the shift amount) |

---

## 🏛️ The Caesar Cipher

Shifts each letter of the alphabet by a set number of spaces.

> [!EXAMPLE] Shift of 3
> `A → D`, `B → E`, `C → F` — so `HELLO` → `KHOOR`

### ⚠️ Vulnerabilities

- **Brute force** — only 25 possible shifts in a small alphabet, so it can be cracked **without the key**.
- **Single-key risk** — if the key leaks, everything is exposed.

> [!TIP] Key management best practices
> - Keep keys **private**.
> - Transmit keys **separately** from the encrypted message.

> [!QUOTE] Fun aside
> A key of `4` isn't "best" just because it was randomly chosen — once it's known, it's no longer secret. ([xkcd "Random Number"](https://xkcd.com/221/))

---

## 🔗 Related

- [[Data Governance]]
- [[Information Privacy]]
- [[Security Controls]]
- [[VPNs]]
- [[Wi-Fi Security]]
