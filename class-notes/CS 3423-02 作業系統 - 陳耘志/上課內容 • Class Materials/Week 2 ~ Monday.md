---
title: 第二週 • 週一
course: CS 3423-02 作業系統
week: 2
day: Monday
tags:
  - os
---

# Operating Systems: Process Management (`fork` + `exec`)

## 1. Core Mechanics of `fork()`
* **Definition:** `fork()` clones the active parent process to create an identical **child process**.
* **Memory Separation:** The parent and child possess **completely separate, sandboxed memory spaces**. Modifying variables in one process has zero effect on the other.
* **The Return Value Trick:** Both processes run the *exact same code file* but split at conditional blocks because `fork()` returns different values:
  * **Parent Process:** Receives the **actual PID of the child** (`> 0`).
  * **Child Process:** Receives `0` (used purely as a flag meaning *"You are the child"*).
  * **Failure:** Returns `-1` if the OS runs out of memory or process slots.

---

## 2. The `fork()` + `exec()` Pattern
* **`execvp()` Mechanic:** Wipes out the current process's memory space and replaces it entirely with a brand-new program (e.g., `ls`). If successful, `execvp()` **never returns**.
* **The Shell Example (`ls`):**
  1. The Shell **forks** itself into a dummy child container.
  2. The child calls **`execvp("ls", args)`**, replacing its shell code with the `ls` program.
  3. This prevents the primary Shell process from being overwritten and terminated.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <stdlib.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        return 1;
    } 
    else if (pid == 0) {
        // --- CHILD PROCESS ---
        // 1. Define command and arguments (Must end with NULL)
        char *args[] = {"ls", "-l", NULL};
        
        // 2. Transform the child into the 'ls' program
        execvp(args[0], args); 
        
        // 3. This only executes if execvp fails to find/run the program
        perror("execvp failed!"); 
        exit(1); 
    } 
    else {
        // --- PARENT PROCESS ---
        int status;
        waitpid(pid, &status, 0); // Freezes parent until child finishes
    }
    return 0;
}
```

### Deep Dive: Deconstructing `execvp(args[0], args)`
The `vp` suffix stands for **V**ector (accepts an array) and **P**ath (auto-searches environment directories like `/bin/` or `/usr/bin/` so you don't have to specify absolute paths).

1. **`char *args[] = {"ls", "-l", NULL};`**
   * `args[0]` ("ls"): By standard convention, the first index must be the name of the executable itself.
   * `"-l"`: The target flags/arguments passed directly to the program.
   * `NULL`: A mandatory terminating sentinel. Tells the OS precisely where the arguments end in RAM.
2. **`execvp(args[0], args);`**
   * First input: The name of the file to execute (`"ls"`).
   * Second input: The complete array of configuration strings to feed into that program.

#### RAM Transformation during `execvp`:
### RAM Overwrite Process Comparison

| Memory Stage        | What's Inside the Process Container                                                                                                                            | What Happens to the Code?                                                                                                       |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------ |
| **BEFORE `execvp`** | <ul><li>Your custom compiled C code</li><li>Your local variables (`pid`, `args`)</li><li>Active stack and heap states</li></ul>                                | The child process runs identically to the parent, up until it hits the `execvp` function call.                                  |
| **THE TRANSITION**  | 💥 **Complete Memory Wipe**                                                                                                                                    | The OS destroys everything inside the container's RAM allocation except for the process's base file descriptors.                |
| **AFTER `execvp`**  | <ul><li>The compiled machine code of `ls`</li><li>New variables initiated by the `ls` tool</li><li>Fresh execution frame starting at `ls`'s `main()`</li></ul> | The program runs `ls -l`. Any lines of code written below `execvp` in your original file are completely gone and never execute. |


---

## 3. Asynchronous Execution & Zombie Processes
When `waitpid()` is omitted, the parent and child execute **asynchronously** (independently at the same time). 

### How Zombies Form
A child process **always** becomes a temporary zombie the exact millisecond it terminates. It uses `exit(code)` (e.g., `exit(42)`) to pass its exit status back.

* **Communication Middleman:** Because memory is separated, the **OS Kernel** catches the exit code and stores it in the system **Process Table**.
* **The Lifespan of a Zombie:**
  * **Parent calls `wait()` first:** The parent blocks. The child exits, the status is handed over immediately, and the zombie lasts less than a microsecond.
  * **Parent calls `wait()` later:** The child remains a zombie (`<defunct>`) inside the Process Table until the parent executes the `wait()` line.
  * **Parent never calls `wait()`:** The zombie remains permanently trapped in the table. If too many accumulate, the OS runs out of PIDs and crashes.

### Automatic OS Cleanup (Reparenting)
If the parent process dies while a child is still a zombie or running, the OS **reparents** the orphaned child to **`init` (PID 1 / `systemd`)**. PID 1 runs an infinite background loop constantly calling `wait()`, instantly clearing the zombie out of the Process Table.


### Connection: Why `fork()` Fails (`-1`) via PID Exhaustion
The finite size of the kernel's **Process Table** links `fork()` failures directly to zombie accumulation:

* **The Mechanism:** When `fork()` is called, the kernel must assign the new child a unique Process ID (PID) and allocate a free row in its internal Process Table.
* **The Failure Trigger:** If a long-running parent leaks thousands of uncleaned **Zombie Processes**, those dead entries continue hoarding slots in the table. 
* **The Crash Point:** Once the table hits its hard system limit (`pid_max`), the OS experiences **PID Exhaustion**. 
* **The Outcome:** Any subsequent calls to `fork()` will instantly fail, returning **`-1`** and triggering errors like *“Resource temporarily unavailable”* or *“No more processes”*, because the OS literally has no empty seats left to accommodate a new child.

## Daemon, Orphan, and Zombie Processes

### Orphan Process

An **orphan process** is a process whose **parent process has terminated while the child is still running**.

```
Parent
   │
   └── Child
