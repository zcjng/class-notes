---
course: Coursera Google Cybersecurity
topic: Operating Systems
tags:
  - google
  - cybersecurity
---

# 🧊 Virtualization

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

**Virtualization** is a technology that lets you create **multiple simulated environments from a single physical hardware system**. Instead of dedicating an entire physical computer to one task, it breaks the system into independent, software-defined pieces so it can do the work of several computers at once.

> [!TIP] The apartment analogy
> Think of a physical computer as an **apartment building**: the structure, plumbing, and electricity are the physical hardware. Virtualization creates individual, isolated **apartments (VMs)** inside. Each shares the same foundation, but residents live independently and can't see into each other's spaces.

---

## 🔑 Key Components

| Term | Definition |
| :--- | :--- |
| **VM (Virtual Machine)** | A fully software-defined, virtualized replica of a physical desktop, server, or computer. |
| **vCPU (Virtual CPU)** | The software-allocated execution engine that simulates the primary processing core of a physical machine. |
| **Virtual Storage** | Abstracted, software-defined storage volumes configured to mirror local hard drives or solid-state arrays. |
| **Virtual Hardware** | The holistic collection of simulated components (networks, RAM, chips) built entirely through software definitions. |

---

## 🧱 Core Structural Components

- **OS (Operating System)** — two roles in virtualization:
    - **Host OS** — the primary OS running directly on the physical hardware.
    - **Guest OS** — the isolated OS running inside a VM (e.g. a Linux Guest OS inside a Windows Host OS).
- **Hypervisor** — the fundamental engine of virtualization: a specialized **software layer between the physical hardware and the virtual machines**.

## ⚖️ Hardware Allocation: Physical vs. Virtual

The hypervisor carves physical resources into isolated, virtualized equivalents for each Guest OS:

| Core Term | Physical | Virtual |
| :--- | :--- | :--- |
| **CPU** | Physical microchip on the motherboard that executes calculations and instructions | **vCPU** — a slice of processing time scheduled by the hypervisor, letting multiple systems share the physical core |
| **RAM** | Physical memory sticks providing fast, temporary workspace for active applications | **Virtual memory** — a boundary-enforced partition of physical RAM; the hypervisor builds digital walls so one VM cannot access another's memory |

---

## 🛡️ Security Relevance

- VMs provide **isolation** for testing malware and untrusted code, and are **disposable** (snapshot/revert).
- A hypervisor flaw can allow **virtualization escape** into the host. → see [[Vulnerability Assessment]]

---

## 🔗 Related

- [[Inside the Operating System]]
- [[Operating Systems]]
- [[Cloud Computing]]
- [[Vulnerability Assessment]]
---
course: Coursera Google Cybersecurity
topic: Operating Systems
tags:
  - google
  - cybersecurity
---

# 🧊 Virtualization

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

**Virtualization** is a technology that lets you create **multiple simulated environments from a single physical hardware system**. Instead of dedicating an entire physical computer to one task, it breaks the system into independent, software-defined pieces so it can do the work of several computers at once.

> [!TIP] The apartment analogy
> Think of a physical computer as an **apartment building**: the structure, plumbing, and electricity are the physical hardware. Virtualization creates individual, isolated **apartments (VMs)** inside. Each shares the same foundation, but residents live independently and can't see into each other's spaces.

---

## 🔑 Key Components

| Term | Definition |
| :--- | :--- |
| **VM (Virtual Machine)** | A fully software-defined, virtualized replica of a physical desktop, server, or computer. |
| **vCPU (Virtual CPU)** | The software-allocated execution engine that simulates the primary processing core of a physical machine. |
| **Virtual Storage** | Abstracted, software-defined storage volumes configured to mirror local hard drives or solid-state arrays. |
| **Virtual Hardware** | The holistic collection of simulated components (networks, RAM, chips) built entirely through software definitions. |

---

## 🧱 Core Structural Components

- **OS (Operating System)** — two roles in virtualization:
    - **Host OS** — the primary OS running directly on the physical hardware.
    - **Guest OS** — the isolated OS running inside a VM (e.g. a Linux Guest OS inside a Windows Host OS).
- **Hypervisor** — the fundamental engine of virtualization: a specialized **software layer between the physical hardware and the virtual machines**.

## ⚖️ Hardware Allocation: Physical vs. Virtual

The hypervisor carves physical resources into isolated, virtualized equivalents for each Guest OS:

| Core Term | Physical | Virtual |
| :--- | :--- | :--- |
| **CPU** | Physical microchip on the motherboard that executes calculations and instructions | **vCPU** — a slice of processing time scheduled by the hypervisor, letting multiple systems share the physical core |
| **RAM** | Physical memory sticks providing fast, temporary workspace for active applications | **Virtual memory** — a boundary-enforced partition of physical RAM; the hypervisor builds digital walls so one VM cannot access another's memory |

---

## 🛡️ Security Relevance

- VMs provide **isolation** for testing malware and untrusted code, and are **disposable** (snapshot/revert).
- A hypervisor flaw can allow **virtualization escape** into the host. → see [[Vulnerability Assessment]]

---

## 🔗 Related

- [[Inside the Operating System]]
- [[Operating Systems]]
- [[Cloud Computing]]
- [[Vulnerability Assessment]]
