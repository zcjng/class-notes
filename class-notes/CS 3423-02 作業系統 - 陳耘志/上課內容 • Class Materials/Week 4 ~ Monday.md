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

What are the two CPU execution modes?  
??  
User mode and kernel mode.

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
<!--SR:!2026-10-07,1,230-->

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
<!--SR:!2026-10-07,1,230-->

Does mapping the kernel into a process's address space mean the process can access the kernel?
??
No. The kernel's memory is protected so user-mode code cannot access it directly.
<!--SR:!2026-10-10,4,270-->

Does every process have its own kernel stack?
??
Yes. Each process has a separate kernel stack used when that process enters the kernel.
<!--SR:!2026-10-07,1,230-->

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

What is an abort?  
??  
An abort is a serious, unrecoverable error that generally cannot be restarted.

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

What is the main problem with cooperative scheduling?  
??  
A buggy or malicious process could keep running forever without yielding, preventing other processes from getting CPU time.

What is preemptive scheduling?
??
The operating system can forcibly interrupt a running process and take control of the CPU.
<!--SR:!2026-10-07,1,230-->

What mechanism allows preemptive scheduling?
??
A hardware timer periodically generates interrupts, allowing the kernel to regain control of the CPU.
<!--SR:!2026-10-07,1,230-->

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

Does every timer interrupt cause a context switch?
??
No. A timer interrupt gives the kernel an opportunity to schedule, but the scheduler may decide to continue running the same process.
<!--SR:!2026-10-07,1,230-->

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