```

If the parent dies:

```
Parent 💀

Child
```

The child becomes an **orphan** and is **reparented to PID 1** (usually `systemd` on modern Linux).

```
systemd (PID 1)
   │
   └── Child
```

The child **continues running normally**. If it eventually terminates, PID 1 can reap it.

> **Orphan = parent dies first, child is still alive.**

---

### Zombie Process

A **zombie process** is a process that has **already terminated**, but its parent has **not yet called `wait()`/`waitpid()`** to collect its exit status.

```
Parent
   │
   └── Child 💀
          ↑
       Zombie
```

The child is no longer executing. However, the kernel keeps a small amount of information about it, such as:

- PID
- Exit status
- Process accounting information

This allows the parent to retrieve the child's termination status using `wait()`.

Once the parent calls:

```c
wait(NULL);
```

the zombie is removed from the process table.

> **Zombie = child dies first, but parent hasn't reaped it yet.**

### Why are zombies a problem?

A zombie does **not** consume CPU and does not occupy the normal memory resources of a running process, but it still occupies a **process-table/PID entry**.

If a buggy parent continually creates children without calling `wait()`:

```
Parent
 ├── Zombie
 ├── Zombie
 ├── Zombie
 ├── Zombie
 ├── Zombie
 └── ...
```

Eventually, the system can run out of available process/PID entries, preventing new processes from being created.

---

### What if the parent dies while the child is a zombie?

Consider:

```
Parent
   │
   └── Zombie 💀
```

If the parent then terminates:

```
Parent 💀
   │
   └── Zombie 💀
```

The zombie is reparented to **PID 1**:

```
systemd (PID 1)
   │
   └── Zombie 💀
```

PID 1 can then **reap the zombie**, removing its process-table entry.

So a forgotten zombie does not necessarily remain forever.

---

## Daemon vs. Orphan vs. Zombie

These terms describe **different things**:

|Type|Parent|Process itself|Meaning|
|---|---|---|---|
|**Orphan**|Has terminated|Still running|Child has lost its original parent|
|**Zombie**|Still exists but hasn't reaped child|Already terminated|Exit information is waiting to be collected|
|**Daemon**|Usually managed independently|Running in background|A process designed to provide a background service|

An **orphan** and a **zombie** are process/lifecycle conditions, whereas a **daemon** describes the **purpose and behavior** of a process.

### Easy way to remember

```
ORPHAN:
Parent dies → Child lives
                 ↓
              PID 1 adopts


ZOMBIE:
Child dies → Parent hasn't wait()
                 ↓
             Dead entry remains


DAEMON:
Process designed to run
in the background as a service
```

---

## Traditional Daemon Creation — Double Fork

The traditional Unix daemonization technique commonly uses:

```
fork()
  ↓
setsid()
  ↓
fork()
```

### Why the first `fork()`?

The original parent exits, allowing the child to become independent of the original process hierarchy.

### Why `setsid()`?

`setsid()` creates a **new session**, separating the process from the old session and its controlling terminal.

```
Before:

Terminal
   │
   └── Shell
        │
        └── Process
             ↑
        old session


