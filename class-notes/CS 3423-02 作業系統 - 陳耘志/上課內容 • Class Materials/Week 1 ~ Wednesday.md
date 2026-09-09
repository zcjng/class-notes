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


---
tags:
  - hardware
  - computing
  - reference
aliases:
  - Memory Comparison
  - RAM vs Cache
---

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

