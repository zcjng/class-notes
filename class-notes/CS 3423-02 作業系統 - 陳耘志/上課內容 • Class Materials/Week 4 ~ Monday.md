---
title: 第四週 • 週一
course: CS 3423-02 作業系統
week: 4
day: Monday
tags:
  - os
---

# 第四週 • 週一 — Exceptions, System Calls & Context Switches

> [!abstract] Main idea
> How does the OS keep control of the CPU while allowing programs to run?
> - **User mode / Kernel mode** → protection
> - **Virtual address spaces** → memory isolation
> - **System calls** → controlled requests to the kernel
> - **Exceptions** → controlled transfer of execution to the kernel
> - **Timer interrupts** → prevent one process from monopolizing the CPU
> - **Context switches** → switch between processes

## 1. Fault Isolation & Protection

The OS treats **every process as potentially buggy or malicious**. A process should not be able to:

- Read/write another process's memory, or access kernel memory
- Directly access hardware/I/O devices
- Execute privileged instructions (disable interrupts, change page tables)
- Keep the CPU forever

The OS must ensure a failure in one process does not bring down the entire system.

### Why hardware is necessary

Software alone cannot enforce protection — a malicious program could execute a privileged instruction **before another program's checking code gets a chance to run**. Protection therefore requires **hardware support**:

- CPU execution modes
- Memory permissions
- Privileged instructions

> [!quote] Hardware enforces the boundary between untrusted programs and the kernel.

## 2. User Mode vs Kernel Mode

Modern CPUs have at least two execution modes.

| | User mode | Kernel mode |
| --- | --- | --- |
| Code | Untrusted applications | Trusted kernel |
| Instructions | Limited | Privileged instructions allowed |
| Memory | User memory | Kernel + user memory |
| I/O devices | ❌ | ✅ |
| Privileged operations | ❌ | ✅ |

The CPU maintains a **mode bit**:

```
mode = 0 → User mode
mode = 1 → Kernel mode
```

A user program **cannot directly modify the mode bit**. Hardware changes it during controlled entry into the kernel and restores it when returning:

```
User program ──system call / fault / interrupt──▶ Kernel mode
User program ◀─────────return-from-trap────────── Kernel
```

### What user mode prevents

A user program cannot directly access hardware, stop the CPU, disable interrupts, change page tables, install the trap table, or access kernel-only memory. If it attempts a privileged operation, the CPU raises an exception instead of executing it.

> [!warning] Root ≠ Kernel mode (important quiz question)
> **Root** is a user identity (`UID 0`). **Kernel mode** is a CPU privilege state.
> A root process still runs in user mode, must use system calls, and cannot directly execute privileged instructions. Root has powerful *permissions*, but does not automatically become kernel code.

## 3. Address Space

Each process gets its own **private virtual address space**:

```
High addresses
┌────────────────────┐
│      Stack ↓       │
│                    │
│       Free         │
│                    │
│      Heap ↑        │
├────────────────────┤
│       Code         │
├────────────────────┤
│   Kernel mapping   │
└────────────────────┘
Low addresses
```

### Main regions

- **Code** — program instructions.
- **Heap** — memory allocated with `malloc()` / `new`. Grows **upward**.
- **Stack** — function-call frames: local variables, arguments, return addresses. Grows **downward**. The stack pointer points to the current stack frame.

Stack and heap are placed apart so they can grow toward each other while leaving a large unused region between them.

### Virtual addresses

Every address visible to a program is a **virtual address**:

```
Program ──virtual address──▶ MMU / page tables ──▶ Physical memory
```

This gives each process the illusion that it owns its own memory, and prevents one process from touching another's.

**ASLR** (Address Space Layout Randomization) changes virtual memory locations between executions, making memory attacks harder.

### The kernel mapping

The upper part of every process's address space contains a mapping of the kernel. The kernel code/data mapping is shared, but **each process has its own kernel stack**.