After setsid():

Terminal ── Shell ── old session

Process
   ↑
new session
```

### Why fork a second time?

After `setsid()`, the process becomes a **session leader**.

A session leader can potentially acquire a controlling terminal. The second `fork()` creates a child that is **not a session leader**.

```
fork()
  ↓
setsid()
  ↓
Become session leader
  ↓
fork() again
  ↓
Final daemon is NOT a session leader
```

This prevents the final daemon from accidentally acquiring a controlling terminal.

### Important distinction

Simply doing:

```c
fork();
fork();
```

can create a grandchild and eventually an orphan, but **it does not properly detach the process from the original terminal/session**.

`setsid()` is what performs the important **session detachment**.

---

## Modern Linux: `systemd`

On modern Linux systems, applications generally **do not need to implement traditional daemonization themselves**.

Instead, `systemd` can manage the program as a service:

```
systemd
   │
   └── Your program
```

The program can simply run normally in the foreground, while `systemd` handles:

- Starting it
- Stopping it
- Restarting it
- Managing its lifetime
- Running it in the background
- Logging
- Dependencies

So the traditional double-fork technique is mainly important for understanding **how Unix daemonization works**, while `systemd` is the modern way to manage system services.


## Shell Background Jobs

A command followed by `&` runs as a **background job**:

```
sleep 10 &
```

The shell roughly:

```
shell
  │
  └── fork()
       ↓
     child
       ↓
     exec("sleep")
```

For a foreground command, the shell waits:

```c
waitpid(child, &status, 0);
```

For a background command, the shell **does not wait immediately**, so it can continue accepting commands and display the prompt.

---

### Child Termination → Zombie

When the child finishes:

```
Child
  │
  └── exit()
       ↓
   terminates
```

The child is **dead**, but the kernel temporarily keeps a small process-table entry containing information such as its exit status.

This is a **zombie**.

```
Running → exit() → Zombie → waitpid() → removed
```

> **Zombie = terminated child whose parent has not yet collected its exit status.**

---

### `waitpid()`

```c
waitpid(pid, &status, 0);
```

- Waits for the specified child.
- If the child is still running, the parent **blocks** until it changes state.
- If the child has already terminated, its exit status is collected and the zombie is removed.

---

### `WNOHANG`

```c
waitpid(pid, &status, WNOHANG);
```

`WNOHANG` means:

> **Don't block if the child hasn't finished.**

```
Child finished?
   │
   ├── YES → collect status
   │
   └── NO  → return immediately
```

This is useful for shells because the shell **must remain responsive** instead of waiting for background jobs.

---

## `SIGCHLD`

When a child terminates, the kernel can send **`SIGCHLD`** to its parent.

```
Child terminates
      ↓
Kernel
      ↓
SIGCHLD
      ↓
Parent
```

`SIGCHLD` is a **notification**, not the exit status itself.

The parent can install a **signal handler** to respond to it:

```
SIGCHLD received
      ↓
Signal handler runs
      ↓
waitpid(..., WNOHANG)
      ↓
Collect child's status
      ↓
Zombie removed
```

> **Signal = notification**  
> **Signal handler = code that responds to the notification**  
> **`waitpid()` = collects the child's termination information and reaps it**

---

### Overall Picture

```
$ sleep 10 &
       │
       ↓
     fork()
       │
       ├──────────────→ Shell continues
       │
       ↓
     Child
       │
       ↓
     exec()
       │
       ↓
   runs in background
       │
       ↓
     exit()
       │
       ↓
    Zombie
       │
       ↓
 Kernel sends SIGCHLD
       │
       ↓
 Parent's signal handler
       │
       ↓
 waitpid(..., WNOHANG)
       │
       ↓
 Exit status collected
       │
       ↓
 Zombie removed
```

**Key idea:** `&` makes the shell **not wait immediately**; `SIGCHLD` can notify the shell when the child finishes; `waitpid()` reaps the finished child; `WNOHANG` makes sure the shell doesn't block while checking.

## Environment Variables

An **environment variable** is a named piece of information stored in a process's environment and inherited by its child processes.

Examples:

```
HOME=/home/user
USER=user
PATH=/usr/local/bin:/usr/bin:/bin
```

View a variable:

```
echo $HOME
echo $PATH
```

`export` makes a shell variable part of the environment inherited by child processes:

```
export NAME=John
```

```
Shell
  │
  ├── NAME=John
  │
  └── fork() → Child
                  ↓
             inherits NAME=John
