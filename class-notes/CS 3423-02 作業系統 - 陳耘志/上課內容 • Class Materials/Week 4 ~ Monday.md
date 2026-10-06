---
tiitle: 第四週 • 週一
course: CS 3423-02 作業系統
week: 4
tags:
  - os
---
# OS Worksheet 4 — Exceptions, System Calls & Context Switches

> **Main idea:** How does the OS keep control of the CPU while allowing programs to run?
> 
> The key mechanisms are:
> 
> - **User mode / Kernel mode**
>     
> - **System calls**
>     
> - **Exceptions**
>     
> - **Interrupts**
>     
> - **Timer interrupts**
>     
> - **Context switches**
>     
> - **Virtual address spaces**
>     

---

# 1. Fault Isolation & Protection

The OS treats every process as potentially **buggy or malicious**.

A process should not be able to:

- Access another process's memory
    
- Access kernel memory
    
- Directly access hardware/I/O devices
    
- Execute privileged instructions
    
- Disable interrupts
    
- Change page tables
    
- Run forever without giving other processes CPU time
    

### Why hardware is necessary

Software cannot protect itself reliably.

A malicious program could execute a privileged instruction **before any checking code gets a chance to run**.

Therefore, the **CPU hardware** enforces protection using:

- Execution modes
    
- Memory permissions
    
- Privileged instructions
    

---

# 2. User Mode vs Kernel Mode

Modern CPUs have at least two execution modes.

||User Mode|Kernel Mode|
|---|---|---|
|Code|Untrusted application|Trusted OS/kernel|
|Instructions|Limited|All|
|Memory|User memory|All permitted memory|
|I/O devices|❌|✅|
|Privileged instructions|❌|✅|

The CPU maintains a **mode bit**:

```
mode = 0 → User mode
mode = 1 → Kernel mode
```

A user program **cannot directly change the mode bit**.

The hardware changes it when entering the kernel and restores it when returning.

### User → Kernel

```
User program
     │
     │ system call / exception / interrupt
     ▼
Kernel mode
```

### Kernel → User

```
Kernel
  │
  │ return-from-trap
  ▼
User mode
```

### Important

> **Root ≠ Kernel mode**

A root process still normally runs in **user mode**. Root has extensive OS-level permissions, but it does not directly become kernel code.

---

# 3. Address Space

Each process gets its own **private virtual address space**.

Simplified layout:

```
High addresses
┌──────────────────┐
│      Stack ↓     │
│                  │
│       Free       │
│                  │
│      Heap ↑      │
├──────────────────┤
│       Code       │
├──────────────────┤
│   Kernel space   │
└──────────────────┘
Low addresses
```

### Main regions

- **Code** — program instructions
    
- **Heap** — memory from `malloc()` / `new`
    
    - grows upward
        
- **Stack** — function calls
    
    - local variables
        
    - arguments
        
    - return addresses
        
    - grows downward
        
- **Kernel mapping** — kernel code/data and kernel stack
    

### Virtual addresses

Every address a program sees is a **virtual address**.

The OS + hardware translate virtual addresses to physical memory.

### ASLR

**Address Space Layout Randomization (ASLR)** randomizes the locations of memory regions between executions.

Purpose:

> Make memory attacks harder.

---

# 4. Function Call vs System Call

## Normal function call

A function call stays entirely in **user mode**.

```
User program
     │
     │ function call
     ▼
User function
```

A function call mainly changes:

- Program counter
    
- Stack pointer
    

It does **not** change:

- CPU mode
    
- Address space
    
- Process
    

---

## System call

A system call lets a user program request a service from the kernel.

Example:

```
write(1, "meow", 4);
```

The program cannot directly talk to the screen because it is an I/O device.

Instead:

```
User program
     │
     │ system call / trap
     ▼
Kernel
     │
     │ return-from-trap
     ▼
User program
```

A system call therefore involves a **user → kernel mode transition**.

---

# 5. System Call Steps

Example:

```
write(1, "meow", 4);
```

The system call uses registers to pass:

```
R1 → system call number
R2 → first argument
R3 → second argument
R4 → third argument
```

For `write`:

```
R1 = 1          syscall number
R2 = 1          stdout
R3 = address    pointer to "meow"
R4 = 4          number of bytes
```

### Five steps

1. **Program**
    
    - libc wrapper puts syscall number + arguments in registers
        
    - Executes `syscall` / trap instruction
        
