# Course 4 ~ Module 1
# Inside the Operating System

**Topic:** The operating system at work
**Source:** Google Cybersecurity Professional Certificate

---

## Overview
This video explains how operating systems (OS) work, focusing on the processes that occur from the moment a computer is powered on to when a user interacts with an application.

## Booting the Computer
* When you press the power button, the computer's hardware is activated, initiating the booting process.
* A special microchip, either Basic Input/Output System) BIOS or (Unified Extensible Firmware Interface) UEFI, contains instructions to load the bootloader, which then starts the operating system.

## User Interaction with the OS
* Users interact with applications to perform tasks, and these applications send requests to the operating system.
* The OS interprets these requests and directs them to the appropriate hardware components, which then send information back to the OS and subsequently to the application.

## Security Implications
* Understanding these processes is crucial for security analysts, as vulnerabilities can exist in the booting process, such as in the BIOS, which is often not scanned by antivirus software.
* Tracing the flow of information between applications, the OS, and hardware is essential for investigating security events and identifying where issues may have occurred.




OS as an Energy Manager

The OS ensures efficient resource allocation, similar to how a person manages energy for different tasks.

Tasks like antivirus scans require more "energy" (resources) than simpler applications like a calculator.

The OS as an Orchestra Conductor

The OS acts as a conductor, directing various computer components (like instruments) to work in harmony.

It manages memory, storage, and CPU usage, ensuring programs receive the resources they need.

Resource Monitoring and Troubleshooting

The OS allocates and de-allocates resources for competing programs and processes.

Tools like the task manager allow users to monitor resource usage, which is crucial for troubleshooting and identifying issues like malware


# Virtualization Technology

## Overview
Operating systems can run inside standalone, software-controlled isolated containers. This architectural separation is achieved through virtualization layers rather than deploying isolated physical infrastructure.


### Key Acronyms & Components
- ==**VM (Virtual Machine)**==: A fully software-defined, virtualized replica of a physical desktop, server, or computer.
- ==**vCPU (Virtual Central Processing Unit)**==: The software-allocated execution engine that simulates the primary processing core of a physical machine.
- ==**Virtual Storage**==: Abstracted, software-defined storage volumes configured to mirror local hard drives or solid-state arrays.
- ==**Virtual Hardware**==: The holistic collection of simulated components (including networks, RAM, and chips) constructed entirely through software definitions.
-

# Understanding Virtualization

==**Virtualization**== is a technology that allows you to create multiple simulated environments from a single physical hardware system. Instead of dedicating an entire physical computer to just one task, virtualization breaks the physical system down into independent, software-defined pieces so it can do the work of several computers at once.

> [!TIP] The Apartment Analogy
> Think of a physical computer as an **apartment building**. The structure, plumbing, and electricity represent the physical hardware. Virtualization creates individual, isolated **apartments** (Virtual Machines) inside that building. Each apartment shares the same underlying foundation, but the residents live completely independent lives and cannot see into each other's spaces.

---

## Core Structural Components

- ==**OS (Operating System)**==: The foundational software that manages computer hardware and software resources (e.g., Windows, macOS, Linux). In virtualization, we break this down into two roles:
    - **Host OS**: The primary operating system running directly on the physical hardware.
    - **Guest OS**: The isolated operating system running inside a specific virtual machine. For example, you can run a Linux Guest OS inside a Windows Host OS.
- ==**Hypervisor**==: The fundamental engine of virtualization. It is a specialized software layer that sits between the physical hardware and your virtual machines.

## Hardware Allocation: Physical vs. Virtual

The ==**Hypervisor**== takes physical resources and carves them up into isolated, virtualized equivalents for each Guest OS:

| Core Term | Physical Definition | Virtual Definition |
| :--- | :--- | :--- |
| ==**CPU**== (Central Processing Unit) | The physical microchip on the motherboard that executes computing calculations and instructions. | **vCPU (Virtual CPU)**: A slice of processing time scheduled by the hypervisor, allowing multiple systems to share the physical core. |
| ==**RAM**== (Random Access Memory) | The physical memory sticks that provide a fast, temporary working space for active applications. | **Virtual Memory**: A boundary-enforced partition of physical RAM. The hypervisor creates digital walls so one VM cannot access another's memory. |