```

> A child inherits the parent's environment, but changes to the child's environment do not change the parent's environment.

---

## `PATH`

`PATH` is a special **environment variable containing a list of directories where executable programs can be found**.

```
echo $PATH
```

Example:

```
/home/user/.local/bin:/usr/local/bin:/usr/bin:/bin
```

The `:` separates directories.

When you type:

```
python
```

the shell/libc can search the directories in `PATH` from **left to right**:

```
/home/user/.local/bin/python   ❌
/usr/local/bin/python          ❌
/usr/bin/python                ✅
```

The first matching executable is used.

### `which`

```
which python
```

might output:

```
/usr/bin/python
```

This shows which executable is found first through the `PATH` search.

> **`PATH` = directories to search for executable commands.**

`PATH` does **not** contain the executables themselves; it contains the **directories containing them**.

---

## Shell Built-in Commands

A **shell built-in** is a command implemented directly inside the shell rather than being a separate executable.

Some important built-ins:

```
cd
export
unset
exit
alias
source / .
```

### Why do these need to be built into the shell?

Because they need to modify the **current shell's state**.

For example:

```
cd /tmp
```

If the shell did:

```
fork()
  ↓
Child
  ↓
cd /tmp
```

only the child's working directory would change:

```
Shell       → /home/user
Child       → /tmp
```

The child eventually exits, so the shell would still be in `/home/user`.

Therefore `cd` must execute inside the **current shell process**.

The same idea applies to:

```
export → changes shell's environment
unset  → changes shell's environment
alias  → changes shell's aliases
exit   → terminates the shell itself
source → executes commands in the current shell
```

---

## External Commands

Commands that don't need to modify the current shell's state can normally be run as separate processes.

Examples:

```
ls
cat
grep
gcc
python
sleep
```

The shell roughly does:

```
Shell
  │
  ├── fork()
  │
  └── Child
       │
       └── exec(...)
```

For a foreground command, the shell then waits:

```
fork()
  ↓
Child → exec("ls")
  ↓
waitpid()
  ↓
Shell continues
```

For a background command:

```
sleep 10 &
```

the shell does **not immediately wait**, so it can continue accepting commands.

---

## `execvp()` and `PATH`

`execvp()` is a **libc function**.

For example:

```c
execvp("python", argv);
```

The `p` means it performs a **`PATH` search**.

Conceptually:

```
execvp("python")
       │
       ↓
      libc
       │
       ├── /home/user/.local/bin/python  ❌
       ├── /usr/local/bin/python         ❌
       └── /usr/bin/python               ✅
                                     
       ↓
execve("/usr/bin/python", ...)
       ↓
     Kernel
       ↓
actually executes /usr/bin/python
```

So `execvp()` is doing the **PATH searching in user space**.

---

## libc vs. Kernel

### libc

**libc (C standard library)** provides convenient functions that programs can call.

For example:

```
execvp()
printf()
malloc()
```

Some libc functions eventually make **system calls** to the kernel.

### Kernel

The kernel is responsible for the actual OS-level operations.

For executing a program, the kernel provides the `execve` **system call**.

```
Program
   ↓
libc: execvp()
   ↓
PATH search in user space
   ↓
libc: execve(...)
   ↓
system call
   ↓
Kernel
   ↓
execute program
```

The kernel **doesn't search `$PATH`**.

It receives an actual pathname such as:

```
/usr/bin/python
```

and executes that file.

> **`PATH` searching is user-space functionality provided by programs such as shells and libc's `execvp()`.**

---

## Big Picture

```
                    User types:
                  python program.py
                          │
                          ↓
                        Shell
                          │
                    Search PATH
                          │
                  Find /usr/bin/python
                          │
                        fork()
                          │
                          ↓
                       Child
                          │
                     execvp() / execve()
                          │
                          ↓
                        Kernel
                          │
                          ↓
                   Python executes
```


## Fork Process Counting

![[Pasted image 20260915175726.png]]

### Key Rule

After `fork()`, **both the parent and child continue executing from the next line**.

Each `fork()` doubles the number of processes that reach the next statement.

```text
1st fork → 2 processes
2nd fork → 4 processes
3rd fork → 8 processes
````

Therefore:

Total processes=2n\text{Total processes} = 2^n

where `n` is the number of `fork()` calls executed by each process.

> **Important:** If the question asks "how many processes are created," distinguish between:
> 
> - **Total processes:** includes the original process
> - **New processes created:** total − 1

---

### Example

