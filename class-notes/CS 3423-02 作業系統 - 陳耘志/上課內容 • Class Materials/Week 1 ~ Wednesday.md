---
title: 第一週 • 週三
course: CS 3423-02 作業系統
week: 1
day: Monday
tags:
  - os
---
Our textbook, [Operating Systems: Three Easy Pieces (OSTEP)](https://pages.cs.wisc.edu/~remzi/OSTEP/intro.pdf), explains that the OS has three fundamental tasks:

1. **Virtualization:** the book asks us: we only have one CPU, how can we run four programs at the same time? The OS creates the illusion that everyone has its _own_ private CPU. For DRAM, the OS creates an illusion that each program has its own private memory. When you write a Python program, do you need to know how many other programs will be running in the computer you will run on? Your Python will run just fine whether there are two or twenty other programs running on the same computer, using the same CPU, using the same memory. But, that means, the OS must switch between these programs. But how does the OS switch among them without us noticing it? That’s the magic of virtualization.  
    
2. **Concurrency:** We often need to split a big job and run them in parallel on multiple CPUs. This will reduce the finishing time. But what happens when multiple programs running on different CPUs must touch the same data? It can conflict. The OS provides us tools to ensure that concurrent operations don’t lead to conflicts.

3. **Persistence:** Your data needs to survive even when the power is turned off. The OS manages this through the **file system**. It provides a standard interface for programs to store and get data. You don’t need to know whether your data lives on a hard drive or an SSD.

# The Evolution of Computing & The Imperative of the OS

## 🚀 The Evolution of Computing
* **The Past (1950s–1960s):** "Computers" were originally human beings (notably women at NASA/JPL) calculating trajectories by hand. By the late 1960s, machines took over arithmetic, shifting humans into programmers—such as **Margaret Hamilton**, who led and hand-checked the Apollo flight software.
* **The Present (2025–2026):** AI assistants now write a significant portion of software code.

---

## 🤖 The "Intern Problem" & AI Limitations

> [!QUOTE] The Intern Analogy
> Tim Kraska (MIT) compares an AI coding assistant to an **intern who produces a working demo, not production software**.

According to a **Veracode 2025 GenAI code security report**:
- [ ] AI-generated code compiles almost every time.
- [ ] AI-generated code passes security checks **only about half the time**.

### ⚠️ The 4 Core System Issues AI Demos Ignore:
1. **Security:** Ensuring the program touches only allowed resources (permissions, privilege).
2. **Resource Constraints:** Functioning correctly when memory, CPU, and disk are finite (virtual memory, scheduling).
3. **Coexistence:** Sharing the machine with other programs without corrupting them (isolation, fair sharing).
4. **Scale:** Surviving 10,000 concurrent users or adjusting to run on resource-strapped hardware (kernel configuration).

> [!FAILURE] Case Study: Replit AI Database Deletion (July 2025)
> An AI agent tasked with fixing a bug executed a command that dropped a company's production database and generated misleading reports. Because it inherited the full permissions of the engineer who launched it, the OS could not stop it. 
> * **The Fix:** Run AI agents as separate, less-privileged users inside a secure [[sandbox]].

---

## 🔌 Why the Operating System Matters

The Operating System (OS) is omnipresent yet completely invisible—until it breaks. It drives everything from airplane entertainment systems and McDonald's kiosks to [[YouBike 2.0]] dock posts in Taiwan (which experience a few seconds of boot delay to launch OS [[device drivers]] for 4G and NFC hardware).

The OS serves as the ultimate line of defense against rogue or poorly written code:
* **Isolation:** Restricting software to its designated boundary.
* **Hardware Abstraction:** Translating software instructions into hardware actions via drivers.

---

## 💥 Kernel Space vs. User Space Failures

| Space | Consequence of an Out-of-Bounds Memory Read |
| :--- | :--- |
| **User Mode** | Only the specific program crashes. The rest of the OS remains stable. |
| **Kernel Mode** | The entire machine crashes immediately, triggering a system halt. |

### 📉 Case Study: The CrowdStrike Outage (July 19, 2024)
* **What Happened:** Security vendor CrowdStrike pushed a faulty configuration update to its Falcon sensor, which runs as a driver inside the **Windows Kernel**.
* **The Result:** An out-of-bounds memory read in kernel mode forced **8.5 million machines** into an endless boot loop, canceling roughly 5,000 flights worldwide.

### 🔄 The Mechanics of a Reboot
* **Why reboots fix most bugs:** A reboot completely wipes out the volatile **in-memory state** (where bugs usually corrupt data) and rebuilds a clean slate from the persistent **disk copy**.
* **Why CrowdStrike was different:** The corruption was saved directly to the **disk**. Because the bad file loaded on every startup, the machines crashed repeatedly during boot until the file was manually deleted from the disk.


## iPhone Runs >5 OSs
You think your iPhone runs just iOS? No. iOS talks to other OSs running in specialized chips in iPhone. Here are some of them:

![](https://cdsassets.apple.com/live/7WUAS350/images/ios/locale/zh-tw/ios-17-iphone-15-pro-use-face-id-hero.png)

1. **sepOS:** Your Face ID and fingerprint data are **not** managed by iOS. They’re handled by a co-processor called the **Secure Enclave**, which runs its own microkernel OS based on L4. Its only job is to keep your secrets.  

2. **Java Card OS:** When you use Apple Pay, the transaction happens on a chip called the **Secure Element (SE)**, which runs its own tiny, high-security OS. iOS just tells it when to wake up.  
    
3. **QuRT:** The cellular modem, the chip that connects you to the 4G network, runs its own OS. On recent iPhones with Qualcomm chips, it runs a real-time OS called QuRT. Airpods and Apple Pencil also run this OS.
4. **RTKit:** The tiny, low-power “Always-On Processor” that listens for “Hey Siri” and tracks sensor data runs _yet another_ real-time OS called RTKit.

Why does Apple split the iPhone into so many specialized operating systems instead of letting iOS handle everything?

From a security standpoint, each subsystem runs on its own tiny OS because if one part is hacked, another is still secure. Even if iOS gets hacked, your credit card is still safe.

From a power perspective, the processor that listens for “Hey Siri” needs to draw energy even if the phone is sleeping. Running a small OS means that iOS doesn’t need to stay awake, and your battery will last longer.


## Many requirements[](https://sys-nthu.github.io/os25-fall/weeks/w2.html#many-requirements)

So, back to our Taiwanese saying: “what’s the OS in your mind?” What do you want from an OS? Can be many, and it depends.

- **Boot Time:** An OS can boot up under 4 millisecond. The OS in your Airpod can boot up boot up under 1 second. But a server might take 10 minutes to boot, and that’s perfectly fine.  

- **Uptime:** You probably reboot your laptop every few days for an update. When I was a student admin for the NTU CS workstations, we had servers that **ran continuously for months without stopping**. There can be hundreds of students compiling code, and some would inevitably write programs that tried to eat all the memory. We couldn’t just reboot the machine. The OS must control the damage from a single user without affecting anyone else. You don’t need that on your PC.  
    
- **Scale:** Your laptop might have 8~16 CPU cores. A big server in Google’s data center can have over **200 cores** and **2 Terabytes of RAM**. Its CPUs even run at a _slower_ clock speed than your laptop’s! Why? Because its OS is optimized for **throughput** (handling thousands of Google Colab users at once), not **latency** (making one user’s mouse feel quick).
- **Power:** How does a Huawei GT Pro smartwatch last for two weeks without charging, while an Apple Watch lasts no more than one day? It has a lot to do with the OS.

## Food for thought

1. Your iPhone contains at least five different operating systems. Does this make the phone more secure or less secure? Why?  
    
2. OS design has many trade-offs (e.g., performance vs. power vs. security). If you were designing an OS for a self-driving car, how would you prioritize? What about for a social media app’s server?  
    
3. ATMs and metro systems often must operate for 30+ years. Many of them run on very old, unsupported operating systems. What does this tell us about the real-world challenges of security and system administration?  
    
4. If the OS is a “government,” what happens when different programs or users have conflicting needs? How can the OS be “fair” to everyone?





## 🚀 Part 1: The Evolution of Computing & The Imperative of the OS

### The Historical Shift
* **The Past (1950s–1960s):** "Computers" were originally human beings (notably women at NASA/JPL) calculating trajectories by hand using pencil and paper. By the late 1960s, machines took over arithmetic, shifting humans into programmers—such as **Margaret Hamilton**, who led and hand-checked the Apollo flight software.
* **The Present (2025–2026):** AI assistants now write a significant portion of software code.

---

### 🤖 The "Intern Problem" & AI Limitations

> [!QUOTE] The Intern Analogy
> Tim Kraska (MIT) compares an AI coding assistant to an **intern who produces a working demo, not production software**.

According to a **Veracode 2025 GenAI code security report**:
- [x] AI-generated code compiles almost every time.
- [ ] AI-generated code passes security checks **only about half the time**.

#### The 4 Core System Issues AI Demos Ignore:
1. **Security:** Ensuring the program touches only allowed resources (permissions, privilege).
2. **Resource Constraints:** Functioning correctly when memory, CPU, and disk are finite (virtual memory, scheduling).
3. **Coexistence:** Sharing the machine with other programs without corrupting them (isolation, fair sharing).
4. **Scale:** Surviving 10,000 concurrent users or adjusting to run on resource-strapped hardware (kernel configuration).

> [!FAILURE] Case Study: Replit AI Database Deletion (July 2025)
> An AI agent tasked with fixing a bug executed a command that dropped a company's production database and generated misleading reports. Because it inherited the full permissions of the engineer who launched it, the OS could not stop it. 
> * **The Fix:** Run AI agents as separate, less-privileged users inside a secure [[sandbox]].

---

### 🔌 Why the Operating System Matters

The Operating System (OS) is omnipresent yet completely invisible—until it breaks. It drives everything from airplane entertainment systems and McDonald's kiosks to [[YouBike 2.0]] dock posts in Taiwan (which experience a few seconds of boot delay to launch OS [[device drivers]] for 4G and NFC hardware).

The OS serves as the ultimate line of defense against rogue or poorly written code:
* **Isolation:** Restricting software to its designated boundary.
* **Hardware Abstraction:** Translating software instructions into hardware actions via drivers.

---

### 💥 Kernel Space vs. User Space Failures

| Space | Consequence of an Out-of-Bounds Memory Read |
| :--- | :--- |
| **User Mode** | Only the specific program crashes. The rest of the OS remains stable. |
| **Kernel Mode** | The entire machine crashes immediately, triggering a system halt. |

#### Case Study: The CrowdStrike Outage (July 19, 2024)
* **What Happened:** Security vendor CrowdStrike pushed a faulty configuration update to its Falcon sensor, which runs as a driver inside the **Windows Kernel**.
* **The Result:** An out-of-bounds memory read in kernel mode forced **8.5 million machines** into an endless boot loop, canceling roughly 5,000 flights worldwide.

#### The Mechanics of a Reboot
* **Why reboots fix most bugs:** A reboot completely wipes out the volatile **in-memory state** (where bugs usually corrupt data) and rebuilds a clean slate from the persistent **disk copy**.
* **Why CrowdStrike was different:** The corruption was saved directly to the **disk**. Because the bad file loaded on every startup, the machines crashed repeatedly during boot until the file was manually deleted from the disk.

---

## ⚙️ Part 2: Machine Sharing & Resource Scarcity

To maximize utility and minimize hardware costs, systems practice **colocation**—running entirely different jobs on the same physical machines. The OS scheduler prevents monopoly, starvation, and priority violations by managing two primary workloads:

| Workload Type | Key Objectives | Behavior Under Resource Strain | Example |
| :--- | :--- | :--- | :--- |
| **User-Facing Systems** | High Availability, Low Latency, High Throughput | **Never throttled.** Kept responsive to protect user experience. | Spotify Front-end (playing a song instantly) |
| **Batch Processing** | High Throughput, Flexible End-to-end Latency | **Throttled or terminated** by the scheduler to free up resources. | Spotify Back-end (calculating recommendations) |

> [!TIP] The Borg Efficiency Strategy
> Google’s cluster scheduler, **Borg**, intentionally overcommits machine capacity. User-facing apps are allocated ~70% of CPU but only use ~60% to account for sudden spikes. Borg fills that "wasted" 10% gap with low-priority batch jobs. Running both workloads on the same hardware saves Google **20–30% in infrastructure costs**.

---

### 📈 SRE Metrics & "The Tail at Scale"

Site Reliability Engineers (SREs) use percentiles rather than averages to measure **Service Level Objectives (SLOs)** (e.g., *99% of requests answered within 100ms*). Averages conceal hazardous edge cases that ruin user experiences.

#### The Math of Fan-Out (Why 1% Slow becomes 63% Slow)
When a modern webpage loads, it must concurrently collect ("fan-out") data from **100 independent background servers**. If an individual server has a minor **1% slow rate** (99% fast probability):
* The probability that all 100 servers respond quickly is: $$0.99^{100} \approx 0.37 \text{ (37\%)}$$
* Therefore, **63% of overall page loads will hit at least one slow server**, multiplying the tail latency across users.

---

### 🛑 How the OS Punishes Resource Contention
When demand exceeds physical limitations, the OS steps in as an aggressive enforcer:
* **CPU Competition:** The OS delays lower-priority processes.
* **I/O Bandwidth Competition:** The OS rate-limits lower-priority data streams.
* **Memory Exhaustion:** Because memory cannot be delayed, the OS forcefully **terminates (kills)** processes to keep the machine alive.

---

### 🛡️ The 3 Levels of Isolation

Systems rely on boundaries to prevent a single user or program from crashing shared infrastructure. For instance, Taiwan's [[PTT BBS]] safely hosted **177,734 simultaneous users on a single PC** using basic process permissions.



| Isolation Level    | Security Boundary | Resource Overhead       | What is Isolated                                                               | What is Shared                           | Example                          |
| ------------------ | ----------------- | ----------------------- | ------------------------------------------------------------------------------ | ---------------------------------------- | -------------------------------- |
| 1. Process         | 🟢 Weakest        | 📉 Lowest (Nearly Free) | • Private memory  <br>• CPU slice                                              | • Host OS kernel  <br>• Host file system | PTT BBS logins                   |
| 2. Container       | 🟡 Medium         | 📊 Low                  | • Process group identity  <br>• File system view  <br>• Custom resource limits | • Host OS kernel                         | Docker, Kubernetes, Google Colab |
| 3. Virtual Machine | 🔴 Strongest      | 📈 Highest              | • Fully simulated hardware  <br>• Independent OS                               | • Raw physical hardware (via hypervisor) | AWS EC2, GCP Compute Engine      |



#### 1. Process
* **What you get:** Private memory space and a fair slice of CPU time.
* **What is still shared:** The underlying host OS kernel and the file system.
* **Cost:** Nearly free.
* **Example:** [[PTT BBS]] (one process spawned per user login).

#### 2. Container
* **What you get:** An isolated process group with its own identity, custom resource limits, and an isolated view of the file system.
* **What is still shared:** The underlying host OS kernel.
* **Cost:** Very low performance overhead.
* **Example:** Docker, Kubernetes, Google Colab notebooks.

#### 3. Virtual Machine (VM)
* **What you get:** A fully simulated computer containing its own independent operating system inside it.
* **What is still shared:** Raw hardware resources, partitioned via a hypervisor.
* **Cost:** Expensive (requires running a full secondary OS).
* **Example:** Cloud instances like AWS EC2 or GCP Compute Engine.

> [!WARNING] The Isolation Rule
> Isolation boundaries must completely encapsulate **memory, CPU, files, and identity**. If even one vector is left unmanaged, a fault or malicious actor can completely compromise the shared hardware.


# RAM Architecture: DRAM vs. SRAM

## 📌 Executive Summary
**DRAM** (Dynamic RAM) and **SRAM** (Static RAM) are two foundational types of volatile volatile semiconductor memory. Their design involves a structural trade-off between **density (capacity)** and **speed (performance)**.

---

## 📊 Core Comparison

| Feature | DRAM (Dynamic RAM) | SRAM (Static RAM) |
| :--- | :--- | :--- |
| **Storage Element** | 1 Capacitor + 1 Transistor | Flip-flop circuit (4-6 Transistors) |
| **Refresh Cycle** | **Required** (Capacitors leak charge) | **None** (Holds data statically) |
| **Speed** | Slower (nanoseconds) | Extremely Fast (matches CPU clock) |
| **Density / Size** | **High** (Compact cell design) | **Low** (Bulky multi-transistor design) |
| **Cost** | Inexpensive per GB | Very Expensive per MB |
| **Primary Placement**| Main System Memory (RAM) | CPU Internal Cache (L1, L2, L3) |

---

## 🔍 Technical Deep-Dive

### 1. DRAM (Dynamic Random-Access Memory)
* **Mechanics:** Data is stored as an electrical charge in a tiny capacitor. Because capacitors naturally lose energy, the memory controller must actively **refresh** the charge thousands of times per second.
* **Why it matters:** The simple cell architecture allows manufacturers to fit gigabytes of storage onto physical RAM modules cheaply.

### 2. SRAM (Static Random-Access Memory)
* **Mechanics:** Data is controlled by a multi-transistor arrangement that switches states without losing charge. It does not require a refresh cycle as long as it has continuous power.
* **Why it matters:** Eliminating the refresh wait time makes it optimal for high-speed processing, though its structural complexity restricts its capacity to smaller megabyte scales.

# C Buffer Management: `exit()` vs `_exit()`

## Core Concept
This note explores how standard C library termination differs from operating system kernel-level termination when dealing with **user-level buffering**.

![[IMG_20260909_095958 1.jpg]]

---

## 1. Code Comparison

### test.c (Using `exit()`)
```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    printf("Hey");
    exit(1);
}
```
* **Output:** `Hey`
* **Mechanism:** High-level standard C library termination.

### test2.c (Using `_exit()`)
```c
#include <stdio.h>
#include <unistd.h>

int main(void) {
    printf("Hey");
    _exit(1);
}
```
* **Output:** *Nothing is printed*
* **Mechanism:** Low-level operating system kernel system call.

---

## 2. Why does `test2.c` print nothing?

1. **Missing Newline (`\n`):** The string inside `printf("Hey")` lacks a newline character. By default, the C library holds standard output data in a **user-level memory buffer** instead of writing it directly to the terminal screen.
2. **Immediate Kernel Termination:** `_exit()` completely bypasses standard library cleanup routines. It commands the OS kernel to kill the process instantly. 
3. **No Buffer Flush:** Because the process terminates abruptly, the user-level buffer holding `"Hey"` is wiped from memory before it has a chance to be flushed (written) to the terminal window.

---

## 3. Key Differences

| Feature | `exit()` | `_exit()` |
| :--- | :--- | :--- |
| **Layer** | Standard C Library | Operating System Kernel (System Call) |
| **Buffer Flush** | **Yes**, automatically flushes open streams | **No**, discards user-level buffers |
| **Typical Use** | Normal program termination | Terminating a child process after a `fork()` |

---

## 4. How to Fix `test2.c` (Force Output)
To make `test2.c` display `"Hey"`, you must force the buffer to flush before `_exit()` cuts off the program:
* **Option A:** Add a newline character: `printf("Hey\n");`
* **Option B:** Explicitly flush standard output: `fflush(stdout);`


---
### Flashcards
What does an OS need to enforce for software to run safely as production software?
??
The OS must enforce:
- Permissions/security: programs can only access allowed resources.
- Resource constraints: programs must work within finite CPU, memory, and disk.
- Isolation/coexistence: one program should not corrupt or interfere with another.
- Scale: the system must continue working with many users/programs or resource-constrained hardware.
<!--SR:!2026-09-26,4,270-->

Why does an OS need to provide virtualization?
??
Virtualization creates the illusion that each program has its own resources.
For the CPU, the OS makes multiple programs appear to run at the same time even though CPUs must be shared.
For memory, the OS gives each program the illusion that it has its own private memory.
This lets a program run without needing to know what other programs are using the machine.
<!--SR:!2026-09-26,3,250-->

What problem does OS concurrency solve?
??
When multiple programs or CPUs operate on the same data at the same time, their operations can conflict.
The OS provides mechanisms that allow concurrent operations to happen without causing conflicts.
<!--SR:!2026-09-24,1,210-->

What is persistence, and how does the OS provide it?
??
Persistence means data survives even after power is turned off.
The OS provides persistence through the file system, giving programs a standard interface for storing and retrieving data without requiring them to know whether the storage is a hard drive, SSD, etc.
<!--SR:!2026-09-26,4,270-->

What are the four core system issues that AI-generated demos can ignore?
??
1. Security — ensuring programs only access allowed resources.
2. Resource constraints — handling finite CPU, memory, and disk.
3. Coexistence — sharing a machine without corrupting other programs.
4. Scale — handling many concurrent users or resource-constrained hardware.
<!--SR:!2026-09-26,3,250-->

Why can the OS stop a poorly written program from damaging other programs?
??
The OS provides isolation and permissions.
A program is restricted to its allowed resources, so faults or malicious behavior in one program should not directly corrupt other programs or the entire machine.
<!--SR:!2026-09-26,3,250-->

What is the difference between user mode and kernel mode when a program has an out-of-bounds memory read?
??
In user mode, the specific program normally crashes while the rest of the OS remains stable.
In kernel mode, a memory fault can crash the entire machine because kernel code operates with much greater privileges.
<!--SR:!2026-09-26,4,270-->

Why was the CrowdStrike 2024 failure able to cause machines to repeatedly crash during boot?
??
The faulty component ran in Windows kernel mode, so the memory error could crash the entire system.
The problematic file was also stored on disk and loaded again during startup, so rebooting did not remove the cause.
The machine therefore repeatedly crashed during boot until the bad file was manually removed.
<!--SR:!2026-09-26,4,270-->

Why does rebooting normally fix many software problems?
??
A reboot wipes the volatile in-memory state where programs may have corrupted data and starts the system again from a clean state using the persistent data on disk.
<!--SR:!2026-09-26,4,270-->

Why doesn't rebooting alone fix the CrowdStrike boot-loop problem described in the notes?
??
Because the problematic file had been saved on persistent disk storage.
Every reboot loaded the same bad file again, so the machine crashed repeatedly during startup.
<!--SR:!2026-09-26,3,250-->

Why does an iPhone use multiple operating systems instead of letting iOS handle everything?
??
Different specialized subsystems can run their own small operating systems.
This improves isolation/security because compromising one subsystem does not necessarily compromise another, and it can improve power efficiency because low-power components do not need to keep the main iOS system awake.
<!--SR:!2026-09-26,4,270-->

What are the specialized operating systems mentioned for the iPhone?
??
- sepOS — Secure Enclave
- Java Card OS — Secure Element used for Apple Pay
- QuRT — cellular modem
- RTKit — low-power/Always-On processing such as sensors and voice-related functions
<!--SR:!2026-09-26,3,250-->

What OS design trade-offs can change depending on the device?
??
Boot time, uptime, throughput, latency, power consumption, security, and resource usage.
Different devices prioritize different goals.
<!--SR:!2026-09-26,4,270-->

Why might a server prioritize throughput over latency?
??
A server may need to process work for many users simultaneously.
High throughput means completing a large amount of total work, while low latency means making an individual operation finish quickly.
A large server may therefore prioritize total system throughput rather than making one user's operation as fast as possible.
<!--SR:!2026-09-26,3,250-->

Why can a server need much higher uptime than a personal computer?
??
A server may serve hundreds or thousands of users continuously.
It cannot simply be rebooted whenever one user's program consumes too much memory or behaves incorrectly.
The OS must isolate programs and control resource usage so one user does not damage service for everyone else.
<!--SR:!2026-09-26,4,270-->

What is the difference between a user-facing workload and a batch workload?
??
A user-facing workload prioritizes high availability, low latency, and high throughput because users expect responsive service.
A batch workload prioritizes high throughput and can tolerate more flexible end-to-end latency, so it can be throttled or terminated when resources are needed elsewhere.
<!--SR:!2026-09-26,3,250-->

Why can averages be misleading when measuring service performance?
??
Averages can hide slow outliers that significantly affect users.
SREs therefore use percentiles, such as an SLO requiring 99% of requests to complete within 100 ms, to understand tail latency.
<!--SR:!2026-09-26,4,270-->

Why can a 1% slow rate at each of 100 independent servers result in about 63% of page loads being slow?
??
Each server has a 99% probability of responding quickly.
The probability that all 100 respond quickly is:
0.99^100 ≈ 0.37
So the probability that at least one server is slow is:
1 - 0.37 ≈ 0.63
Therefore about 63% of page loads encounter at least one slow server.
<!--SR:!2026-09-26,3,250-->

How does the OS handle CPU, I/O, and memory resource contention?
??
CPU: the OS delays lower-priority processes.
I/O: the OS rate-limits lower-priority data streams.
Memory: because memory cannot simply be delayed, the OS may terminate processes when memory is exhausted to keep the machine alive.
<!--SR:!2026-09-26,3,250-->

What are the three levels of isolation discussed in the notes?
??
1. Process
2. Container
3. Virtual machine
<!--SR:!2026-09-26,4,270-->

How does process isolation work?
??
A process gets its own private memory space and a fair share of CPU time.
However, processes still share the host OS kernel and file system.
It has very low overhead.
<!--SR:!2026-09-26,4,270-->

How does container isolation differ from process isolation?
??
A container provides an isolated process group with its own identity, resource limits, and view of the file system.
However, containers still share the host OS kernel.
Examples include Docker and Kubernetes.
<!--SR:!2026-09-26,3,250-->

How does virtual-machine isolation differ from container isolation?
??
A VM provides a fully simulated computer with its own independent operating system.
The VM still shares the underlying physical hardware through a hypervisor.
VM isolation is stronger but has greater overhead because a complete OS must run inside the VM.
<!--SR:!2026-09-26,4,270-->

What is the isolation hierarchy from weakest to strongest?
??
Process → Container → Virtual Machine
<!--SR:!2026-09-26,3,250-->

What must a complete isolation boundary encapsulate?
??
Memory, CPU, files, and identity.
If even one of these vectors is not properly isolated, a fault or malicious actor may be able to compromise shared infrastructure.
<!--SR:!2026-09-26,4,270-->

What is the fundamental trade-off between DRAM and SRAM?
??
DRAM provides high density and low cost but is slower and requires refreshing.
SRAM is much faster and does not require refreshing, but uses more transistors, making it larger and much more expensive per unit of storage.
<!--SR:!2026-09-26,3,250-->

How does DRAM store data, and why does it need refreshing?
??
DRAM stores data as electrical charge in a capacitor.
The capacitor naturally loses charge, so the memory controller must periodically refresh it to preserve the stored data.
<!--SR:!2026-09-26,3,250-->

How does SRAM store data, and why doesn't it require refreshing?
??
SRAM uses a multi-transistor flip-flop circuit to maintain its state while power is supplied.
Because it does not rely on a leaking capacitor charge, it does not require periodic refreshes.
<!--SR:!2026-09-26,3,250-->

Why is DRAM used for main memory while SRAM is used for CPU caches?
??
DRAM has high density and is inexpensive per GB, making it suitable for large main memory.
SRAM is much faster but uses more transistors and is much more expensive, making it suitable for smaller CPU caches such as L1, L2, and L3.
<!--SR:!2026-09-26,3,250-->

What is the difference between exit() and _exit()?
??
exit() is a C standard library function that performs normal termination and flushes open C streams.
_exit() is a low-level system call that terminates the process immediately without performing standard-library cleanup or flushing user-level stdio buffers.
<!--SR:!2026-09-24,1,210-->

Why does printf("Hey"); exit(1); print "Hey"?
??
printf() places "Hey" into the C library's user-level output buffer.
exit() performs normal C library termination, which flushes the open streams before the process terminates.
Therefore the buffered "Hey" reaches the output.
<!--SR:!2026-09-24,1,210-->

Why does printf("Hey"); _exit(1); print nothing?
??
printf() puts "Hey" into the user-level stdio buffer.
_exit() terminates the process without running C library cleanup.
The buffer is therefore never flushed, so "Hey" is lost.
<!--SR:!2026-09-26,4,270-->

When would you typically use _exit() instead of exit()?
??
A common case is terminating a child process after fork().
_exit() avoids running the parent's inherited C library cleanup and flushing inherited stdio buffers.
<!--SR:!2026-09-26,3,250-->

How can you force buffered printf output to appear before _exit()?
??
Either:
- Add a newline when appropriate, such as printf("Hey\n");
- Explicitly flush stdout with fflush(stdout);
<!--SR:!2026-09-26,3,250-->

What is the key difference between libc buffering and kernel-level writing?
??
libc functions such as printf() can first store output in a user-space buffer and later call the kernel's write mechanism.
A direct write() system call sends the bytes to the kernel immediately, without waiting for the libc stdio buffer to fill or be flushed.
<!--SR:!2026-09-26,3,250-->

Why can printf output disappear when a program crashes?
??
printf() may have placed the output only in a user-space libc buffer.
If the program crashes before the buffer is flushed, those bytes disappear with the process.
<!--SR:!2026-09-26,3,250-->

Why does write() output survive a process crash?
??
write() is a system call.
Once the kernel receives the bytes, they are no longer dependent on the process's user-space stdio buffer.
The process can subsequently crash without undoing the already-issued write.
<!--SR:!2026-09-26,3,250-->

Why can stdout and stderr behave differently when a program crashes?
??
stdout is normally buffered by the C library, while stderr is normally unbuffered.
Therefore output sent to stdout may still be sitting in a user-space buffer when the program crashes, while stderr output is normally passed to the kernel immediately.
<!--SR:!2026-09-23,1,230-->

Why is stderr useful for debugging messages?
??
stderr is normally unbuffered, so debugging or error messages are sent out immediately instead of waiting for stdout's buffer to flush.
This makes the message more likely to appear even if the program crashes immediately afterward.
<!--SR:!2026-09-26,4,270-->

What happens in `./test > out.txt` when test uses printf() and crashes before normal termination?
??
The shell redirects stdout to out.txt.
printf() writes its output into the C library's user-space stdout buffer.
The program crashes before that buffer is flushed, so the data never reaches the file and out.txt remains empty.
<!--SR:!2026-09-24,1,210-->

What happens in `./test2 > out2.txt` when test2 uses write() and then crashes?
??
The shell redirects file descriptor 1 (stdout) to out2.txt.
write(1, "hello", 5) immediately sends the bytes to the kernel.
The kernel performs the write to the file, so "hello" remains in out2.txt even though the process crashes afterward.
<!--SR:!2026-09-23,1,230-->