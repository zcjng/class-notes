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
> - **User mode / Kernel mode** → protection
>     
> - **Virtual address spaces** → memory isolation
>     
> - **System calls** → controlled requests to the kernel
>     
> - **Exceptions** → controlled transfer of execution to the kernel
>     
> - **Timer interrupts** → prevent one process from monopolizing the CPU
>     
> - **Context switches** → switch between processes
>     

---

# 1. Fault Isolation & Protection

The OS treats **every process as potentially buggy or malicious**.

A process should not be able to:

- Read/write another process's memory
    
- Access kernel memory
    
- Directly access hardware/I/O devices
    
- Execute privileged instructions
    
- Disable interrupts
    
- Change page tables
    
- Keep the CPU forever
    

The OS must ensure that a failure in one process does not bring down the entire system.

## Why hardware is necessary

Software alone cannot enforce protection.

A malicious program could execute a privileged instruction **before another program's checking code gets a chance to run**.

Therefore, protection requires **hardware support**:

- CPU execution modes
    
- Memory permissions
    
- Privileged instructions
    

> **Hardware enforces the boundary between untrusted programs and the kernel.**

---

# 2. User Mode vs Kernel Mode

Modern CPUs have at least two execution modes.

||User Mode|Kernel Mode|
|---|---|---|
|Code|Untrusted applications|Trusted kernel|
|Instructions|Limited|Privileged instructions allowed|
|Memory|User memory|Kernel + user memory|
|I/O devices|❌|✅|
|Privileged operations|❌|✅|

The CPU maintains a **mode bit**:

```
mode = 0 → User mode
mode = 1 → Kernel mode
```

A user program **cannot directly modify the mode bit**. The hardware changes it during controlled entry into the kernel and restores it when returning.

## User → Kernel

```
User program
     │
     │ system call / fault / interrupt
     ▼
Kernel mode
```

## Kernel → User

```
Kernel
  │
  │ return-from-trap
  ▼
User mode
```

### What user mode prevents

A user program cannot directly:

- Access hardware
    
- Stop the CPU
    
- Disable interrupts
    
- Change page tables
    
- Install the trap table
    
- Access kernel-only memory
    

If it attempts a privileged operation, the CPU raises an exception instead of executing it.

---

## ⚠️ Root ≠ Kernel Mode

This is an important quiz question.

**Root** is a user identity (`UID 0`).

**Kernel mode** is a CPU privilege state.

Therefore:

```
root process
     ↓
still runs in user mode
     ↓
must use system calls
     ↓
cannot directly execute privileged instructions
```

Root has very powerful **permissions**, but it does not automatically become kernel code.

---

# 3. Address Space

Each process gets its own **private virtual address space**.

Simplified:

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

## Main regions

### Code

Program instructions.

### Heap

Memory allocated with:

```
malloc()
new
```

Grows **upward**.

### Stack

Contains function-call frames:

- Local variables
    
- Arguments
    
- Return addresses
    

Grows **downward**.

The stack pointer points to the current stack frame.

---

## Virtual addresses

Every address visible to a program is a **virtual address**.

```
Program
   │
   │ virtual address
   ▼
MMU / page tables
   │
   ▼
Physical memory
```

This gives each process the illusion that it owns its own memory.

It prevents:

```
Process A
    ✗
    ↓
Process B's memory
```

### ASLR

**Address Space Layout Randomization** changes virtual memory locations between executions.

Purpose:

> Make memory attacks harder.

---

# 4. The Kernel Mapping

The upper part of every process's address space contains a mapping of the kernel.

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

The kernel code/data mapping is shared, but **each process has its own kernel stack**.

## Why map the kernel into every address space?

**Speed.**

When a system call or interrupt occurs, the CPU can enter the kernel without first switching to a completely separate address space.

## Why does each process need its own kernel stack?

The kernel may be working on multiple processes at the same time.

For example:

```
Process A → sleeping inside read()
Process B → currently running
```

A's kernel state must remain intact while B runs.

Therefore:

> **Each process has its own kernel stack so its kernel state cannot overwrite another process's state.**

---

# 5. Function Call vs System Call

## Normal function call

A normal function call stays in:

> **User mode**

Example:

```
foo();
```

It mainly changes:

- Program counter
    
- Stack pointer
    

It does **not** change:

- CPU mode
    
- Address space
    
- Process
    

```
User mode
   │
   │ function call
   ▼
Another user function
```

---

## System call

A system call allows a user program to ask the kernel to perform a service.

Example:

```
write(1, "meow", 4);
```

A user program cannot directly talk to the screen because the screen is an I/O device.

Instead:

```
User program
     │
     │ system call
     ▼
Kernel
     │
     │ return
     ▼
User program
```

A system call therefore requires a **privilege transition**.

