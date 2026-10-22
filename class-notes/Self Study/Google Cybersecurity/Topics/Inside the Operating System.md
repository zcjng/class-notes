---
course: Coursera Google Cybersecurity
topic: Operating Systems
tags:
  - google
  - cybersecurity
---

# ⚙️ Inside the Operating System

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

How the OS works — from the moment the computer powers on to the moment a user interacts with an application.

---

## 🔌 Booting the Computer

- Pressing the power button activates the hardware and initiates **booting**.
- A special microchip — **BIOS** (Basic Input/Output System) or **UEFI** (Unified Extensible Firmware Interface) — holds instructions to load the **bootloader**, which then starts the operating system.

## 🖱️ User Interaction with the OS

- Users interact with **applications**, which send requests to the **operating system**.
- The OS interprets the requests and directs them to the appropriate **hardware components**, which send information back to the OS and then to the application.

> [!WARNING] Security implications
> Vulnerabilities can exist in the **booting process** — e.g. in the **BIOS**, which is often **not scanned by antivirus software**. Tracing the flow of information between applications, the OS, and hardware is essential for investigating security events and locating where things went wrong.

---

## 🎛️ The OS as a Resource Manager

| Analogy | Meaning |
| :--- | :--- |
| **Energy manager** | Ensures efficient resource allocation — heavy tasks like antivirus scans need more "energy" (resources) than a calculator does. |
| **Orchestra conductor** | Directs all computer components to work in harmony; manages **memory, storage, and CPU usage** so programs get the resources they need. |

- The OS **allocates and de-allocates** resources for competing programs and processes.
- Tools like the **task manager** let users monitor resource usage — crucial for troubleshooting and spotting issues like **malware**.

---

## 🔗 Related

- [[Operating Systems]]
- [[Virtualization]]
- [[Linux File System]]
- [[Vulnerability Assessment]]