```c
char *letters = "ABC";

for (int i = 0; i < 3; i++) {
    printf("Process %d prints: %c\n", getpid(), letters[i]);
    fork();
}

printf("Process %d finished!\n", getpid());
```

#### `i = 0`

There is initially **1 process**.

```c
printf("A")
    ↓
fork()
    ↓
2 processes
```

`A` is printed **1 time**.

#### `i = 1`

Both processes continue the loop.

```
2 processes
    ↓
both print "B"
    ↓
both fork()
    ↓
4 processes
```

`B` is printed **2 times**.

#### `i = 2`

There are now 4 processes.

```
4 processes
    ↓
all print "C"
    ↓
all fork()
    ↓
8 processes
```

`C` is printed **4 times**.

After the loop, all 8 processes execute:

```c
printf("Process %d finished!\n", getpid());
```

Therefore:

```
A           → 1 time
B           → 2 times
C           → 4 times
finished!   → 8 times
```

### Answer

- **Total processes:** 8
- **New processes created:** 7
- **`C` printed:** 4 times
- **`finished!` printed:** 8 times

---

## `fork()` Return Value

`fork()` returns different values to the parent and child:

```
Parent → returns child's PID (> 0)
Child  → returns 0
```

This allows the processes to determine whether they are the parent or child.

---

## Example: Two `fork()` Calls and Conditions

![[Pasted image 20260915175740.png]]

```c
pid_t pid1 = fork();
pid_t pid2 = fork();

if (pid1 != 0 && pid2 != 0) {
    printf("Meow!\n");
}

if (pid2 != 0) {
    printf("Ah!\n");
}
```

### After the first `fork()`

```
        Original
        /      \
       /        \
    Parent      Child
   pid1 > 0    pid1 = 0
```

There are 2 processes.

### After the second `fork()`

**Both processes execute the second `fork()`**, so:

```
             Original
            /        \
           /          \
          A            B
        /   \        /   \
       A     C      B     D
```

There are now **4 processes**.

Their `pid1` and `pid2` values are:

|Process|`pid1`|`pid2`|
|---|---|---|
|A|> 0|> 0|
|B|0|> 0|
|C|> 0|0|
|D|0|0|

### `"Meow!"`

Condition:

```
pid1 != 0 && pid2 != 0
```

Both must be nonzero.

Only process A satisfies this.

Therefore:

> **`Meow!` → 1 time**

### `"Ah!"`

Condition:

```
pid2 != 0
```

Processes A and B satisfy this.

Therefore:

> **`Ah!` → 2 times**

### Final Answer

```
Meow! → 1 time
Ah!   → 2 times
```

---

## Quick Fork Rules

### Number of processes

```
n fork() calls
→ up to 2ⁿ total processes
```

### Number of new processes

```
2ⁿ - 1
```

### Printing before `fork()`

```c
printf("X");
fork();
```

The `printf()` happens **before** the process is duplicated.

So the number of prints is based on the number of processes **before** that `fork()`.

### Printing after `fork()`

```c
fork();
printf("X");
```

Both parent and child reach the `printf()`, so the number of prints is doubled.

> **Always ask: "How many processes reach this line?"**  
> That tells you how many times that line executes.



---
### Flashcards

What does `fork()` do at the Operating System level?  
??  
It commands the OS to make a complete, 100% identical clone of the currently running process in RAM, starting from the exact line where `fork()` was called.

How does modern Chrome utilize process separation when you open a new tab?  
??  
It uses a multi-process architecture where opening a new tab forks an isolated Renderer Process, ensuring that if one tab crashes, the rest of the browser remains stable.