---

# 6. Why Can't We Just Call a Kernel Function?

The kernel's code is mapped into the process's address space, so why not:

```
sys_write(...);
```

like an ordinary function?

Because a normal function call does **not** change the CPU mode.

```
function call:

PC changes
mode stays = user
```

The kernel function requires:

```
mode = kernel
```

Only the **hardware** can make this transition.

Therefore the CPU provides a special instruction:

> **trap / syscall instruction**

It:

1. Switches to kernel mode
    
2. Jumps to a controlled kernel entry point
    

The program provides a **system call number** telling the kernel which service it wants.

---

# 7. System Call Steps

Example:

```
write(1, "meow", 4);
```

Registers contain the syscall number and arguments:

```
R1 = syscall number
R2 = argument 1
R3 = argument 2
R4 = argument 3
```

For `write`:

```
R1 = 1          write syscall
R2 = 1          stdout
R3 = address    pointer to "meow"
R4 = 4          number of bytes
```

The important point:

> `R3` contains the **address** of `"meow"`, not the string itself.

## Five steps

### 1. Program

libc:

- Places syscall number in a register
    
- Places arguments in registers
    
- Executes the trap/syscall instruction
    

### 2. Hardware

CPU:

- Saves registers and program counter
    
- Switches to kernel mode
    
- Jumps to the kernel entry point
    

### 3. Kernel

Kernel:

- Reads syscall number
    
- Validates arguments
    
- Performs privileged work
    

### 4. Hardware

On return:

- Restores saved registers
    
- Places result in the return register
    
- Switches back to user mode
    
- Continues after the syscall
    

### 5. Program

libc/program:

- Checks the return value
    
- Converts failure into `-1` + `errno`
    

  

---

# 8. System Call Argument Validation

System calls receive **untrusted input**.

Consider:

```
write(fd, buffer, size);
```

`buffer` is passed as a pointer.

The kernel must verify that:

```
buffer ... buffer + size
```

lies within valid user memory.

Otherwise a malicious program could give the kernel:

```
pointer → kernel memory
```

and trick the kernel into reading or modifying protected data.

Therefore:

> **Every system call argument must be validated at the user/kernel boundary.**

This is especially important for:

- Pointers
    
- Strings
    
- Arrays
    
- Memory ranges
    

  

---

# 9. `printf()` and System Calls

`printf()` is a **libc function**, not a direct kernel operation.

It can buffer output.

For example:

```
printf("meow");
printf("meow");
printf("meow\n");
```

may result in:

```
3 × printf()
      ↓
1 × write()
      ↓
Kernel
```

Why?

> System calls are more expensive than normal function calls, so libc can reduce kernel transitions by buffering output.

---

# 10. Four Types of CPU Exceptions

An **exception** is an event that interrupts the CPU's normal instruction sequence and transfers control to a kernel handler.

The four categories are based on:

1. **Who caused it?**
    
2. **What happens afterward?**
    

|Type|Cause|Afterward|
|---|---|---|
|**Trap**|Program intentionally asks|Continue at next instruction|
|**Fault**|Instruction goes wrong|Fix + retry OR kill process|
|**Interrupt**|External hardware|Handle event, continue|
|**Abort**|Serious hardware/system failure|Usually kernel panic|

  

---

## Trap

Program intentionally requests kernel attention.

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
Continue
```

### Key idea

> **Trap = intentional**

---

## Fault

The current instruction causes a problem.

Examples:

- Page fault
    
- Divide by zero
    
- Invalid address
    
- Privileged instruction in user mode
    

A fault can be:

### Recoverable

```
Page fault
    ↓
Kernel fixes mapping
    ↓
Retry instruction
    ↓
Continue
```

### Fatal

```
Invalid address
    ↓
Kernel cannot fix it
    ↓
SIGSEGV
    ↓
Process terminates
```

Therefore:

> **A fault does NOT automatically mean the process dies.**

A page fault can be completely normal.

---

## Interrupt

Caused by something **outside the currently running program**.

Examples:

- Keyboard input
    
- Timer tick
    
- Disk completion
    
- Network packet
    

```
Hardware
   ↓
Interrupt
   ↓
Kernel handler
   ↓
Continue / schedule
```

### Key idea

> **Interrupt = external event**

---

## Abort

A serious failure where the system cannot safely continue.

Examples:

- Uncorrectable memory error
    
- Machine check
    
- Corrupted kernel state
    

Usually:

```
Abort
  ↓