2. **Hardware**
    
    - Saves registers and program counter
        
    - Switches to kernel mode
        
    - Jumps to kernel entry point
        
3. **Kernel**
    
    - Reads syscall number
        
    - Validates arguments
        
    - Performs privileged operation
        
4. **Hardware**
    
    - `return-from-trap`
        
    - Restores registers
        
    - Switches back to user mode
        
5. **Program**
    
    - Checks return value
        
    - Failure may result in `errno`
        

---

# 6. System Call Argument Validation

System calls receive **untrusted input** from user programs.

For example:

```
write(fd, buffer, size);
```

`buffer` is represented by a pointer.

The kernel must check that the pointer refers to valid user memory.

Otherwise, a malicious program could give the kernel a pointer to kernel memory.

### Important principle

> **Every system call argument must be validated at the user/kernel boundary.**

This is especially important for:

- Pointers
    
- Strings
    
- Arrays
    
- Memory ranges
    

---

# 7. `printf()` and System Calls

`printf()` is a **libc function**, not directly a kernel operation.

It can buffer output.

For example:

```
printf("meow");
printf("meow");
printf("meow\n");
```

These may become **one** `**write()**` **system call** instead of three.

Why?

> System calls are expensive compared with normal function calls, so libc can reduce the number of trips into the kernel by buffering output.

---

# 8. Four Types of CPU Exceptions

An **exception** is an event that interrupts the CPU's normal instruction sequence and transfers control to the kernel.

There are four types:

|Type|Who/what causes it?|What happens?|
|---|---|---|
|**Trap**|Program intentionally|Continue at next instruction|
|**Fault**|Current instruction goes wrong|Fix + retry, or kill process|
|**Interrupt**|External hardware|Handle event, continue|
|**Abort**|Serious hardware/system failure|Usually kernel panic|

---

## Trap

The program intentionally asks to enter the kernel.

Examples:

- System call
    
- GDB breakpoint
    

```
Program
  ↓
Trap
  ↓
Kernel
  ↓
Continue at next instruction
```

---

## Fault

An instruction encounters a problem.

Examples:

- Page fault
    
- Divide by zero
    
- Invalid memory access
    
- Privileged instruction in user mode
    

A fault can be recoverable.

### Page fault

The first access to newly allocated memory may cause a page fault.

The kernel can:

```
page fault
    ↓
kernel creates/maps page
    ↓
retry instruction
    ↓
program continues
```

So:

> **A page fault is not necessarily an error.**

A bad address that cannot be fixed can instead result in a **Segmentation fault**.

---

## Interrupt

An interrupt comes from the **outside world**, not the currently executing program.

Examples:

- Keyboard input
    
- Timer tick
    
- Disk completion
    
- Network packet
    

```
Hardware event
      ↓
Interrupt
      ↓
Kernel handler
      ↓
Continue execution
```

---

## Abort

A severe hardware/system failure where the system cannot safely continue.

Example:

- Uncorrectable memory error
    
- Corrupted kernel structure
    

Usually:

```
Abort → Kernel panic
```

---

# 9. Time Sharing

A CPU can only execute one process at a time on a single core.

The OS creates the illusion of simultaneous execution by rapidly switching between processes.

```
Process A → Process B → Process C → Process A → ...
```

This is **time sharing**.

---

# 10. Cooperative vs Preemptive Scheduling

## Cooperative scheduling

The process must voluntarily give up the CPU.

For example:

```
yield();
```

Problem:

```
while (1) {
}
```

If the process never yields:

```
Process runs forever
       ↓
Kernel cannot regain CPU
       ↓
Other processes cannot run
       ↓
System hangs
```

---

## Preemptive scheduling

Modern systems use a **hardware timer**.

The timer periodically generates an interrupt.

```
Timer
  ↓
Timer interrupt
  ↓
Kernel regains control
  ↓
Scheduler chooses process
```

The process cannot prevent the timer interrupt.

This allows the OS to enforce CPU sharing.

---

# 11. Context Switch

A **context switch** switches the CPU from one process to another.

The kernel needs to save the current process's CPU state:

- Registers
    
- Program counter
    
- Stack pointer
    
- Other CPU state
    

This state is called the **context**.

It is stored in the process's:

> **PCB — Process Control Block**

### Context switch

```
Process A
   │
   │ save context
   ▼
PCB of A
   │
   │ load context
   ▼
PCB of B
   │
   ▼
Process B
```

