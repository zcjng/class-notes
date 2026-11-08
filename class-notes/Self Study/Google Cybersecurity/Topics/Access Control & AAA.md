---
course: Coursera Google Cybersecurity
topic: Access Control
tags:
  - google
  - cybersecurity
---

# 🚪 Access Control & AAA

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-01|Related day: 2026-11-01]]

Access control exists to maintain the **CIA triad**. It is broken into the **AAA** framework: **Authentication → Authorization → Accounting**.

---

## 1️⃣ Authentication — *who are you?*

Proven through one of three **factors**:

| Factor | Type | Example |
| :--- | :--- | :--- |
| **Knowledge** | Something you **know** | Password, PIN |
| **Ownership** | Something you **have** | OTP device, phone, smart card |
| **Characteristic** | Something you **are** | Fingerprint, facial scan |

### Single Sign-On (SSO)

Combines many logins into one identity verification step. Three organizational reasons to adopt it:

1. **Better UX** — fewer usernames and passwords to remember.
2. **Lower cost** — streamlined management of connected services.
3. **Better security** — fewer access points for attackers to target.

**How it works:** a trusted third party (the **identity provider**) proves identity and issues an encrypted **access token** to the service provider.

| Protocol | Where it's used |
| :--- | :--- |
| **LDAP** (Lightweight Directory Access Protocol) | Mostly **on-premises** directory lookups |
| **SAML** (Security Assertion Markup Language) | Mostly **off-premises / cloud** |

> [!WARNING] Don't rely on SSO alone
> If SSO depends on a single authentication factor, compromising that one factor unlocks everything at once. Pair it with **MFA**.

### MFA

Requires **two (2FA)** or **three (3FA)** *distinct* factors — e.g. password (know) + SMS OTP (have) + fingerprint (are).

### Web authentication patterns

| Mechanism | Risk / benefit |
| :--- | :--- |
| **HTTP Basic Auth** | Sends username/password as **plain text** → vulnerable to interception |
| **HTTPS** | Encrypts the transmission in transit → the correct baseline |
| **OAuth** | Open-standard **authorization** protocol; shares designated access **without** exposing user passwords |
| **API tokens** | Encrypted blocks carrying identity + permission details between devices and servers |

---

## 2️⃣ Authorization — *what may you do?*

Two core principles:

- **Least privilege** — grant only the access needed, only for as long as needed. *Limits access.*
- **Separation of duties** — split critical responsibilities across people/systems so no one holds too much control. *Divides responsibility.*

### The three authorization frameworks

| Framework | Model | Example |
| :--- | :--- | :--- |
| **MAC** (Mandatory Access Control) | Strictest. Need-to-know only; access granted manually by a **central authority**. Also called *non-discretionary*. | Military, law enforcement, government — access via chain of command |
| **DAC** (Discretionary Access Control) | The **data owner** decides who gets access | Sharing a Google Drive folder as viewer/commenter/editor |
| **RBAC** (Role-Based Access Control) | Access determined by the user's **role** | Marketing can see analytics but not network administration |

---

## 3️⃣ Accounting — *what did you do?*

Accounting monitors **access logs** to track sessions, uncover incidents, and flag unauthorized activity.

- A **session** begins on access and generates a unique **session ID** identifying the user and device until logout or timeout.
- **Session cookies** are exchanged between device and server to validate the session without resending credentials each time.
- **Session hijacking** — an attacker steals the session ID/cookie and impersonates the user. Monitoring access logs is how you spot it.

---

## 🆔 IAM — Identity and Access Management

**IAM** is the collection of processes and technologies for managing digital identities. Both AAA and IAM aim to grant the **right user** access to the **right resources** at the **right time** for the **right reasons**.

- A "user" can be a person, a **device**, or **software**.
- **User provisioning** — creating and maintaining a user's digital identity.
- **Deprovisioning** — removing access rights when they're no longer needed. An easy-to-miss but critical analyst duty.

---

## 🔗 Related

- [[Cloud Security]] — IAM in cloud environments
- [[Information Privacy]] — the principle of least privilege
- [[Security Controls]]
- [[Encryption & PKI]]
- [[Hashing & Password Storage]]
- [[OWASP Security Principles]]