Kernel panic
```

### Key idea

> **Abort = catastrophic**

---

## Easy way to remember

```
TRAP      → program intentionally asks
FAULT     → instruction goes wrong
INTERRUPT → outside hardware asks
ABORT     → system is seriously broken
```

---

# 11. Time Sharing

A single CPU core can execute only **one process at a time**.

The OS creates the illusion of concurrency by switching rapidly:

```
A → B → C → A → B → C → ...
```

This is **time sharing**.

The mechanism is called:

> **Limited direct execution**

The program runs directly on the CPU for speed, but the OS needs mechanisms to regain control.

---

# 12. Cooperative vs Preemptive Scheduling

## Cooperative scheduling

The process voluntarily gives up the CPU.

Example:

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

This is why cooperative scheduling is unsafe.

---

## Preemptive scheduling

Modern systems use a **hardware timer**.

The timer periodically generates an interrupt.

```
Timer tick
    ↓
Interrupt
    ↓
Kernel regains CPU
    ↓
Scheduler decides what to run
```

The process cannot prevent the hardware timer from interrupting it.

Linux commonly uses around **250 or 1000 timer ticks per second**.

---

# 13. Context Switch

A **context switch** changes the process running on the CPU.

To stop a process and resume it later, the kernel saves its **context**:

- Registers
    
- Program counter
    
- Stack pointer
    
- Other CPU state
    

The context is stored in the process's:

> **PCB — Process Control Block**

## Context switch

```
Process A
   │
   │ save context
   ▼
PCB of A

PCB of B
   │
   │ load context
   ▼
Process B
```

When A runs again, its saved state is restored.

Therefore:

> **A resumes exactly where it stopped.**

---

# 14. Mode Switch vs Context Switch

This distinction is **very important**.

## Mode switch

Changes **privilege level**:

```
User
 ↓
Kernel
 ↓
User
```

The same process may continue running.

## Context switch

Changes **which process is running**:

```
Process A
    ↓
Process B
```

### Therefore:

```
Mode switch ≠ Context switch
```

Examples:

### `getpid()`

```
Process A
  ↓
user → kernel       ← mode switch
  ↓
kernel → user
  ↓
Process A
```

**No context switch.**

### `read()` with no data

```
Process A
  ↓
user → kernel
  ↓
A sleeps
  ↓
Process B runs
```

**Context switch occurs.**

### Timer tick with only one ready process

```
Process A
  ↓
timer interrupt
  ↓
kernel
  ↓
Process A
```

**No context switch.**

> A timer tick **may** cause a context switch, but does not always cause one.

---

# 15. Fast vs Slow System Calls

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

The kernel must wait for an external event.

Examples:

```
read()    // keyboard / disk / network
write()   // full pipe
wait()    // child hasn't exited
```

Instead of wasting CPU:

```
Process A
    ↓
slow syscall
    ↓
sleep
    ↓
Process B runs
```

When the event occurs:

```
External event
      ↓
Interrupt
      ↓
Wake A
      ↓
A becomes runnable
```

A sleeping process consumes **no CPU time**.

### Important distinction

A slow syscall does not necessarily mean:

> "The system call itself is computationally slow."

It means:

> **The process may have to wait for an external event.**

---

# 16. vDSO

System calls have overhead because they require:

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

For some operations, this transition is unnecessary.

Linux provides the:

> **vDSO — virtual dynamic shared object**

For certain time-related functions, libc can use the vDSO instead of trapping into the kernel.

```
gettimeofday()
      ↓
     vDSO
      ↓
read shared kernel-maintained data
```

No:

- Trap
    
- Mode switch
    
- Kernel entry
    

Therefore it behaves like a normal function call.

The worksheet gives an example where `gettimeofday()` took about **961 ns** through a real syscall versus **128 ns** through the vDSO.

## Why can't every syscall use vDSO?

Because many system calls require actual kernel work.

For example, `read()` may:

- Access an I/O device
    
- Block
    
- Use the process's kernel-managed file descriptor table
    

So:

> **vDSO works only for operations that can be answered from suitable kernel-maintained data without privileged work.**

---

# 17. Two System Programming Habits

## Habit 1 — If you provide a service

> **Validate every input from untrusted code.**

Example:

```
Kernel
  ↑
untrusted syscall arguments
```

The kernel must not assume the caller is correct.

This applies to:

- Kernels
    
- Browsers
    
- Databases
    
- Other system software
    

---

## Habit 2 — If you use a service

> **Assume every system call can fail.**

For example:

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

# 18. Why Error Checking Matters

Consider:

```
int fd = open("config.txt", O_RDONLY);
read(fd, buf, 100);
printf("config: %s\n", buf);
```

If the file doesn't exist:

```
open()
 ↓
-1 + errno = ENOENT
 ↓
ignored ❌

read(-1, ...)
 ↓
fails with EBADF
 ↓
ignored ❌

printf(buf)
 ↓
buf contains invalid/uninitialized data
 ↓