What does a `fork()` return value of `0` signify to the running program?
??
It acts as a flag telling that specific process container: "You are the child process." (The child's actual PID is positive, but `fork()` returns `0` inside its code execution path).
<!--SR:!2026-09-26,4,270-->

What does `fork()` return to the Parent process, and why?
??
It returns the actual, positive PID of the newly created child process (`> 0`) so the parent can track, manage, or wait for that specific child.
<!--SR:!2026-10-07,15,290-->

What does it mean when parent and child processes "execute different branches" of the same `if-else` statement?
??
They share the exact same source code file, but because `fork()` returns `0` to the child and `> 0` to the parent, the conditional variables dynamically force them into separate execution paths.
<!--SR:!2026-09-26,4,270-->

How does memory isolation behave between a parent and child process right after a `fork()`?
??
They have completely separate, sandboxed memory spaces in RAM; if the child or parent modifies a local variable, the other process cannot see the change.
<!--SR:!2026-09-26,4,270-->

What core transformation happens in RAM when a child process successfully executes `execvp()`?
??
The OS completely wipes out the current program code and variables inside that container, replacing it entirely with the machine code of a new executable (like `ls`).
<!--SR:!2026-09-23,1,230-->

Why does `execvp()` never return to execute any code lines written directly below it?
??
Because a successful `execvp()` entirely overwrites and erases the original program's memory space, leaving no original code left to execute.
<!--SR:!2026-10-02,10,270-->

Why must a command-line utility array passed to `execvp` always end with a `NULL` pointer?
??
It acts as a mandatory terminating sentinel so the OS kernel knows precisely where the argument list ends in RAM, preventing memory corruption crashes.
<!--SR:!2026-09-23,1,230-->

Why does a Shell need to use BOTH `fork()` and `execvp()` to run a command like `ls`?
??
If the Shell called `execvp()` directly without forking, the `ls` code would overwrite the Shell itself, causing your terminal window to instantly close when `ls` finished.
<!--SR:!2026-10-06,14,290-->

What two things happen when a parent process executes the `waitpid()` function?
??
1. The parent blocks (freezes) execution until the target child process terminates.
2. The parent collects the child's final exit status from the OS Kernel.
<!--SR:!2026-09-25,3,250-->

What exact data does a child process pass to the OS Kernel upon calling exit(42)`?  
??  
It hands over its **Exit Status** (the integer `42`). The Kernel already knows the child's PID and pairs it with this exit code inside the system Process Table.

What defines a Zombie Process in an operating system?
??
A child process that has finished executing (`exit()`) but whose exit status has not yet been collected by its parent via `wait()`, leaving its metadata trapped in the Process Table.
<!--SR:!2026-09-26,4,270-->

What happens to a child's zombie status if the parent calls `wait()` _after_ the child has already exited?
??
The child becomes a zombie temporarily; the moment the parent finally calls `wait()`, the status is instantly collected and the zombie is wiped out of the Process Table.
<!--SR:!2026-09-26,4,270-->

What happens to a running child process (or a zombie child) if its parent process dies unexpectedly?
??
The child becomes an **Orphan** and is immediately adopted by **`init` (PID 1 / `systemd`)**, which continuously runs background `wait()` calls to clean them up.
<!--SR:!2026-09-26,4,270-->

If zombie processes occupy virtually zero RAM, why are they considered dangerous?
??
They hoard rows in the kernel's fixed-size **Process Table**. If a long-running app leaks thousands of zombies, it fills up the table and causes **PID Exhaustion**.
<!--SR:!2026-09-26,4,270-->

What is the consequence of PID Exhaustion on an operating system? 
??  
The OS hits its hard limit (`pid_max`) and cannot generate new PIDs; it will refuse to launch any new programs, open terminal commands, or handle new tabs, effectively freezing the system.

What are two primary system limitations that will cause a `fork()` call to fail and return `-1`?
??
1. The computer runs completely out of physical memory (RAM).
2. The OS hits PID Exhaustion because the Process Table is entirely full.
<!--SR:!2026-09-26,4,270-->

What does `fork()` do at the Operating System level?
??
It creates a new child process by cloning the calling parent process.
<!--SR:!2026-09-26,4,270-->

How does `fork()` return differently to the parent and child?
??
The parent receives the child's PID (> 0), while the child receives 0.
<!--SR:!2026-09-26,4,270-->

What does a `fork()` return value of `0` signify?
??
It means that the process receiving the return value is the child process.
<!--SR:!2026-09-26,4,270-->

What does `fork()` return to the parent?
??
It returns the actual PID of the newly created child process (> 0).

What happens if `fork()` fails?
??
It returns `-1`, typically because the OS cannot allocate the resources needed to create a new process.
<!--SR:!2026-09-23,1,230-->

How does memory isolation behave between a parent and child after `fork()`?
??
The parent and child have separate memory spaces, so modifying a variable in one process does not modify the other.
<!--SR:!2026-09-26,4,270-->

Why can parent and child execute different branches of the same `if-else` after `fork()`?
??
They execute the same code, but `fork()` returns different values in the parent and child, allowing the program to distinguish them.
<!--SR:!2026-09-23,1,230-->

What does `exec()` do?
??
It replaces the current process's program image with another program.

What happens to the original program after a successful `exec()`?
??
Its program code and execution state are replaced by the new program, so the original code below `exec()` does not execute.

Why does `execvp()` not return if it succeeds?
??
Because the current process has been replaced by the new program.
<!--SR:!2026-09-25,3,250-->

Why does a shell use both `fork()` and `execvp()` to run an external command?
??
The shell forks a child so that the child can be replaced by the new program without replacing the shell itself.
<!--SR:!2026-09-26,4,270-->

What happens if the shell directly calls `execvp()` to run `ls`?
??
The shell itself would be replaced by `ls`, so the original shell would no longer be running.

What does the `p` in `execvp()` mean?
??
It means `execvp()` performs a `PATH` search to find the executable.
<!--SR:!2026-09-23,1,230-->

What does the `v` in `execvp()` mean?
??
It means the arguments are provided as a vector/array.
<!--SR:!2026-09-23,1,230-->

What is `libc`?
??
libc is the C standard library that provides user-space functions such as `printf()`, `malloc()`, and `execvp()`.
<!--SR:!2026-09-25,3,250-->

Does the kernel search `PATH` when executing a program?
??
No. `PATH` searching is performed in user space by programs such as the shell or libc's `execvp()`. The kernel receives an actual pathname.
<!--SR:!2026-09-23,1,230-->

What is an environment variable?
??
A named piece of information stored in a process's environment that can be inherited by child processes.
<!--SR:!2026-09-26,4,270-->

What does `export` do?
??
It makes a shell variable part of the environment inherited by child processes.

What is `PATH`?
??
`PATH` is an environment variable containing a list of directories where executable programs can be found.
<!--SR:!2026-10-04,12,270-->

What does `echo $PATH` show?
??
It shows the directories listed in the `PATH` environment variable, separated by `:`.
<!--SR:!2026-10-04,12,270-->

How does the shell find `python` when the user types `python`?
??
It searches the directories in `PATH` from left to right until it finds an executable named `python`.

What does `which python` do?
??
It shows which `python` executable is found first through the `PATH` search.

Does `PATH` contain executable files?
??
No. `PATH` contains directories that contain executable files.
<!--SR:!2026-09-26,4,270-->

What is the difference between `/usr/bin` and `~/.local/bin`?
??
`/usr/bin` is generally a system-wide directory containing programs available to users. `~/.local/bin` is a directory inside a user's home directory for programs installed for that user.

What is a shell built-in command?
??
A command implemented directly inside the shell rather than being a separate executable.

Why does `cd` need to be a shell built-in?
??
`cd` must change the current shell's working directory. If it ran in a child process, only the child's directory would change.
<!--SR:!2026-09-26,4,270-->

What happens if `cd /tmp` were executed only in a child process?
??
The child would change to `/tmp`, but the parent shell would remain in its original directory after the child exits.
<!--SR:!2026-09-26,4,270-->

What are important examples of shell built-in commands?
??
`cd`, `export`, `unset`, `exit`, `alias`, and `source`/`.`.
<!--SR:!2026-09-26,4,270-->

Why does `exit` need to be a shell built-in?
??
It needs to terminate the current shell itself. If it ran in a child, only the child would terminate.
<!--SR:!2026-09-26,4,270-->

Why is `source` or `.` a shell built-in?
??
It executes commands inside the current shell process, allowing those commands to modify the shell's state.

What are examples of external commands?
??
`ls`, `cat`, `grep`, `gcc`, `python`, and `sleep`.
<!--SR:!2026-09-23,1,230-->

How does a shell normally execute an external command?
??
The shell uses `fork()` to create a child, and the child uses `exec()` to replace itself with the external program.
<!--SR:!2026-09-26,4,270-->

What is a foreground command?
??
A command for which the shell waits for the child process to finish before continuing.

What is a background command?
??
A command followed by `&` that allows the shell to continue running without immediately waiting for the child.
<!--SR:!2026-10-03,11,270-->

What does `&` do in a shell command?
??
It tells the shell to run the command as a background job, so the shell does not immediately wait for the child.

What happens when a background child finishes?
??
The child terminates and can temporarily become a zombie until the parent reaps it.

What is a zombie process?
??
A child process that has already terminated, but whose parent has not yet collected its exit status using `wait()` or `waitpid()`.
<!--SR:!2026-09-26,4,270-->

Does a zombie process still execute?
??
No. It has already terminated. Only a small amount of information about it remains in the kernel's process table.

What information does the kernel keep for a zombie?
??
Information such as the child's PID and exit status.
<!--SR:!2026-09-25,3,250-->

Why does the kernel keep a terminated child's exit status?
??
So that the parent can retrieve the child's termination information using `wait()` or `waitpid()`.
<!--SR:!2026-09-26,4,270-->

What happens when the parent calls `wait()` or `waitpid()` on a zombie?
??
The parent collects the child's exit status and the kernel removes the zombie's process-table entry.
<!--SR:!2026-10-02,10,270-->

What is an orphan process?
??
A process whose parent has terminated while the process itself is still running.

What happens to an orphan process?
??
It is reparented to PID 1, usually `systemd` on modern Linux systems.
<!--SR:!2026-09-26,4,270-->

What is the difference between an orphan and a zombie?
??
An orphan is still running but its parent has died. A zombie has already terminated but its parent has not yet reaped it.

What happens if the parent dies while its child is a zombie?
??
The zombie is reparented to PID 1, which can reap it and remove its process-table entry.
<!--SR:!2026-09-26,4,270-->

What is a daemon process?
??
A process designed to run in the background and provide a service.

What is the traditional Unix technique for creating a daemon?
??
The traditional technique commonly uses `fork()` → `setsid()` → `fork()`.
<!--SR:!2026-09-26,4,270-->

Why is the first `fork()` used during traditional daemonization?
??
It allows the original parent to exit and the child to become independent of the original process hierarchy.
<!--SR:!2026-09-23,1,210-->

What does `setsid()` do?
??
It creates a new session, separating the process from the old session and its controlling terminal.

Why is the second `fork()` used during traditional daemonization?
??
It creates a child that is not a session leader, preventing the final daemon from accidentally acquiring a controlling terminal.

Why isn't `fork(); fork();` alone enough to properly daemonize a process?
??
It can create a grandchild, but it does not properly detach the process from the original session and controlling terminal. `setsid()` performs the important session detachment.
<!--SR:!2026-09-25,3,250-->

What does `waitpid(pid, &status, 0)` do?
??
It waits for the specified child. If the child is still running, the parent blocks until the child changes state, then collects its status.
<!--SR:!2026-10-03,11,270-->

What does `WNOHANG` mean?
??
It tells `waitpid()` not to block if the child has not finished.
<!--SR:!2026-09-26,4,270-->

What happens when `waitpid(pid, &status, WNOHANG)` finds that the child is still running?
??
It returns immediately instead of making the parent wait.

Why is `WNOHANG` useful for shells?
??
It allows the shell to check whether a background child has finished without blocking the shell.

What is `SIGCHLD`?
??
A signal that the kernel can send to a parent when one of its children changes state, commonly when the child terminates.
<!--SR:!2026-09-23,1,230-->

Is `SIGCHLD` the child's exit status?
??
No. `SIGCHLD` is a notification. The parent uses `wait()` or `waitpid()` to collect the child's exit status.
<!--SR:!2026-09-26,4,270-->

What is a signal handler?
??
Code registered by a process to respond when the process receives a particular signal.

What can a parent do when it receives `SIGCHLD`?
??
It can run a signal handler that responds to the notification, commonly by calling `waitpid()` to reap terminated children.

Does receiving `SIGCHLD` automatically call `waitpid()`?
??
No. `SIGCHLD` is only a notification. The program must choose how to respond, such as by calling `waitpid()`.

What is the relationship between `SIGCHLD` and `waitpid()`?
??
`SIGCHLD` tells the parent that a child changed state, while `waitpid()` allows the parent to collect the child's termination information and reap it.

How many total processes can result from `n` independent `fork()` calls if every process reaches every `fork()`?
??
Up to `2^n` total processes, including the original process.
<!--SR:!2026-09-23,1,210-->

How many new processes are created by `n` independent `fork()` calls if every process reaches every `fork()`?
??
`2^n - 1` new processes, because the original process is included in the total.

How do you determine how many times a line executes in a `fork()` counting problem?
??
Ask: "How many processes reach this line?" Every process that reaches the line executes it.

What happens if `printf()` appears before a `fork()`?
??
That `printf()` executes before the process is duplicated, so that fork does not double the number of times that particular `printf()` executes.
<!--SR:!2026-10-04,12,270-->

What happens if `printf()` appears after a `fork()`?
??
Both the parent and child continue from the next line, so both execute the `printf()`.
<!--SR:!2026-09-25,3,250-->

If a loop has 3 `fork()` calls and every process reaches every call, how many total processes can exist?
??
`2^3 = 8` total processes.

If a loop has 3 `fork()` calls and 8 total processes exist, how many new processes were created?
??
7 new processes, because the original process is included in the 8 total.
<!--SR:!2026-09-23,1,190-->




#os 