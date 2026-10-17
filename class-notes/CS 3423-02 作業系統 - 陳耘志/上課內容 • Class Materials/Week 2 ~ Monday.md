---
title: 第二週 • 週一
course: CS 3423-02 作業系統
week: 2
day: Monday
tags:
  - os
---

# 第二週 • 週一 — Process Management (`fork` + `exec`)

## 1. Core Mechanics of `fork()`

- **Definition:** `fork()` clones the active parent process to create an identical **child process**.
- **Memory separation:** parent and child possess **completely separate memory spaces**. Modifying variables in one process has zero effect on the other.
- **The return value trick:** both processes run the *exact same code* but can split at conditional blocks, because `fork()` returns different values:
  - **Parent:** the **actual PID of the child** (`> 0`).
  - **Child:** `0` — a flag meaning *"you are the child"*.
  - **Failure:** `-1` if the OS runs out of memory or process slots.

## 2. The `fork()` + `exec()` Pattern

- **`execvp()` mechanic:** wipes out the current process's memory and replaces it entirely with a brand-new program (e.g. `ls`). If successful, `execvp()` **never returns**.
- **The shell example (`ls`):**
  1. The shell **forks** itself into a dummy child container.
  2. The child calls **`execvp("ls", args)`**, replacing its shell code with the `ls` program.
  3. This prevents the primary shell process from being overwritten and terminated.

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
        // 1. Define command and arguments (must end with NULL)
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

### Deep dive: `execvp(args[0], args)`