```
Process A                  Process B

┌─────────────┐            ┌─────────────┐
│   Kernel    │            │   Kernel    │
│   mapping   │            │   mapping   │
├─────────────┤            ├─────────────┤
│   Stack A   │            │   Stack B   │
│             │            │             │
│   Heap A    │            │   Heap B    │
│   Code A    │            │   Code B    │
└─────────────┘            └─────────────┘
```

**Why map the kernel into every address space?** Speed. When a system call or interrupt occurs, the CPU can enter the kernel without first switching to a completely separate address space. (Mapping ≠ access: the kernel's memory is still protected from user-mode code.)

**Why does each process need its own kernel stack?** The kernel may work on multiple processes at once — e.g. process A is sleeping inside `read()` while B runs. A's kernel state must remain intact, so each process has its own kernel stack that cannot be overwritten by another process's kernel state.

## 4. Function Call vs System Call

### Normal function call

Stays in **user mode**. It mainly changes the program counter and stack pointer — it does **not** change CPU mode, address space, or process.

### System call

A system call lets a user program ask the kernel to perform a service, e.g. `write(1, "meow", 4)`. A user program cannot directly talk to the screen (an I/O device), so a system call requires a **privilege transition**:

```
User program ──system call──▶ Kernel ──return──▶ User program
```

### Why can't we just call a kernel function?

The kernel's code is mapped into the process's address space, so why not just call `sys_write(...)` like an ordinary function? Because a normal function call does **not** change the CPU mode — the kernel function requires `mode = kernel`, and only **hardware** can make this transition.

The CPU therefore provides a special **trap / syscall instruction** that:

1. Switches to kernel mode
2. Jumps to a controlled kernel entry point

The program provides a **system call number** telling the kernel which service it wants.

### The five steps of a system call

Example: `write(1, "meow", 4)`. Registers carry the syscall number and arguments (for `write`: `R1` = syscall number, `R2` = fd, `R3` = **address** of `"meow"` (not the string itself), `R4` = byte count):

1. **Program (libc)** — places syscall number and arguments in registers, executes the trap/syscall instruction.
2. **Hardware** — saves registers and PC, switches to kernel mode, jumps to the kernel entry point.
3. **Kernel** — reads syscall number, validates arguments, performs privileged work.
4. **Hardware (return)** — restores saved registers, places result in the return register, switches back to user mode, continues after the syscall.
5. **Program (libc)** — checks the return value, converts failure into `-1` + `errno`.

### Argument validation

System calls receive **untrusted input**. Given `write(fd, buffer, size)`, `buffer` is a pointer, and the kernel must verify that `[buffer, buffer + size)` lies within valid user memory — otherwise a malicious program could pass a pointer into kernel memory and trick the kernel into reading or modifying protected data.

> [!important] Every system call argument must be validated at the user/kernel boundary.
> Especially pointers, strings, arrays, and memory ranges.

### `printf()` and buffering

`printf()` is a **libc function**, not a direct kernel operation, and it can buffer output — three `printf("meow")` calls may result in **one** `write()`:

```
3 × printf() ──▶ 1 × write() ──▶ Kernel
```

> System calls are more expensive than normal function calls, so libc reduces kernel transitions by buffering output.

## 5. Four Types of CPU Exceptions

An **exception** interrupts the CPU's normal instruction sequence and transfers control to a kernel handler. The four categories differ in *who caused it* and *what happens afterward*:

| Type | Cause | Afterward | Mnemonic |
| --- | --- | --- | --- |
| **Trap** | Program intentionally asks | Continue at next instruction | program intentionally asks |
| **Fault** | Instruction goes wrong | Fix + retry OR kill process | instruction goes wrong |
| **Interrupt** | External hardware | Handle event, continue | outside hardware asks |
| **Abort** | Serious hardware/system failure | Usually kernel panic | system is seriously broken |

### Trap

Program intentionally requests kernel attention — e.g. a system call or a GDB breakpoint. **Trap = intentional.**

### Fault

The current instruction causes a problem — e.g. page fault, divide by zero, invalid address, privileged instruction in user mode.

- **Recoverable**: page fault → kernel fixes mapping → retry instruction → continue.
- **Fatal**: invalid address → kernel cannot fix it → `SIGSEGV` → process terminates.

> [!note] A fault does NOT automatically mean the process dies.
> A page fault can be completely normal (e.g. first access to a newly allocated page).

### Interrupt

Caused by something **outside the currently running program** — keyboard input, timer tick, disk completion, network packet. Kernel handles the event, then continues or schedules. **Interrupt = external event.**

### Abort

A serious failure where the system cannot safely continue — uncorrectable memory error, machine check, corrupted kernel state. Usually ends in kernel panic. **Abort = catastrophic.**

## 6. Time Sharing & Scheduling

A single CPU core executes only **one process at a time**. The OS creates the illusion of concurrency by switching rapidly (`A → B → C → A → ...`) — this is **time sharing**, implemented via **limited direct execution**: the program runs directly on the CPU for speed, but the OS has mechanisms to regain control.

### Cooperative scheduling

The process voluntarily gives up the CPU (e.g. `yield()`). Problem: if a process runs `while (1) {}` and never yields, the kernel cannot regain the CPU and the system hangs. This is why cooperative scheduling is unsafe.

### Preemptive scheduling

Modern systems use a **hardware timer** that periodically generates an interrupt:

```
Timer tick ──▶ Interrupt ──▶ Kernel regains CPU ──▶ Scheduler decides what to run
```

The process cannot prevent the hardware timer from interrupting it. Linux commonly uses around **250 or 1000 timer ticks per second**.

## 7. Context Switch

A **context switch** changes the process running on the CPU. To stop a process and resume it later, the kernel saves its **context** (registers, program counter, stack pointer, other CPU state) in the process's **PCB — Process Control Block**:

```
Process A ──save context──▶ PCB of A
PCB of B ──load context──▶ Process B
```

When A runs again, its saved state is restored, so **A resumes exactly where it stopped**.

### Mode switch vs context switch

This distinction is **very important**:

- **Mode switch** — changes privilege level (User ↔ Kernel). The *same process* may continue running.
- **Context switch** — changes *which process is running* (Process A ↔ Process B). Saves/loads process context.

> [!important] Mode switch ≠ Context switch

Examples:

- `getpid()`: A → kernel → A. **Mode switch only, no context switch.**
- `read()` with no data: A → kernel → A sleeps → B runs. **Context switch occurs.**
- Timer tick with only one ready process: A → kernel → A. **No context switch.**

> A timer tick **may** cause a context switch, but does not always cause one.

## 8. Fast vs Slow System Calls

**Fast system call** — the kernel can answer immediately; the process does not sleep. E.g. `getpid()`, `getuid()`, `gettimeofday()`.

**Slow system call** — the kernel must wait for an external event. E.g. `read()` (keyboard/disk/network), `write()` (full pipe), `wait()` (child hasn't exited).

```
Process A ──slow syscall──▶ sleep ──▶ Process B runs
External event ──▶ Interrupt ──▶ Wake A ──▶ A becomes runnable
```

A sleeping process consumes **no CPU time**.

> [!note] "Slow" ≠ computationally slow
> A slow syscall means the process may have to **wait for an external event**, not that the call itself is expensive.

## 9. vDSO

System calls have overhead because they require User → trap → Kernel → return → User. For some operations this is unnecessary: Linux provides the **vDSO** (virtual dynamic shared object), which lets libc answer certain time-related functions without trapping:

```
gettimeofday() ──▶ vDSO ──▶ read shared kernel-maintained data
```

No trap, no mode switch, no kernel entry — it behaves like a normal function call. The worksheet measures `gettimeofday()` at ~**961 ns** via a real syscall versus ~**128 ns** via the vDSO.

**Why can't every syscall use the vDSO?** Many system calls require actual kernel work — `read()` may access an I/O device, block, or use the process's kernel-managed file descriptor table.

> vDSO works only for operations that can be answered from kernel-maintained data **without privileged work**.

## 10. Two System Programming Habits

### Habit 1 — If you provide a service

> **Validate every input from untrusted code.**

The kernel must not assume the caller is correct. This applies to kernels, browsers, databases, and other system software.

### Habit 2 — If you use a service

> **Assume every system call can fail.**

```c
int fd = open("config.txt", O_RDONLY);

if (fd == -1) {
    perror("open");
}
```

Common `errno` values:

| `errno` | Meaning |
| --- | --- |
| `ENOENT` | No such file/directory |
| `EACCES` | Permission denied |
| `EFAULT` | Bad address |
| `ENOMEM` | Cannot allocate memory |

### Why error checking matters

Consider ignoring errors from `open()` then `read()` then `printf()`:

```
open() → -1 + ENOENT, ignored ❌
read(-1, ...) → fails with EBADF, ignored ❌
printf(buf) → buf holds uninitialized data → undefined behavior / crash
```

> [!tip] Always check the return value of every system call before using its result.
> For `read()`, use its return value to know how many bytes in the buffer are actually valid.

## 11. Cheat Sheet — Key Comparisons

### Function call vs system call

| Function call | System call |
| --- | --- |
| User → user | User → kernel → user |
| Stays in user mode | Enters kernel mode |
| Uses user stack | Kernel uses kernel stack |
| Cheap | More expensive |
| Program chooses function address | Controlled kernel entry point |

### Trap vs fault vs interrupt vs abort

```
TRAP      → program intentionally asks
FAULT     → instruction goes wrong
INTERRUPT → outside hardware asks
ABORT     → system is seriously broken
```

### Fast vs slow system call

```
FAST  → kernel answers immediately → process continues
SLOW  → must wait for external event → process sleeps
        → another process runs → event wakes it → runnable again
```

### Mode switch vs context switch

```
MODE SWITCH     → changes privilege level (User ↔ Kernel)
                  same process may continue
CONTEXT SWITCH  → changes process (A ↔ B)
                  saves/loads process context
```

## 12. Quiz Scenarios

Worth being able to answer immediately:

| Scenario | Mode switch? | Context switch? | Notes |
| --- | --- | --- | --- |
| `getpid()` | Yes | No | A → kernel → A |
| `read()` from keyboard, no key yet | Yes | Usually yes | A sleeps → B runs; later a keyboard interrupt wakes A |
| Timer tick, only one runnable process | Yes (interrupt) | No | A → kernel → A |
| Timer tick, another runnable process | Yes | May happen | A → kernel → B |
| First access to a newly allocated page | Fault, yes | — | Kernel maps the page, retries, process is **not** killed |
| `while(1){}` | — | — | Cooperative: can hang the machine. Preemptive: timer interrupts return control to the kernel |

## 13. One-Minute Mental Model

> Programs run directly in user mode for speed. Hardware prevents them from doing dangerous things. System calls provide a controlled way to request kernel services. Exceptions and interrupts give the kernel a way to regain control. Timer interrupts prevent a process from monopolizing the CPU. Context switches let the kernel move the CPU from one process to another. Virtual address spaces isolate processes from each other. Slow system calls put waiting processes to sleep instead of wasting CPU. vDSO avoids the kernel transition for a small set of operations where it is safe to do so.

---

## Flashcards

What is fault isolation? 
??
Fault isolation is the OS's ability to prevent a buggy or malicious process from interfering with other processes or the kernel.
<!--SR:!2026-10-07,1,230-->

Why does the OS treat every process as potentially buggy or malicious?
??
Because any process could contain bugs or intentionally attempt to access resources it should not be allowed to access.
<!--SR:!2026-10-10,4,270-->

Why can't software alone enforce protection?
??
A buggy or malicious program could simply ignore software rules. Hardware must enforce the boundary between protected and unprotected operations.
<!--SR:!2026-10-07,1,230-->

Why is hardware support necessary for OS protection?
??
Hardware provides mechanisms such as CPU privilege modes and protected memory access that software cannot bypass.
<!--SR:!2026-10-07,1,230-->

What are the two CPU execution modes?
??
User mode and kernel mode.
<!--SR:!2026-10-10,4,270-->

What can a program do in user mode?
??
It can perform normal unprivileged operations but cannot perform privileged operations reserved for the kernel.
<!--SR:!2026-10-10,4,270-->

What can the kernel do in kernel mode?
??
The kernel can perform privileged operations such as managing hardware, memory, and other protected resources.
<!--SR:!2026-10-10,4,270-->

Why can't a user program perform privileged instructions?
??
The CPU detects that the program is running in user mode and prevents privileged operations.
<!--SR:!2026-10-10,4,270-->

Can a user program directly change the CPU's mode bit?
??
No. Allowing a user program to change itself to kernel mode would defeat the entire protection mechanism. Hardware controls the transition.
<!--SR:!2026-10-07,1,230-->

How does a program enter kernel mode?
??
Through controlled mechanisms such as system calls, exceptions, and interrupts.
<!--SR:!2026-10-07,1,230-->

Is being root the same as being in kernel mode?
??
No. Root is a user-space privilege level. A root process still normally runs in user mode and must enter kernel mode to perform privileged operations.
<!--SR:!2026-10-07,1,230-->

What is the purpose of the `time` command?
??
It measures how much time a program spends running, including user CPU time, system CPU time, and real elapsed time.
<!--SR:!2026-10-10,4,270-->

What is user time?
??
The amount of CPU time spent executing the program's code in user mode.
<!--SR:!2026-10-10,4,270-->

What is system time?
??
The amount of CPU time spent executing kernel code on behalf of the program.
<!--SR:!2026-10-10,4,270-->

What is real time?
??
The actual elapsed wall-clock time from when the program starts until it finishes.
<!--SR:!2026-10-09,2,230-->

Why can real time be greater than user time plus system time?
??
Because the process may spend time waiting, sleeping, or being descheduled while other processes use the CPU.
<!--SR:!2026-10-07,1,230-->

What happens if a bug occurs while the kernel is executing?
??
A kernel-mode bug can be much more serious because the kernel has access to protected resources and controls the entire system.
<!--SR:!2026-10-10,4,270-->

What is a virtual address space?
??
It is the set of virtual memory addresses that a process can use. Each process has its own virtual address space.
<!--SR:!2026-10-10,4,270-->

Why does each process have its own address space?
??
So processes can use the same virtual addresses without directly interfering with each other's memory.
<!--SR:!2026-10-10,4,270-->

What is a virtual address?
??
An address generated and used by a program that is translated by the memory-management hardware into a physical memory address.
<!--SR:!2026-10-07,1,230-->

What does a typical process address space contain?
??
It contains regions such as program code, data, heap, stack, and a mapping of the kernel.
<!--SR:!2026-10-07,1,230-->

What is the heap used for?
??
The heap is used for dynamically allocated memory, such as memory obtained with `malloc()`.
<!--SR:!2026-10-10,4,270-->

Which direction does the heap normally grow?
??
The heap grows upward toward higher virtual addresses.
<!--SR:!2026-10-10,4,270-->

What is the stack used for?
??
The stack stores function-call information such as local variables, function arguments, return addresses, and saved registers.
<!--SR:!2026-10-10,4,270-->

Which direction does the stack normally grow?
??
The stack grows downward toward lower virtual addresses.
<!--SR:!2026-10-10,4,270-->

Why are the stack and heap placed apart?
??
They can grow toward each other while leaving a large region of unused virtual address space between them.
<!--SR:!2026-10-10,4,270-->

What is ASLR?
??
Address Space Layout Randomization randomizes important memory locations so that their addresses are different between executions, making certain attacks harder.
<!--SR:!2026-10-07,1,230-->

Why is the kernel mapped into every process's address space?
??
It allows the kernel to access its own code and data when handling a system call, exception, or interrupt without switching to a completely different address space.
<!--SR:!2026-10-09,2,230-->

Does mapping the kernel into a process's address space mean the process can access the kernel?
??
No. The kernel's memory is protected so user-mode code cannot access it directly.
<!--SR:!2026-10-10,4,270-->

Does every process have its own kernel stack?
??
Yes. Each process has a separate kernel stack used when that process enters the kernel.
<!--SR:!2026-10-09,2,230-->

What is a normal function call?
??
A normal function call transfers execution from one function to another within the same privilege level, normally staying in user mode.
<!--SR:!2026-10-07,1,230-->

What is a system call?
??
A system call is a controlled mechanism that allows a user program to request a service from the kernel.
<!--SR:!2026-10-10,4,270-->

Why can't a user program simply call a kernel function like a normal function?
??
A normal function call does not provide the required privilege transition. The CPU must switch from user mode to kernel mode through a protected system-call mechanism.
<!--SR:!2026-10-10,4,270-->

How does a system call enter the kernel?
??
The program executes a special system-call or trap instruction that causes the CPU to enter kernel mode and transfer control to the kernel.
<!--SR:!2026-10-10,4,270-->

Where is the system-call number passed?
??
The system-call number is placed in a designated CPU register before entering the kernel.
<!--SR:!2026-10-10,4,270-->

Where are system-call arguments passed?
??
They are passed using designated CPU registers according to the system-call calling convention.
<!--SR:!2026-10-10,4,270-->

Where is the system-call return value passed?
??
The kernel places the return value in the designated return-value register before returning to user mode.
<!--SR:!2026-10-10,4,270-->

What are the basic steps of a system call?
??
1. The user program prepares the system-call number and arguments.
2. It executes the system-call instruction.
3. Hardware switches to kernel mode and enters the kernel.
4. The kernel validates the arguments.
5. The kernel performs the requested operation.
6. The kernel places the result in the return-value register.
7. Control returns to user mode.
<!--SR:!2026-10-10,4,270-->

Why must the kernel validate system-call arguments?
??
System-call arguments come from untrusted user programs, so the kernel must verify that they are valid and safe to access.
<!--SR:!2026-10-07,1,230-->

Why are pointer arguments especially important to validate?
??
A pointer supplied by a user program could point to invalid memory or protected kernel memory.
<!--SR:!2026-10-10,4,270-->

What does the kernel need to check when given a pointer to a buffer?
??
It must verify that the entire memory range of the buffer is valid and accessible, not just the first address.
<!--SR:!2026-10-07,1,230-->

What could happen if the kernel fails to validate a user pointer?
??
The kernel could access invalid or protected memory, potentially causing a kernel crash or security vulnerability.
<!--SR:!2026-10-10,4,270-->

Is `printf()` a system call?
??
No. `printf()` is a user-space C library function that may eventually use a system call such as `write()`.
<!--SR:!2026-10-10,4,270-->

Why might multiple `printf()` calls produce only one `write()` system call?
??
Because the C library can buffer the output and combine several `printf()` operations before making a system call.
<!--SR:!2026-10-10,4,270-->

What are the four types of CPU exceptions?
??
Trap, fault, interrupt, and abort.
<!--SR:!2026-10-10,4,270-->

What is a trap?
??
A trap is an intentional exception caused by the currently running program, such as a system call or breakpoint.
<!--SR:!2026-10-10,4,270-->

What is a fault?
??
A fault occurs when the current instruction encounters a condition that may be recoverable. The kernel may fix the problem and retry the instruction.
<!--SR:!2026-10-10,4,270-->

What is an interrupt?
??
An interrupt is an event generated externally, typically by hardware, that causes the CPU to temporarily stop the current execution and enter the kernel.
<!--SR:!2026-10-10,4,270-->

What is an abort?
??
An abort is a serious, unrecoverable error that generally cannot be restarted.
<!--SR:!2026-10-10,4,270-->

What is the key difference between a trap and an interrupt?
??
A trap is caused intentionally by the currently running program, while an interrupt comes from an external source such as hardware.
<!--SR:!2026-10-10,4,270-->

What is the key difference between a fault and an abort?
??
A fault may be recoverable and allow the instruction to be retried, while an abort indicates an unrecoverable failure.
<!--SR:!2026-10-09,3,250-->

Can a page fault be a normal event?
??
Yes. A page fault can occur because a needed page is not currently mapped into physical memory. The kernel can handle the fault and retry the instruction.
<!--SR:!2026-10-10,4,270-->

What is time sharing?
??
Time sharing allows multiple processes to share the CPU by giving each process opportunities to run for limited periods of time.
<!--SR:!2026-10-10,4,270-->

What is cooperative scheduling?
??
Processes voluntarily give up the CPU. If a process never yields, it can prevent other processes from running.
<!--SR:!2026-10-10,4,270-->

What is the main problem with cooperative scheduling?
??
A buggy or malicious process could keep running forever without yielding, preventing other processes from getting CPU time.
<!--SR:!2026-10-07,1,230-->

What is preemptive scheduling?
??
The operating system can forcibly interrupt a running process and take control of the CPU.
<!--SR:!2026-10-07,1,230-->

What mechanism allows preemptive scheduling?
??
A hardware timer periodically generates interrupts, allowing the kernel to regain control of the CPU.
<!--SR:!2026-10-07,0,210-->

What happens when a timer interrupt occurs?
??
The CPU enters the kernel, where the scheduler can decide whether the current process should continue or another process should run.
<!--SR:!2026-10-07,1,230-->

What is a context switch?
??
A context switch changes the CPU from running one process to running another by saving the first process's state and restoring the second process's state.
<!--SR:!2026-10-10,4,270-->

What is saved during a context switch?
??
Important CPU state such as registers, the program counter, and stack pointer is saved so the process can later resume correctly.
<!--SR:!2026-10-10,4,270-->

Where is a process's saved execution state kept?
??
The operating system keeps the process's saved state in its process-control data structures, such as its PCB and related kernel structures.
<!--SR:!2026-10-10,4,270-->

How can a process resume exactly where it stopped?
??
Its program counter, stack pointer, registers, and other necessary execution state were saved during the context switch and restored when it runs again.
<!--SR:!2026-10-10,4,270-->

What is the difference between a mode switch and a context switch?
??
A mode switch changes between user mode and kernel mode. A context switch changes which process is currently running.
<!--SR:!2026-10-10,4,270-->

Can a mode switch happen without a context switch?
??
Yes. A process can make a system call, execute in kernel mode, and then return to the same process without another process running.
<!--SR:!2026-10-07,1,230-->

Can a context switch happen without changing processes' user/kernel modes?
??
A context switch involves changing the running process, while mode changes are separate from that concept. The important distinction is that they represent different state changes.
<!--SR:!2026-10-10,4,270-->

Does every timer interrupt cause a context switch?
??
No. A timer interrupt gives the kernel an opportunity to schedule, but the scheduler may decide to continue running the same process.
<!--SR:!2026-10-09,2,230-->

What is a fast system call?
??
A fast system call completes without waiting for an external event and usually returns quickly.
<!--SR:!2026-10-10,4,270-->

What is a slow system call?
??
A slow system call may block because it needs to wait for an external event, such as input becoming available.
<!--SR:!2026-10-10,4,270-->

What happens when a process makes a slow system call and the required event has not happened yet?
??
The process can sleep or block, allowing the scheduler to run another process instead.
<!--SR:!2026-10-10,4,270-->

How does a blocked process become runnable again?
??
The external event occurs, usually causing a hardware interrupt. The kernel handles the event and wakes the waiting process.
<!--SR:!2026-10-09,2,230-->

What happens when `getpid()` is called?
??
It is a fast system call that returns the process ID without needing to wait for an external event.
<!--SR:!2026-10-10,4,270-->

Does `getpid()` necessarily cause a context switch?
??
No. It may involve a mode switch into the kernel and back, but the same process can continue running afterward.
<!--SR:!2026-10-10,4,270-->

What happens when `read()` waits for keyboard input that has not arrived?
??
The process blocks and sleeps while waiting for input, allowing another process to run.
<!--SR:!2026-10-10,4,270-->

What happens when a timer tick occurs?
??
The timer generates an interrupt and gives the scheduler an opportunity to decide whether to perform a context switch.
<!--SR:!2026-10-07,1,230-->

What is the vDSO?
??
The vDSO is a mechanism that provides certain system-call-like functionality directly in user space, avoiding a kernel transition.
<!--SR:!2026-10-10,4,270-->

Why is the vDSO useful?
??
It avoids the overhead of entering and leaving the kernel for operations that can safely be performed using information available to the process.
<!--SR:!2026-10-10,4,270-->

Why can't every system call use the vDSO?
??
Some operations require privileged kernel access, hardware interaction, or waiting for external events and therefore cannot safely be performed entirely in user space.
<!--SR:!2026-10-10,4,270-->

Why could a time-related operation benefit from the vDSO?
??
The kernel can provide information that allows the operation to be completed in user space without a full system-call transition.
<!--SR:!2026-10-10,4,270-->

Why could `getpid()` potentially be implemented using the vDSO?
??
The process ID can be obtained from information already available to the process without requiring privileged hardware access.
<!--SR:!2026-10-07,1,230-->

Why can't `read()` generally be replaced by a vDSO implementation?
??
`read()` may need to interact with devices or wait for external input, which requires kernel involvement.
<!--SR:!2026-10-10,4,270-->

What is an important system programming habit regarding input?
??
Always treat input from user programs as untrusted and validate it before using it.
<!--SR:!2026-10-10,4,270-->

What is an important system programming habit regarding system calls?
??
Assume every system call can fail and check its return value.
<!--SR:!2026-10-10,4,270-->

Why is ignoring a system-call return value dangerous?
??
The system call may have failed, and continuing as if it succeeded can cause incorrect behavior or hide an important error.
<!--SR:!2026-10-07,1,230-->

What can `errno` tell you?
??
When a system call or library operation reports an error in the appropriate way, `errno` can provide additional information about the reason for the failure.
<!--SR:!2026-10-07,1,230-->

What should you remember about system calls and errors?
??
A system call is a request to the kernel, not a guarantee of success. Always check its return value and handle failure appropriately.
<!--SR:!2026-10-07,1,230-->

What can `strace` be used for?
??
`strace` can observe the system calls made by a program, including their arguments and return values.
<!--SR:!2026-10-07,1,230-->

Why is `strace` useful for learning system calls?
??
It lets you see the boundary between a user-space program and the kernel by showing the system calls the program actually makes.
<!--SR:!2026-10-10,4,270-->

What can `/proc` be used for?
??
The `/proc` filesystem provides information about running processes and various kernel and system state.
<!--SR:!2026-10-10,4,270-->

What should you be able to do with the `time` command?
??
Measure and compare user CPU time, system CPU time, and real elapsed time for a program.
<!--SR:!2026-10-07,1,230-->

What is the key difference between user CPU time and system CPU time?
??
User CPU time is spent executing user-space code, while system CPU time is spent executing kernel code on behalf of the process.
<!--SR:!2026-10-10,4,270-->

What is the key difference between a system call and an exception?
??
A system call is a controlled request from user space to the kernel and is implemented using a trap-like CPU mechanism. Exceptions are the broader category of events that cause the CPU to transfer control to the kernel.
<!--SR:!2026-10-10,4,270-->

What is the overall mental model for Worksheet 4?
??
User programs normally run in user mode inside their own virtual address spaces. When they need privileged OS services, they enter the kernel through system calls, traps, faults, or interrupts. The kernel validates untrusted input, performs the requested work, and returns to user mode. Hardware timers allow the kernel to preempt processes, and context switches allow the CPU to move between processes.
<!--SR:!2026-10-07,1,230-->
