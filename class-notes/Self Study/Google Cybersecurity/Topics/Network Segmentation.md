---
course: Coursera Google Cybersecurity
topic: Networking
tags:
  - google
  - cybersecurity
---

# 🧩 Network Segmentation

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-17|Related day: 2026-10-17]]

---

## 📌 What is Network Segmentation?

A **security technique** that divides a unified network into **distinct, smaller segments**.

- **Granular security** — each segment maintains its own unique **access permissions and security rules**.
- **Blast radius mitigation** — acts as a containment barrier; if a segment is compromised, administrators can **isolate it** to prevent lateral movement to the rest of the network.
- **Examples:**
    - *Hospitality:* a hotel hosting an unsecured public guest Wi-Fi network completely isolated from the encrypted staff network.
    - *Education:* a university separating traffic into a faculty subnet and a student subnet.

---

## 🗂️ Classifications of Security Zones

### 🔴 1. Uncontrolled Zone
Any external network that falls completely **outside** the organization's control, monitoring, or administration. Primary example: **the public internet**.

### 🟢 2. Controlled Zone
The collective subnets and internal networks **protected from the uncontrolled zone**, structurally divided into three layers:

| Layer | Nature | Purpose |
| :--- | :--- | :--- |
| **A. DMZ** (Demilitarized Zone) | The network **perimeter**, outermost layer of the controlled zone | Houses public-facing services that must securely interact with the internet — web servers, public proxies, external DNS, email/file-transfer servers |
| **B. Internal Network** | The core corporate layer behind the DMZ | Private servers, endpoints, and standard business data the organization must protect from public exposure |
| **C. Restricted Zone** | The deepest, most isolated layer within the internal network | Highly confidential information and critical assets; access restricted to specific employees with elevated privileges |

---

## 🧱 Perimeter Architecture & Firewall Deployment

To maximize **defense-in-depth**, firewalls isolate each zone:

1. **External Firewall** — scrubs incoming traffic from the uncontrolled internet into the DMZ.
2. **Internal Firewall** — between DMZ and internal network; blocks threats that penetrated the outer perimeter from spreading deeper.
3. **Restricted Firewall** — guards the restricted zone, ensuring even compromised internal devices cannot access highly confidential data.

![[Pasted image 20261017080635.png]]

### 🛡️ Security Analyst Operations

- **Policy regulation** — implement and enforce access control policies across all zone-boundary firewalls.
- **Traffic restriction** — tightly restrict allowed IP addresses and communication ports.
- **Example rule:** configure perimeter firewalls to only allow inbound **HTTPS (port 443)** traffic directly to public web servers in the DMZ, dropping all other traffic types.

---

## 🧱 Subnetting & CIDR

**Subnetting** is the process of taking one large network and **dividing it into several smaller, organized groups** called **subnets** — defined by the unique combination of the **IP address** and the **network mask** assigned to every device.

### Classful → Classless Addressing

- **Classful addressing (1980s)** — grouped IPs into strict categories (**Class A through E**), each with a rigid, limited pool that rapidly depleted in the 1990s internet boom.
- **Classless Inter-Domain Routing (CIDR)** — replaced classful networking, expanding efficiency and availability of the modern IPv4 space.

**CIDR** is a flexible method of assigning subnet masks to split networks into customizable chunks — enabling precise segmentation, smaller routing tables, and no wasted empty address blocks.

**CIDR notation** — a trailing slash + number, e.g. `198.51.100.0/24`:
- The number after the slash (**24**) is the **IP network prefix** — how many bits are dedicated to the network identifier.
- A `/24` encompasses the entire block of **256 sequential IPs**: `198.51.100.0` – `198.51.100.255`.

---

## 🔗 Related

- [[Network Fundamentals]]
- [[Firewalls]]
- [[Proxy Servers]]
- [[IP Addressing & ARP]]
- [[Security Controls]]