The `vp` suffix stands for **V**ector (accepts an array) and **P**ath (auto-searches environment directories like `/bin/` or `/usr/bin/`, so you don't have to specify absolute paths).

1. **`char *args[] = {"ls", "-l", NULL};`**
   - `args[0]` (`"ls"`): by convention, the first index is the name of the executable itself.
   - `"-l"`: flags/arguments passed directly to the program.
   - `NULL`: a mandatory terminating sentinel that tells the OS where the arguments end in RAM.
2. **`execvp(args[0], args);`**
   - First input: the name of the file to execute (`"ls"`).
   - Second input: the complete array of argument strings.

### RAM transformation during `execvp`

| Stage | What's inside the process | What happens to the code |
| --- | --- | --- |
| **BEFORE `execvp`** | Your compiled C code, local variables (`pid`, `args`), active stack and heap | The child runs identically to the parent, up to the `execvp` call |
| **THE TRANSITION** | 💥 Complete memory wipe | The OS destroys everything inside the process's RAM except the base file descriptors |
| **AFTER `execvp`** | Machine code of `ls`, new variables, fresh frame at `ls`'s `main()` | `ls -l` runs. Code below `execvp` in your original file is gone and never executes |

## 3. Zombies, Orphans, and Daemons

When `waitpid()` is omitted, parent and child execute **asynchronously**. Three related but distinct concepts:

| Type | Parent | Process itself | Meaning |
| --- | --- | --- | --- |
| **Orphan** | Has terminated | Still running | Child has lost its original parent |
| **Zombie** | Still exists but hasn't reaped child | Already terminated | Exit information is waiting to be collected |
| **Daemon** | Usually managed independently | Running in background | A process designed to provide a background service |

An **orphan** and a **zombie** are process-lifecycle conditions; a **daemon** describes the *purpose* of a process.

### Zombie process

A **zombie** is a child that has **already terminated**, but whose parent has **not yet called `wait()`/`waitpid()`** to collect its exit status.

- Communication middleman: because memory is separated, the **kernel** catches the exit code (e.g. from `exit(42)`) and stores it in the system **process table**, paired with the child's PID.
- The kernel keeps only a small entry: PID, exit status, accounting info — so the parent can retrieve it via `wait()`.
- Once the parent calls `wait(NULL)`, the zombie is removed from the process table.

> **Zombie = child dies first, but parent hasn't reaped it yet.**

**Lifespan of a zombie:**

- **Parent calls `wait()` first:** the parent blocks; the child exits, status is handed over immediately, and the zombie lasts less than a microsecond.
- **Parent calls `wait()` later:** the child remains a zombie (`<defunct>`) in the process table until the parent executes `wait()`.
- **Parent never calls `wait()`:** the zombie remains until reaped by someone else (see reparenting below).

**Why are zombies a problem?** A zombie consumes no CPU and almost no memory, but it still occupies a **process-table/PID entry**. A buggy parent that continually creates children without calling `wait()` fills the table:

```
Parent
 ├── Zombie
 ├── Zombie
 ├── Zombie
 └── ...
```

This links directly to **why `fork()` fails with `-1`**:

1. `fork()` requires the kernel to assign a unique PID and allocate a free process-table row.
2. Leaked zombies hoard those slots.
3. Once the table hits its hard limit (`pid_max`) — **PID exhaustion** — subsequent `fork()` calls fail with `-1` and errors like *"Resource temporarily unavailable"* or *"No more processes"*.

### Orphan process

An **orphan** is a process whose **parent terminated while the child is still running**.

```
Parent 💀          Child becomes orphan
                   ↓
systemd (PID 1) ──── Child   (reparented)
```

The child **continues running normally** and is **reparented to PID 1** (usually `systemd` on modern Linux). If it eventually terminates, PID 1 can reap it.

> **Orphan = parent dies first, child is still alive.**

If the parent dies while the child is still a zombie, the zombie is also reparented to PID 1, which reaps it — so a forgotten zombie does not necessarily remain forever.

### Easy way to remember

```
ORPHAN:  Parent dies → Child lives → PID 1 adopts
ZOMBIE:  Child dies → Parent hasn't wait() → dead entry remains
DAEMON:  Process designed to run in the background as a service
```

### Traditional daemon creation — double fork

The classic Unix daemonization technique:

```
fork() → setsid() → fork()
```

1. **Why the first `fork()`?** The original parent exits, letting the child become independent of the original process hierarchy.
2. **Why `setsid()`?** It creates a **new session**, separating the process from the old session and its controlling terminal:

```
Before:  Terminal ── Shell ── Process (old session)
After:   Terminal ── Shell (old session)
         Process (new session)
```

3. **Why fork a second time?** After `setsid()`, the process is a **session leader**, and a session leader can potentially acquire a controlling terminal. The second `fork()` creates a child that is **not** a session leader, so the final daemon cannot accidentally acquire one.

> [!warning] `fork(); fork();` alone is not enough
> It can create a grandchild and eventually an orphan, but it does **not** detach the process from the original terminal/session. `setsid()` performs the important session detachment.

### Modern Linux: `systemd`

On modern Linux, applications generally **don't implement daemonization themselves** — `systemd` manages the program as a service. The program just runs in the foreground, while `systemd` handles starting, stopping, restarting, backgrounding, logging, and dependencies. The double-fork technique remains important for understanding *how Unix daemonization works*.

## 4. `wait()`, `waitpid()`, and `SIGCHLD`

### `waitpid()`

```c
waitpid(pid, &status, 0);
```

- Waits for the specified child.
- If the child is still running, the parent **blocks** until it changes state.
- If the child has already terminated, its exit status is collected and the zombie is removed.

### `WNOHANG`

```c
waitpid(pid, &status, WNOHANG);
```

> **Don't block if the child hasn't finished.**

```
Child finished?
   ├── YES → collect status
   └── NO  → return immediately
```

This is useful for shells, which **must remain responsive** instead of waiting for background jobs.

### `SIGCHLD`

When a child terminates, the kernel can send **`SIGCHLD`** to its parent. It is a **notification**, not the exit status itself. The parent can install a **signal handler**:

```
Child terminates → Kernel sends SIGCHLD → Parent's handler runs
→ waitpid(..., WNOHANG) → status collected → zombie removed
```

> - **Signal** = notification
> - **Signal handler** = code that responds to the notification
> - **`waitpid()`** = collects the child's termination information and reaps it

Receiving `SIGCHLD` does **not** automatically call `waitpid()` — the program must choose how to respond.

## 5. Shell Background Jobs

A command followed by `&` runs as a **background job**:

```
sleep 10 &
```

The shell roughly does `fork()` → child `exec("sleep")`. For a **foreground** command the shell then `waitpid()`s; for a **background** command it **does not wait immediately**, so it can keep accepting commands.

When the background child finishes:

```
Running → exit() → Zombie → waitpid() → removed
```

> **Zombie = terminated child whose parent has not yet collected its exit status.**

### Overall picture

```
$ sleep 10 &
      ↓ fork()
      ├──▶ Shell continues (doesn't wait)
      ↓
Child → exec() → runs in background → exit() → Zombie
      ↓
Kernel sends SIGCHLD → Parent's signal handler
      ↓
waitpid(..., WNOHANG) → exit status collected → zombie removed
```

**Key idea:** `&` makes the shell not wait immediately; `SIGCHLD` can notify the shell; `waitpid()` reaps the finished child; `WNOHANG` makes sure the shell doesn't block while checking.

## 6. Environment Variables

An **environment variable** is a named piece of information stored in a process's environment and inherited by its child processes:

```
HOME=/home/user
USER=user
PATH=/usr/local/bin:/usr/bin:/bin
```

View with `echo $HOME` / `echo $PATH`. `export` makes a shell variable part of the environment inherited by children:

```
export NAME=John

Shell ──fork()──▶ Child inherits NAME=John
```

> A child inherits the parent's environment, but changes to the child's environment do not change the parent's.

### `PATH`

`PATH` is an environment variable containing a **list of directories where executable programs can be found** (separated by `:`):

```
/home/user/.local/bin:/usr/local/bin:/usr/bin:/bin
```

When you type `python`, the shell/libc searches the directories **left to right**:

```
/home/user/.local/bin/python   ❌
/usr/local/bin/python          ❌
/usr/bin/python                ✅
```

The first matching executable wins. `which python` shows which executable is found first.

> **`PATH` = directories to search for executable commands.** It contains the *directories*, not the executables themselves.

## 7. Shell Built-ins vs External Commands

A **shell built-in** is implemented directly inside the shell rather than being a separate executable — because it needs to modify the **current shell's state**:

```
cd, export, unset, exit, alias, source / .
```

Example — why `cd` must be built in:

```
fork() → Child → cd /tmp

Shell  → /home/user   (unchanged)
Child  → /tmp         (exits shortly after)
```

If `cd` ran in a child, only the child's working directory would change. Same logic applies to:

- `export` / `unset` → change the shell's environment
- `alias` → changes the shell's aliases
- `exit` → terminates the shell itself
- `source` → executes commands in the current shell

**External commands** don't need to modify shell state, so they run as separate processes — `ls`, `cat`, `grep`, `gcc`, `python`, `sleep`:

```
Shell ──fork()──▶ Child ──exec(...)──▶ external program
```

Foreground: the shell then `waitpid()`s. Background (`&`): it doesn't wait immediately.

## 8. libc vs Kernel

**libc** (the C standard library) provides convenient user-space functions like `execvp()`, `printf()`, `malloc()`. Some of them eventually make **system calls** to the kernel, which performs the actual OS-level operations.

```
Program → libc execvp() → PATH search in user space
        → libc execve(...) → system call → Kernel → execute program
```

The kernel **doesn't search `$PATH`** — it receives an actual pathname like `/usr/bin/python` and executes that file.

> **`PATH` searching is user-space functionality** provided by shells and libc's `execvp()`. The `p` in `execvp()` means it performs this search; `execvp()` is doing the PATH searching in user space before finally calling the `execve` system call.

### Big picture

```
User types: python program.py
      ↓
Shell → searches PATH → finds /usr/bin/python
      ↓
fork() → Child → execvp() / execve() → Kernel → Python executes
```

## 9. Fork Process Counting

![[Pasted image 20260915175726.png]]

### Key rule

After `fork()`, **both the parent and child continue executing from the next line**. Each `fork()` doubles the number of processes that reach the next statement:

```text
1st fork → 2 processes
2nd fork → 4 processes
3rd fork → 8 processes
```

Therefore:

$$\text{Total processes} = 2^n$$

where $n$ is the number of `fork()` calls executed by each process.

> [!important] "How many processes are created?"
> - **Total processes:** includes the original process → $2^n$
> - **New processes created:** total − 1 → $2^n - 1$

### Example

```c
char *letters = "ABC";

for (int i = 0; i < 3; i++) {
    printf("Process %d prints: %c\n", getpid(), letters[i]);
    fork();
}

printf("Process %d finished!\n", getpid());
```

- **`i = 0`:** 1 process prints `A`, then forks → 2 processes. `A` printed **1 time**.
- **`i = 1`:** both print `B`, both fork → 4 processes. `B` printed **2 times**.
- **`i = 2`:** all 4 print `C`, all fork → 8 processes. `C` printed **4 times**.
- After the loop, all 8 print `finished!`.

**Answer:** 8 total processes (7 new), `C` printed 4 times, `finished!` printed 8 times.

### Example: two `fork()` calls with conditions

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

After the first `fork()` there are 2 processes; after the second, **both** fork again, so there are 4:

| Process | `pid1` | `pid2` |
| --- | --- | --- |
| A | > 0 | > 0 |
| B | 0 | > 0 |
| C | > 0 | 0 |
| D | 0 | 0 |

- `"Meow!"` requires `pid1 != 0 && pid2 != 0` → only A → **1 time**.
- `"Ah!"` requires `pid2 != 0` → A and B → **2 times**.

### Quick fork rules

- **Number of processes:** `n` fork() calls → up to $2^n$ total. **New processes:** $2^n - 1$.
- **Printing before `fork()`** — the `printf()` happens *before* duplication, so it is not doubled by that fork.
- **Printing after `fork()`** — both parent and child reach it, so the count doubles.

> [!tip] Always ask: "How many processes reach this line?"
> That tells you how many times that line executes.

---

## Flashcards

How does modern Chrome utilize process separation when you open a new tab?
??
It uses a multi-process architecture where opening a new tab forks an isolated Renderer Process, ensuring that if one tab crashes, the rest of the browser remains stable.
<!--SR:!2026-10-20,14,290-->

What core transformation happens in RAM when a child process successfully executes `execvp()`?
??
The OS completely wipes out the current program code and variables inside that container, replacing it entirely with the machine code of a new executable (like `ls`).
<!--SR:!2026-10-09,3,230-->

Why must a command-line utility array passed to `execvp` always end with a `NULL` pointer?
??
It acts as a mandatory terminating sentinel so the OS kernel knows precisely where the argument list ends in RAM, preventing memory corruption crashes.
<!--SR:!2026-10-07,1,190-->

What two things happen when a parent process executes the `waitpid()` function?
??
1. The parent blocks (freezes) execution until the target child process terminates.
2. The parent collects the child's final exit status from the OS Kernel.
<!--SR:!2026-10-14,8,250-->

What exact data does a child process pass to the OS Kernel upon calling `exit(42)`?
??
It hands over its **Exit Status** (the integer `42`). The Kernel already knows the child's PID and pairs it with this exit code inside the system Process Table.
<!--SR:!2026-10-07,1,190-->

What happens to a child's zombie status if the parent calls `wait()` _after_ the child has already exited?
??
The child becomes a zombie temporarily; the moment the parent finally calls `wait()`, the status is instantly collected and the zombie is wiped out of the Process Table.
<!--SR:!2026-10-20,14,290-->

If zombie processes occupy virtually zero RAM, why are they considered dangerous?
??
They hoard rows in the kernel's fixed-size **Process Table**. If a long-running app leaks thousands of zombies, it fills up the table and causes **PID Exhaustion**.
<!--SR:!2026-10-17,11,270-->

What is the consequence of PID Exhaustion on an operating system?
??
The OS hits its hard limit (`pid_max`) and cannot generate new PIDs; it will refuse to launch any new programs, open terminal commands, or handle new tabs, effectively freezing the system.
<!--SR:!2026-10-18,12,270-->

What are two primary system limitations that will cause a `fork()` call to fail and return `-1`?
??
1. The computer runs completely out of physical memory (RAM).
2. The OS hits PID Exhaustion because the Process Table is entirely full.
<!--SR:!2026-10-21,15,290-->

What does `fork()` do at the Operating System level?
??
It creates a new child process by cloning the calling parent process.
<!--SR:!2026-10-21,15,290-->

How does `fork()` return differently to the parent and child?
??
The parent receives the child's PID (> 0), while the child receives 0.
<!--SR:!2026-10-21,15,290-->

What does a `fork()` return value of `0` signify?
??
It means that the process receiving the return value is the child process.
<!--SR:!2026-10-22,16,290-->

What does `fork()` return to the parent?
??
It returns the actual PID of the newly created child process (> 0).
<!--SR:!2026-10-21,15,290-->

What happens if `fork()` fails?
??
It returns `-1`, typically because the OS cannot allocate the resources needed to create a new process.
<!--SR:!2026-10-09,3,230-->

How does memory isolation behave between a parent and child after `fork()`?
??
The parent and child have separate memory spaces, so modifying a variable in one process does not modify the other.
<!--SR:!2026-10-22,16,290-->

Why can parent and child execute different branches of the same `if-else` after `fork()`?
??
They execute the same code, but `fork()` returns different values in the parent and child, allowing the program to distinguish them.
<!--SR:!2026-10-09,3,230-->

What does `exec()` do?
??
It replaces the current process's program image with another program.
<!--SR:!2026-10-22,16,290-->

What happens to the original program after a successful `exec()`?
??
Its program code and execution state are replaced by the new program, so the original code below `exec()` does not execute.
<!--SR:!2026-10-21,15,290-->

Why does `execvp()` not return if it succeeds?
??
Because the current process has been replaced by the new program.
<!--SR:!2026-10-17,11,270-->

Why does a shell use both `fork()` and `execvp()` to run an external command?
??
The shell forks a child so that the child can be replaced by the new program without replacing the shell itself.
<!--SR:!2026-10-20,14,290-->

What happens if the shell directly calls `execvp()` to run `ls`?
??
The shell itself would be replaced by `ls`, so the original shell would no longer be running.
<!--SR:!2026-10-21,15,290-->

What does the `p` in `execvp()` mean?
??
It means `execvp()` performs a `PATH` search to find the executable.
<!--SR:!2026-10-18,12,270-->

What does the `v` in `execvp()` mean?
??
It means the arguments are provided as a vector/array.
<!--SR:!2026-10-16,10,270-->

What is `libc`?
??
libc is the C standard library that provides user-space functions such as `printf()`, `malloc()`, and `execvp()`.
<!--SR:!2026-10-14,8,250-->

Does the kernel search `PATH` when executing a program?
??
No. `PATH` searching is performed in user space by programs such as the shell or libc's `execvp()`. The kernel receives an actual pathname.
<!--SR:!2026-10-08,2,230-->

What is an environment variable?
??
A named piece of information stored in a process's environment that can be inherited by child processes.
<!--SR:!2026-10-22,16,290-->

What does `export` do?
??
It makes a shell variable part of the environment inherited by child processes.
<!--SR:!2026-10-22,16,290-->

What is `PATH`?
??
`PATH` is an environment variable containing a list of directories where executable programs can be found.
<!--SR:!2026-10-12,6,250-->

What does `echo $PATH` show?
??
It shows the directories listed in the `PATH` environment variable, separated by `:`.
<!--SR:!2026-11-19,44,290-->

How does the shell find `python` when the user types `python`?
??
It searches the directories in `PATH` from left to right until it finds an executable named `python`.
<!--SR:!2026-10-21,15,290-->

What does `which python` do?
??
It shows which `python` executable is found first through the `PATH` search.
<!--SR:!2026-10-21,15,290-->

Does `PATH` contain executable files?
??
No. `PATH` contains directories that contain executable files.
<!--SR:!2026-10-18,12,270-->

What is the difference between `/usr/bin` and `~/.local/bin`?
??
`/usr/bin` is generally a system-wide directory containing programs available to users. `~/.local/bin` is a directory inside a user's home directory for programs installed for that user.
<!--SR:!2026-10-08,2,230-->

What is a shell built-in command?
??
A command implemented directly inside the shell rather than being a separate executable.
<!--SR:!2026-10-16,10,270-->

Why does `cd` need to be a shell built-in?
??
`cd` must change the current shell's working directory. If it ran in a child process, only the child's directory would change.
<!--SR:!2026-10-22,16,290-->

What happens if `cd /tmp` were executed only in a child process?
??
The child would change to `/tmp`, but the parent shell would remain in its original directory after the child exits.
<!--SR:!2026-10-17,11,270-->

What are important examples of shell built-in commands?
??
`cd`, `export`, `unset`, `exit`, `alias`, and `source`/`.`.
<!--SR:!2026-10-08,2,250-->

Why does `exit` need to be a shell built-in?
??
It needs to terminate the current shell itself. If it ran in a child, only the child would terminate.
<!--SR:!2026-10-20,14,290-->

Why is `source` or `.` a shell built-in?
??
It executes commands inside the current shell process, allowing those commands to modify the shell's state.
<!--SR:!2026-10-17,11,270-->

What are examples of external commands?
??
`ls`, `cat`, `grep`, `gcc`, `python`, and `sleep`.
<!--SR:!2026-10-08,2,230-->

How does a shell normally execute an external command?
??
The shell uses `fork()` to create a child, and the child uses `exec()` to replace itself with the external program.
<!--SR:!2026-10-21,15,290-->

What is a foreground command?
??
A command for which the shell waits for the child process to finish before continuing.
<!--SR:!2026-10-16,10,270-->

What is a background command?
??
A command followed by `&` that allows the shell to continue running without immediately waiting for the child.
<!--SR:!2026-11-15,40,290-->

What happens when a background child finishes?
??
The child terminates and can temporarily become a zombie until the parent reaps it.
<!--SR:!2026-10-18,12,270-->

What is a zombie process?
??
A child process that has already terminated, but whose parent has not yet collected its exit status using `wait()` or `waitpid()`.
<!--SR:!2026-10-17,11,270-->

Does a zombie process still execute?
??
No. It has already terminated. Only a small amount of information about it remains in the kernel's process table.
<!--SR:!2026-10-07,1,190-->

What information does the kernel keep for a zombie?
??
Information such as the child's PID and exit status.
<!--SR:!2026-10-13,7,250-->

Why does the kernel keep a terminated child's exit status?
??
So that the parent can retrieve the child's termination information using `wait()` or `waitpid()`.
<!--SR:!2026-10-22,16,290-->

What happens when the parent calls `wait()` or `waitpid()` on a zombie?
??
The parent collects the child's exit status and the kernel removes the zombie's process-table entry.
<!--SR:!2026-11-12,37,290-->

What is an orphan process?
??
A process whose parent has terminated while the process itself is still running.
<!--SR:!2026-10-21,15,290-->

What happens to an orphan process?
??
It is reparented to PID 1, usually `systemd` on modern Linux systems.
<!--SR:!2026-10-20,14,290-->

What is the difference between an orphan and a zombie?
??
An orphan is still running but its parent has died. A zombie has already terminated but its parent has not yet reaped it.
<!--SR:!2026-10-20,14,290-->

What happens if the parent dies while its child is a zombie?
??
The zombie is reparented to PID 1, which can reap it and remove its process-table entry.
<!--SR:!2026-10-22,16,290-->

What is a daemon process?
??
A process designed to run in the background and provide a service.
<!--SR:!2026-10-18,12,270-->

What is the traditional Unix technique for creating a daemon?
??
The traditional technique commonly uses `fork()` → `setsid()` → `fork()`.
<!--SR:!2026-10-16,10,270-->

Why is the first `fork()` used during traditional daemonization?
??
It allows the original parent to exit and the child to become independent of the original process hierarchy.
<!--SR:!2026-10-09,3,210-->

What does `setsid()` do?
??
It creates a new session, separating the process from the old session and its controlling terminal.
<!--SR:!2026-10-20,14,290-->

Why is the second `fork()` used during traditional daemonization?
??
It creates a child that is not a session leader, preventing the final daemon from accidentally acquiring a controlling terminal.
<!--SR:!2026-10-08,2,250-->

Why isn't `fork(); fork();` alone enough to properly daemonize a process?
??
It can create a grandchild, but it does not properly detach the process from the original session and controlling terminal. `setsid()` performs the important session detachment.
<!--SR:!2026-10-17,11,270-->

What does `waitpid(pid, &status, 0)` do?
??
It waits for the specified child. If the child is still running, the parent blocks until the child changes state, then collects its status.
<!--SR:!2026-11-16,41,290-->

What does `WNOHANG` mean?
??
It tells `waitpid()` not to block if the child has not finished.
<!--SR:!2026-10-20,14,290-->

What happens when `waitpid(pid, &status, WNOHANG)` finds that the child is still running?
??
It returns immediately instead of making the parent wait.
<!--SR:!2026-10-20,14,290-->

Why is `WNOHANG` useful for shells?
??
It allows the shell to check whether a background child has finished without blocking the shell.
<!--SR:!2026-10-21,15,290-->

What is `SIGCHLD`?
??
A signal that the kernel can send to a parent when one of its children changes state, commonly when the child terminates.
<!--SR:!2026-10-14,8,250-->

Is `SIGCHLD` the child's exit status?
??
No. `SIGCHLD` is a notification. The parent uses `wait()` or `waitpid()` to collect the child's exit status.
<!--SR:!2026-10-20,14,290-->

What is a signal handler?
??
Code registered by a process to respond when the process receives a particular signal.
<!--SR:!2026-10-21,15,290-->

What can a parent do when it receives `SIGCHLD`?
??
It can run a signal handler that responds to the notification, commonly by calling `waitpid()` to reap terminated children.
<!--SR:!2026-10-21,15,290-->

Does receiving `SIGCHLD` automatically call `waitpid()`?
??
No. `SIGCHLD` is only a notification. The program must choose how to respond, such as by calling `waitpid()`.
<!--SR:!2026-10-18,12,270-->

What is the relationship between `SIGCHLD` and `waitpid()`?
??
`SIGCHLD` tells the parent that a child changed state, while `waitpid()` allows the parent to collect the child's termination information and reap it.
<!--SR:!2026-10-22,16,290-->

How many total processes can result from `n` independent `fork()` calls if every process reaches every `fork()`?
??
Up to `2^n` total processes, including the original process.
<!--SR:!2026-10-15,9,250-->

How many new processes are created by `n` independent `fork()` calls if every process reaches every `fork()`?
??
`2^n - 1` new processes, because the original process is included in the total.
<!--SR:!2026-10-20,14,290-->

How do you determine how many times a line executes in a `fork()` counting problem?
??
Ask: "How many processes reach this line?" Every process that reaches the line executes it.
<!--SR:!2026-10-07,1,190-->

What happens if `printf()` appears before a `fork()`?
??
That `printf()` executes before the process is duplicated, so that fork does not double the number of times that particular `printf()` executes.
<!--SR:!2026-11-20,45,290-->

What happens if `printf()` appears after a `fork()`?
??
Both the parent and child continue from the next line, so both execute the `printf()`.
<!--SR:!2026-10-16,10,270-->

If a loop has 3 `fork()` calls and every process reaches every call, how many total processes can exist?
??
`2^3 = 8` total processes.
<!--SR:!2026-10-22,16,290-->

If a loop has 3 `fork()` calls and 8 total processes exist, how many new processes were created?
??
7 new processes, because the original process is included in the 8 total.
<!--SR:!2026-10-08,2,190-->
