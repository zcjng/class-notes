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

```
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

```
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

```
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

```
waitpid(pid, &status, 0);
```

- Waits for the specified child.
- If the child is still running, the parent **blocks** until it changes state.
- If the child has already terminated, its exit status is collected and the zombie is removed.

---

### `WNOHANG`

```
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

```
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

### Exam Cheat Sheet

```
Environment variable
→ Named information stored in a process's environment.

export
→ Makes a shell variable available in the environment inherited by children.

PATH
→ List of directories searched for executable programs.

echo $PATH
→ Displays the directories in PATH.

which python
→ Shows which Python executable is found first in PATH.

Built-in command
→ Runs inside the shell because it needs to modify the shell's own state.

External command
→ Usually shell: fork() → child: exec()

execvp()
→ libc function that searches PATH and then calls an exec system call.

execve()
→ Kernel system call that actually replaces the process with the new program.

libc
→ User-space C library; provides functions such as execvp().

Kernel
→ Performs the actual OS operation through system calls.
```

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

What does `fork()` return to the Parent process, and why?
??
It returns the actual, positive PID of the newly created child process (`> 0`) so the parent can track, manage, or wait for that specific child.
<!--SR:!2026-09-19,4,270-->

What does it mean when parent and child processes "execute different branches" of the same `if-else` statement?  
??  
They share the exact same source code file, but because `fork()` returns `0` to the child and `> 0` to the parent, the conditional variables dynamically force them into separate execution paths.

How does memory isolation behave between a parent and child process right after a `fork()`?  
??  
They have completely separate, sandboxed memory spaces in RAM; if the child or parent modifies a local variable, the other process cannot see the change.

What core transformation happens in RAM when a child process successfully executes `execvp()`?  
??  
The OS completely wipes out the current program code and variables inside that container, replacing it entirely with the machine code of a new executable (like `ls`).

Why does `execvp()` never return to execute any code lines written directly below it?  
??  
Because a successful `execvp()` entirely overwrites and erases the original program's memory space, leaving no original code left to execute.

Why must a command-line utility array passed to `execvp` always end with a `NULL` pointer?  
??  
It acts as a mandatory terminating sentinel so the OS kernel knows precisely where the argument list ends in RAM, preventing memory corruption crashes.

Why does a Shell need to use BOTH `fork()` and `execvp()` to run a command like `ls`?
??
If the Shell called `execvp()` directly without forking, the `ls` code would overwrite the Shell itself, causing your terminal window to instantly close when `ls` finished.
<!--SR:!2026-09-19,4,270-->

What two things happen when a parent process executes the `waitpid()` function?  
??
1. The parent blocks (freezes) execution until the target child process terminates.
2. The parent collects the child's final exit status from the OS Kernel.

What exact data does a child process pass to the OS Kernel upon calling exit(42)`?  
??  
It hands over its **Exit Status** (the integer `42`). The Kernel already knows the child's PID and pairs it with this exit code inside the system Process Table.

What defines a Zombie Process in an operating system?  
??  
A child process that has finished executing (`exit()`) but whose exit status has not yet been collected by its parent via `wait()`, leaving its metadata trapped in the Process Table.

What happens to a child's zombie status if the parent calls `wait()` _after_ the child has already exited?  
??  
The child becomes a zombie temporarily; the moment the parent finally calls `wait()`, the status is instantly collected and the zombie is wiped out of the Process Table.

What happens to a running child process (or a zombie child) if its parent process dies unexpectedly?  
??  
The child becomes an **Orphan** and is immediately adopted by **`init` (PID 1 / `systemd`)**, which continuously runs background `wait()` calls to clean them up.

If zombie processes occupy virtually zero RAM, why are they considered dangerous?  
??  
They hoard rows in the kernel's fixed-size **Process Table**. If a long-running app leaks thousands of zombies, it fills up the table and causes **PID Exhaustion**.

What is the consequence of PID Exhaustion on an operating system?  
??  
The OS hits its hard limit (`pid_max`) and cannot generate new PIDs; it will refuse to launch any new programs, open terminal commands, or handle new tabs, effectively freezing the system.

What are two primary system limitations that will cause a `fork()` call to fail and return `-1`?  
??

1. The computer runs completely out of physical memory (RAM).
2. The OS hits PID Exhaustion because the Process Table is entirely full.




#os 