undefined behavior / possible crash
```

The lesson:

> **Check the return value of every system call before using its result.**

Also:

> For `read()`, use its return value to know how many bytes in the buffer are actually valid.

---

# 19. Key Comparisons

## Function Call vs System Call

|Function call|System call|
|---|---|
|User → user|User → kernel → user|
|Stays in user mode|Enters kernel mode|
|Uses user stack|Kernel uses kernel stack|
|Cheap|More expensive|
|Program chooses function address|Controlled kernel entry point|

---

## Trap vs Fault vs Interrupt vs Abort

```
TRAP
→ Program intentionally asks

FAULT
→ Current instruction causes a problem

INTERRUPT
→ External hardware event

ABORT
→ Serious unrecoverable failure
```

---

## Fast vs Slow System Call

```
FAST
→ Kernel can answer immediately
→ Process continues

SLOW
→ Must wait for external event
→ Process sleeps
→ Another process runs
→ Event wakes it
→ Process becomes runnable
```

---

## Mode Switch vs Context Switch

```
MODE SWITCH
→ Changes privilege level
→ User ↔ Kernel
→ Same process may continue

CONTEXT SWITCH
→ Changes process
→ Process A ↔ Process B
→ Saves/loads process context
```

---

# 20. Important Quiz Scenarios

These are worth being able to answer immediately.

### `getpid()`

**Mode switch?** Yes.  
**Context switch?** No.

```
A → kernel → A
```

---

### `read()` from keyboard with no key available

**Mode switch?** Yes.  
**Context switch?** Usually yes.

```
A → kernel → A sleeps → B runs
```

Later:

```
keyboard interrupt
      ↓
wake A
      ↓
A becomes runnable
```

---

### Timer tick with only one runnable process

**Interrupt?** Yes.  
**Context switch?** No.

```
A → kernel → A
```

---

### Timer tick with another runnable process

**Interrupt?** Yes.  
**Context switch?** May happen.

```
A → kernel → B
```

---

### First access to a newly allocated page

**Exception?** Fault.  
**Process killed?** No, if the kernel can handle it.

```
page fault
 → map page
 → retry instruction
 → continue
```

---

### `while(1){}`

**Cooperative scheduling:** can hang the machine.

**Preemptive scheduling:** timer interrupts periodically return control to the kernel.

---

# 21. Quiz Checklist

You should be able to explain **without looking at the notes**:

- Why the kernel treats every process as potentially buggy/malicious
    
- Why protection requires hardware
    
- User mode vs kernel mode
    
- What privileged operations user mode cannot perform
    
- Why root is still user mode
    
- Process virtual address space
    
- Stack grows down / heap grows up
    
- Why addresses are virtual
    
- What ASLR does
    
- Why the kernel is mapped into every address space
    
- Why every process needs its own kernel stack
    
- Function call vs system call
    
- Why a normal function call cannot directly call the kernel
    
- The five steps of a system call
    
- How syscall arguments are passed
    
- Why syscall arguments must be validated
    
- Why `printf()` may result in one `write()`
    
- The four types of exceptions
    
- Trap vs fault vs interrupt vs abort
    
- Why a page fault can be normal
    
- Why cooperative scheduling fails
    
- How timer interrupts enable preemptive scheduling
    
- What a context switch saves
    
- Where the context is stored
    
- Mode switch vs context switch
    
- Whether a timer tick always causes a context switch
    
- Fast vs slow system calls
    
- What happens when a process blocks
    
- How a blocked process becomes runnable again
    
- How the vDSO avoids a trap
    
- Why `read()` cannot simply use the vDSO
    
- Why system calls can fail
    
- Why return values must be checked
    

---

# 22. One-Minute Mental Model

```
                    HARDWARE
                       │
          ┌────────────┴────────────┐
          │                         │
      USER MODE                 KERNEL MODE
          │                         │
    Applications              Operating System
          │                         │
          │ system call             │
          ├──────────────→──────────┤
          │                         │
          │      mode switch        │
          │                         │
          ├────────────←────────────┤
          │                         │
          ▼                         │
      Application                  │
                                    │
                         timer interrupt
                                    │
                                    ▼
                               Scheduler
                                    │
                             context switch
                                    │
                                    ▼
                              Another process
```

### The whole worksheet in one chain

> **Programs run directly in user mode for speed.**

> **Hardware prevents them from doing dangerous things.**

> **System calls provide a controlled way to request kernel services.**

> **Exceptions and interrupts give the kernel a way to regain control.**

> **Timer interrupts prevent a process from monopolizing the CPU.**

> **Context switches let the kernel move the CPU from one process to another.**

> **Virtual address spaces isolate processes from each other.**

> **Slow system calls put waiting processes to sleep instead of wasting CPU.**

> **vDSO avoids the kernel transition for a small set of operations where it is safe to do so.**