When A runs again, its saved context is restored.

Therefore:

> Process A resumes exactly where it stopped.

---

# 12. Mode Switch vs Context Switch

These are **different concepts**.

### Mode switch

Changes privilege level:

```
User mode
    ↓
Kernel mode
    ↓
User mode
```

The same process may continue running.

### Context switch

Changes which process is running:

```
Process A
    ↓
Process B
```

A context switch requires saving/loading process state.

> A system call causes a **mode switch**, but it does not necessarily cause a **context switch**.

---

# 13. Fast vs Slow System Calls

## Fast system call

The kernel can answer immediately.

Examples:

```
getpid()
getuid()
gettimeofday()
```

```
trap
 ↓
kernel does a few instructions
 ↓
return
```

The process does not need to sleep.

---

## Slow system call

The process needs to wait for an external event.

Examples:

```
read()   // keyboard / disk / network
write()  // full pipe
wait()   // child hasn't exited
```

Instead of wasting CPU while waiting:

```
Process A
    ↓
slow syscall
    ↓
sleep
    ↓
Process B runs
```

When the event happens:

```
External event
     ↓
Interrupt
     ↓
Wake Process A
     ↓
Process A becomes runnable
```

A sleeping process consumes **no CPU time**.

---

# 14. vDSO

A system call has overhead because it requires a user/kernel transition.

```
User
 ↓
trap
 ↓
Kernel
 ↓
return
 ↓
User
```

Some operations don't need privileged kernel work every time.

Linux uses the **vDSO (virtual dynamic shared object)** for selected operations such as time functions.

Instead of:

```
gettimeofday()
      ↓
kernel
```

it can do:

```
gettimeofday()
      ↓
vDSO
      ↓
shared read-only data
```

No trap or mode switch is required.

### Key idea

> **vDSO avoids the user/kernel transition for selected operations.**

It cannot replace every system call because many operations actually require privileged kernel work.

---

# 15. Two System Programming Habits

## 1. Platform providers

> **Validate every input from untrusted code.**

Examples:

- Kernel validates syscall arguments
    
- Browser sandbox validates/restricts web code
    
- Database validates user input
    

---

## 2. Platform users

> **Assume every system call can fail.**

Example:

```
int fd = open("config.txt", O_RDONLY);

if (fd == -1) {
    perror("open");
}
```

Common `errno` values:

|`errno`|Meaning|
|---|---|
|`ENOENT`|No such file/directory|
|`EACCES`|Permission denied|
|`EFAULT`|Bad address|
|`ENOMEM`|Cannot allocate memory|

---

# 16. Key Comparisons

## Function call vs System call

|Function call|System call|
|---|---|
|User → user|User → kernel → user|
|Stays in user mode|Switches to kernel mode|
|Uses user stack|Kernel uses kernel stack|
|Relatively cheap|More expensive|
|Program chooses function address|Controlled kernel entry point|

---

## Trap vs Fault vs Interrupt vs Abort

```
Trap
→ Program intentionally asks

Fault
→ Instruction causes a problem

Interrupt
→ External hardware event

Abort
→ Serious unrecoverable failure
```

---

## Fast vs Slow syscall

```
Fast
→ Kernel can answer immediately
→ Process keeps running

Slow
→ Must wait for an event
→ Process sleeps
→ Another process runs
→ Event wakes the sleeping process
```

---

## Mode switch vs Context switch

```
Mode switch
→ Changes privilege level
→ User ↔ Kernel

Context switch
→ Changes process
→ Process A ↔ Process B
```

---

---

# One-Minute Mental Model

The entire worksheet can be reduced to this:

```
                 HARDWARE
                    │
        ┌───────────┴───────────┐
        │                       │
   User Mode                Kernel Mode
        │                       │
   Applications          Operating System
        │                       │
        │ syscall               │
        └──────────→────────────┘
        ←──────── return ────────
        
        Timer interrupt
              ↓
          Kernel gets CPU
              ↓
        Scheduler decides
              ↓
       Context switch
              ↓
        Another process
```

**The big picture:**

> The CPU runs user programs directly for speed, but hardware prevents them from doing dangerous things. When the OS needs to intervene, a system call, fault, interrupt, or other exception transfers control to the kernel. Timer interrupts let the kernel regain control periodically, and context switches let it share the CPU between processes.**