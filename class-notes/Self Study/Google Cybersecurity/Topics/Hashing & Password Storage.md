---
course: Coursera Google Cybersecurity
topic: Cryptography
tags:
  - google
  - cybersecurity
---

# #️⃣ Hashing & Password Storage

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

A **hash function** is a **one-way** process: it turns any input into a fixed-length **digest**. There is no key and no going back — which is exactly what makes it different from [[Encryption & PKI|encryption]].

---

## 🧬 What Makes a Hash Useful

- **Fixed length** — regardless of input size, the digest is always the same length.
- **Deterministic** — the same input always produces the same digest.
- **Avalanche effect** — changing a single character (even one line of code) produces a *completely different* digest.
- **One-way** — you cannot reverse a digest back into the original data.

### Two core security uses

| Use | What it proves |
| :--- | :--- |
| **Data integrity** | Files and applications are accurate and consistent — any tampering is exposed by a mismatched hash |
| **Non-repudiation** | Authenticity can't be denied once data is validated against its original hash |

> [!EXAMPLE] Analyst workflow
> Generate a hash of a file → compare it against a known-good hash, or cross-reference it on **VirusTotal** to see if it matches known malware.

---

## 📜 The Algorithm Families

### MD5 (Message Digest 5)

- Developed by **Ronald Rivest** (MIT) in the early 1990s to verify file transfers.
- Produces a **128-bit** value — written as **32 characters**.
- **Broken:** considered insecure today.

### Hash collisions

Because every possible input is squeezed into a fixed-size output, different inputs can produce the **same hash** — a **hash collision**.

- A collision is like copying someone's identity: you can fraudulently impersonate authentic data.
- MD5's short output made collision attacks practical, which is why it is no longer trusted for security.

### SHA (Secure Hashing Algorithm)

SHA replaced MD5. The number in the name is the digest size in bits:

- **SHA-1** — 160-bit (deprecated)
- **SHA-224**
- **SHA-256**
- **SHA-384**
- **SHA-512**

> [!NOTE] NIST approves all SHA variants **except SHA-1**.

---

## 🔐 Secure Password Storage

Passwords are typically stored in a database mapped to a username. On login, the server retrieves the stored value and compares.

- **Plaintext storage = catastrophic.** A database breach hands attackers every credential.
- **Hashing solves this**, because hashes can't be reversed — a stolen digest isn't a usable password.

### Two attacks against hashed passwords

| Attack | How it works | Defence |
| :--- | :--- | :--- |
| **Rainbow table** | A pre-generated file of hash → plaintext pairs, like a dictionary of weak passwords. Look up the hash, get the password. | **Salting** |
| **Pass the hash** | Reusing a stolen, **unsalted** hash to trick an authentication system into creating a session — no cracking needed | Salting + MFA |

### Salting

A **salt** is a random string of characters added to data *before* hashing.

- Makes each digest unique, even for users with identical passwords.
- Defeats rainbow tables, because the table would need to be regenerated per salt.
- See also [[Vulnerability Assessment]] for the defensive-control framing.

---

## 🔗 Related

- [[Encryption & PKI]]
- [[Cryptography]]
- [[Vulnerability Assessment]]
- [[Data Governance]]
- [[Indicators of Compromise]]
---
course: Coursera Google Cybersecurity
topic: Cryptography
tags:
  - google
  - cybersecurity
---

# #️⃣ Hashing & Password Storage

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

A **hash function** is a **one-way** process: it turns any input into a fixed-length **digest**. There is no key and no going back — which is exactly what makes it different from [[Encryption & PKI|encryption]].

---

## 🧬 What Makes a Hash Useful

- **Fixed length** — regardless of input size, the digest is always the same length.
- **Deterministic** — the same input always produces the same digest.
- **Avalanche effect** — changing a single character (even one line of code) produces a *completely different* digest.
- **One-way** — you cannot reverse a digest back into the original data.

### Two core security uses

| Use | What it proves |
| :--- | :--- |
| **Data integrity** | Files and applications are accurate and consistent — any tampering is exposed by a mismatched hash |
| **Non-repudiation** | Authenticity can't be denied once data is validated against its original hash |

> [!EXAMPLE] Analyst workflow
> Generate a hash of a file → compare it against a known-good hash, or cross-reference it on **VirusTotal** to see if it matches known malware.

---

## 📜 The Algorithm Families

### MD5 (Message Digest 5)

- Developed by **Ronald Rivest** (MIT) in the early 1990s to verify file transfers.
- Produces a **128-bit** value — written as **32 characters**.
- **Broken:** considered insecure today.

### Hash collisions

Because every possible input is squeezed into a fixed-size output, different inputs can produce the **same hash** — a **hash collision**.

- A collision is like copying someone's identity: you can fraudulently impersonate authentic data.
- MD5's short output made collision attacks practical, which is why it is no longer trusted for security.

### SHA (Secure Hashing Algorithm)

SHA replaced MD5. The number in the name is the digest size in bits:

- **SHA-1** — 160-bit (deprecated)
- **SHA-224**
- **SHA-256**
- **SHA-384**
- **SHA-512**

> [!NOTE] NIST approves all SHA variants **except SHA-1**.

---

## 🔐 Secure Password Storage

Passwords are typically stored in a database mapped to a username. On login, the server retrieves the stored value and compares.

- **Plaintext storage = catastrophic.** A database breach hands attackers every credential.
- **Hashing solves this**, because hashes can't be reversed — a stolen digest isn't a usable password.

### Two attacks against hashed passwords

| Attack | How it works | Defence |
| :--- | :--- | :--- |
| **Rainbow table** | A pre-generated file of hash → plaintext pairs, like a dictionary of weak passwords. Look up the hash, get the password. | **Salting** |
| **Pass the hash** | Reusing a stolen, **unsalted** hash to trick an authentication system into creating a session — no cracking needed | Salting + MFA |

### Salting

A **salt** is a random string of characters added to data *before* hashing.

- Makes each digest unique, even for users with identical passwords.
- Defeats rainbow tables, because the table would need to be regenerated per salt.
- See also [[Vulnerability Assessment]] for the defensive-control framing.

---

## 🔗 Related

- [[Encryption & PKI]]
- [[Cryptography]]
- [[Vulnerability Assessment]]
- [[Data Governance]]
- [[Indicators of Compromise]]
