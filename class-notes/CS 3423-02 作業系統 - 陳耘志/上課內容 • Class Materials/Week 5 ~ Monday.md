---
title: 第五週 • 週一
course: CS 3423-02 作業系統
week: 5
day: Monday
tags:
  - os
---

# 第五週 • 週一 — Inter-Process Communication & Pipes

## PTT 
boolean robust system design ptt uses multi-process design, where each process handles different responsibilities.

So whenever one process encounter a problem, the other processes are not affected.

But how do the processes coordinate with each other then?

## IPC (Inter-process Communication)
### Pipes
- Data streams between related processes
- Examples: Parent and child after a fork
### FIFO (Named Pipe)
- Pipe but for unrelated processes
- Useful for decoupled communication between unrelated processes
- Exists as a special file in fs, so any process with permissions can use fifo.
### Shared memory
- Multiple processes access the same memory region. (no copy)
- Each process must sync with each other carefully, otherwise they might overwrite each other's data
### Signals
- Sends notification about some events
- Like CTRL + C, sends a SIGINT signal
### Unix Domain Sockets
- Provides local communication
- Support bidirectional communication
- Can send file descriptor around processes

PTT uses all of these IPC.


### Unix Philosophy
- Make each program do one thing well and let them work together
- We can chain commands together
- Example: ls (list files), wc (count lines), they are separate programs running in separate memory spaces
	- Pipe is utilized to chain them together, ex: ls | wc
	- ls can write its output to a temp file, and make wc read from it, then we can delete the file.
		- Not efficient: ls has to finish writing output before wc can even begin to read, data makes an unnecessary round trip to the disk. (**Sequential execution**)
	- Wire the output of first program to the input of the second
	- This way we dont need to touch the disk (no need for the temp file)
	- The second program can start running immediately picking up the output from the first program on the fly, while its still running. (**Pipelined parallel execution**)


## Week 5 Main Topic: Pipe
 
- Each process has file descriptors.
When, we open "cats.txt", the kernel finds the inode file.
1. Inode table entry contains the metadata of the file
	- SIze
	- Permission
	- The location in the disk
2. The kernel will create a new entry in the system-wide open file table, which stores:
	- The offset (0)
	- Access mode
	- A pointer to the inode table
3. The kernel adds an entry in the process's fd table. So, fd 3 points to the open file table entry.


```c
int main(int argc, char *argv[]) {
	int fd = open("cats.txt", O_RDONLY);
	char buf[1024];
	while (true) { 
		ssize_t bytesRead=read(from_fd, buf, sizeof(buf));
		if (bytesRead == 0) break;
	}
	close(fd);
	return 0;
}
```


- Each time we call 'read' sys-call, the kernel will read up to 1024 bytes from the file, starting from the current offset (0), 
- Then pushes the offset by 1024 each call.
![[Pasted image 20261012042336.png|319]]
Lets say the program has 2778 bytes worth of characters.
- It would then read 3 times, 1024 + 1024 + the rest 728 bytes because we have reached the EOF (end of file)
- If we request to read beyond, the kernel will just give us fewer bytes.
- If we call read after the file is completely empty, the program will return an EOF marker, which means it returns exactly 0 bytes (or an empty string/buffer)
	- In python, it returns an empty string or by string: "", or b""
	- in C / C++ / Java,  returns the integer 0 (0 bytes were copied into the buffer)
- Conclusion, the kernel will keep track of the file offset, and it  exists in the open file table, is similar to a bookmark in the file.

![[Pasted image 20261012044158.png]]

- We can also a system call lseek, to change the file offset manually

```c
lseek(fd, -900, SEEK_CUR)
```
- Which jumps the current file offset back by 900 bytes.

### - cp unix command
```c
1 void copy(int cat_fd, int dog_fd) {
2     char buf[1024];
3     while (true) {
4         ssize_t bytesRead = read(cat_fd, buf, sizeof(buf));
5         if (bytesRead == 0) break;
6 
7         size_t bytesWritten = 0;
8         while (bytesWritten < bytesRead) {
9             ssize_t count = write(dog_fd, buf + bytesWritten, bytesRead - bytesWritten);
10            bytesWritten += count;
11        }
12    }
13 }
14 
15 int main(int argc, char *argv[]) {
16     int cat_fd = open("cat.txt", O_RDONLY);
17     int dog_fd = open("dog.txt", O_WRONLY | O_CREAT | O_EXCL);
18 
19     copy(cat_fd, dog_fd);
20     close(cat_fd); close(dog_fd);
21     return 0;
22 }

```

-  cp: copies one file into another file.
	This program copies the content of 'cat' into 'dog'. After reading from the cat, we need to write it to 'dog'.
	- We need to keep writing in a while loop because a single write system call may only write part of the buffer.
	- We have to write out all the data we have read.
	- Each successful write will also push the file offset.
	After the outer loop 'read' system call returns 0, then that it has reached the end of the 'cat' file.

## Fork

![[Pasted image 20261012045058.png|446]]

- The child inherits a copy of the parent's file descriptor table, both of them has the fd 3 pointing to the same entry in the open file table.
- They share the same file offset. (if the parent reads and pushes the child will be affected as well)

## Dup
- The ```dup()``` system call creates a copy of a file descriptor,
- Places it at the bottom or lowest unused number in the fd table

```c
int main() {
    int fd = open("cats.txt", O_RDWR | O_CREAT);
    int fd2 = dup(fd);

    write(fd, "cat 1\n", 6);
    write(fd2, "cat 2\n", 6);

    return 0;
}
```

This program opens cats file and duplicates the file descriptor 3, which was created from the open() system call.
- Then the duplicated file descriptor is added and placed at entry 4.
- Meaning that they point to he same open file table and same offset

![[Pasted image 20261012050151.png|353]]

When we call write(fd, "cat 1\n", 6), 6 bytes will be written and the file offset will be moved forward by 6, 
-  Then if we call write(fd2, "cat 2\n", 6), the write will continue from the same offset 6 and write to the file.


### Example 2:
```c
1 int main() {
2     int fd = open("cats.txt", O_RDWR | O_CREAT);
3     close(1); // close stdout
4 
5     int fd2 = dup(fd); // fd2 = 1
6     write(fd2, "cat 1\n", 6);
	7 printf("cat 2\n");  // will instead write to cats.txt
	
8     return 0;
9 }
```

This program in the same manner opens cats, occupying file descriptor 3 then closes fd 1 which is the STDOUT pointing to the terminal.
- When we call dup(fd), fd 1 is not occupied anyone and its the lowest unused fd number, so dup gives back 1.
- It takes the fd1 (STDOUT), then makes it point to the same thing that the system call fd3 was pointing because it copied that.
- So, fd1 and fd3 both point to the same file, cats.txt

![[Pasted image 20261012051149.png|312]]


What does this mean?
- The current program main, has its standard output redirected to cats.txt
- So, whenever we output anything ex: printf
- Instead of writing it to the terminal or screen, it will be redirected and be written into the cats.txt
- So, we can think of dup() as not copying the fd, but it copies what the fd is pointing towards. So they point to the same entry on the open file table.


## Dup2
- dup2() focuses mainly on the redirecting technology that dup() has.
- Mainly used to redirect I/O to another file.

```c
dup2(oldfd, newfd);
```
- The I/O of newfd will be redirected to oldfd.
- If newfd was already open then the kernel will close it first, then reassigns it.

```c
int main() {
    int fd = open("cats.txt", O_RDWR | O_CREAT);
    dup2(fd, 1); // redirect I/O to stdout to fd

    write(fd, "cat 1\n", 6);
    printf("cat 2\n"); // write to cats.txt

    return 0;
}
```

- dup2(fd, 1);
	Makes the oldfd, which is the file descriptor 3, which points to cat, copies the pointer and places it onto the newfd which was pointing to the terminal (STDOUT) and replaces it.

![[Pasted image 20261012052935.png|312]]
	So now, the newfd (1) instead points toward the file cats.txt
	Same as before, the write system call and printf command wil all be written to the cats.txt file, and there would be STDOUT writted to the screen.

```c
$ ls > out.txt
```
- This is exactly the mechanism the shell uses to implement output redirection, so when you redirect the output of 'ls' into a file,
- The shell runs dup2(fd, 1) before calling 'exec'.
- From 'ls''s perspective, its still writing to STDOUT but in reality STDOUT has been reassigned to a file instead (out.txt).
- So, dup2() is the system-call that allows the shell to remap STDIN, STDOUT, and STDERR to any file.

## Pipe
			Everything written to fds[1] can be read from fds[0]
- Pipe gives us a one-way communication channel inside the kernel.
- Everything we write into the write-end of the pipe and be read from the read-end

```c
pipe(int fds[]);
```
The argument is an array of two file descriptors, the first is for reading and the latter is for writing.
- These data are written to a temporary buffer inside the kernel, after writing we can close the write end.

```c
1 int main() {
2     char buf[5] = {0};
3     int p[2];
4 
5     if (pipe(p) < 0)
6         exit(1);
7 
8     write(p[1], "meow", 4);
9     close(p[1]);
10 
11    read(p[0], buf, 4);
12    printf("%s\n", buf);
13    close(p[0]);
14 }
```

- Firstly, the integer array p gets initialized as a pipe from the conditional if-statement. 
It returns 0 on pass, and -1 if no pipe is created and the array will be left untouched, and sets the global variable errno to specify the exact error.

-  In this case, `p[0]` is added to file descriptor 3,  and `p[1]`will be added to file descriptor 4, which are the two lowest unused fd numbers.
- We first call write, which will write 4 bytes into fd4 (`p[1]`), writes 4 bytes into the pipe.
- After writing we can then close the write-end of the pipe,  by calling:
```c
close(p[1]);
```
Closing can be very important because the read side is very dependent on it being closed and may wait for more data.
- Next we call read from `p[0]` or fd3, the kernel will deliver the data from the pipe into kernel buffer, which then we can print out.

![[Pasted image 20261012062045.png|652]]

Whenever we fork after we have established the file descriptor pointing to pipes, since the fork() will copy all the pointers so the child will inherit it all.
- That means that the parent and child will share the same pipe buffer.


```c
1 int main() {
2 
3 int fds[2];
4 pipe(fds);
5 pid_t pid = fork();
6 if (pid == 0) { // In child
7   close(fds[1]); // no write
8   char buf[5] = {0};
9   read(fds[0], buf, 4); // just read pipe
10  printf("From parent: %s\n", buf);
11  return 0;
12 }
13 
14 // In parent
15 close(fds[0]); // no read
16 write(fds[1], "meow", 4); // just write
17 close(fds[1]); // say to child: I'm done!
18 waitpid(pid, NULL, 0);
19 return 0;
21 }
```

![[Pasted image 20261012064646.png|280]]


In the child, we first close the write end of the pipe because the child is not going to send anything.
- Then, we prepare a buffer and we read from the read-end from the pipe and print out what we have read from the pipe

![[Pasted image 20261012064705.png|278]]
In the parent side, we have to first close the read end of the pipe because it doesn't need to read.
- Then the parent writes the "meow" string to the write end and closes the write-end to indicate that its done.
If the child processes run and calls the read before anything is written into the pipe, it will block at the 'read' system-call and be put to sleep and only be awakened once the parent has written something into the pipe.
- The read function will return the amount of byte it received.
- The code will advance to the next line, and will not go back to sleep to wait for other remaining 3 bytes.


## ls | wc 
![[Pasted image 20261012065052.png|437]]

The shell firstly will firstly establish the pipe, then forks two processes ls and wc.

```c
1 int main() {
2     int pfd[2];
3     pipe(pfd);
4 
5     pid_t pid1 = fork();
6     if (pid1 == 0) {
7         close(pfd[0]);
8         dup2(pfd[1], 1);
9         close(pfd[1]);
10 
11        execlp("ls", "ls", "-l", (char *) NULL);
12    }
13 
14    pid_t pid2 = fork();
15    if (pid2 == 0) {
16        close(pfd[1]);
17        dup2(pfd[0], 0);
18        close(pfd[0]);
19 
20        execlp("wc", "wc", "-l", (char *) NULL);
21    }
22 
23    close(pfd[0]);
24    close(pfd[1]);
25    waitpid(pid1, NULL, 0);
26    waitpid(pid2, NULL, 0);
27    exit(EXIT_SUCCESS);
28 }
```

For the ls child, it closes the read side because it doesn't need to read.
- Then it redirects STDOUT to point to the same thing file descriptor 4 which is `pid[1]` is pointing, then closes the file descriptor 4 because it is no longer needed.
- So right now, only STDOUT is pointing to the pipe.
- Closing the write end of the pipe or file descriptor 4 before execlp is important, ls only knows about STDOUT, it will never use the original pipe, 
- When `execlp` replaces the child process with the `ls` program, `ls` will inherit every open file descriptor. If you don't close `pfd[1]`, `ls` will run with an extra, unneeded open descriptor sitting in its table. 
- While `ls` finishes quickly, in larger programs, failing to clean these up leads to "file descriptor leaks," which can eventually hit system limits and crash the program.
- This incident or event is generally known as "**leaked descriptors**"
- After `ls` finishes, it would then wipe all the file descriptors clean.

For the wc child, it closes the write side.
- It then calls dup2() to wire STDIN to what file descriptor 3 was pointing which is the read end of the pipe.
- Then it closes the oldfd which was file descriptor 3 because it has been replaced and is no longer needed.
- So now, only the STDIN is waiting and pointing to the write side of the pipe.
- wc thinks that its reading from STDIN, but its actually coming from the output of the ls from the other side.

The parent closes both ends of the pipe and waits for the children to finish.
- The parent closes the file descriptors pointing towards the pipe immediately because it doesn't want to block the pipe from closing, which could hang wc.
- **Why this is critical:** If the parent forgot to close the write end (`pfd[1]`), **`wc` would hang forever**. A reading process like `wc` only stops reading when it receives an End-of-File (EOF) signal. 
- The operating system only sends an EOF signal through a pipe when **every single process** that has the write end open closes it. If the parent keeps its write end open, the system thinks more data might be coming, and `wc` will never finish.

How Reference Counting Controls a Pipe
- **When the Last WRITE End Closes:** If there are **zero** file descriptors pointing to the write end of the pipe anywhere in the entire system, the kernel sends an **EOF (End-of-File) condition** to the reader. The reader's `read()` call instantly unblocks and returns `0`.
- **When the Last READ End Closes:** If there are **zero** file descriptors pointing to the read end of the pipe, but a process still tries to write to it, the kernel sends a **`SIGPIPE` signal** to the writer. By default, this immediately terminates the writing process because there is no one left to listen. 
- **When BOTH Ends Reach Zero:** Once every single copy of both the read and write descriptors across the parent and all children hit zero, the kernel **deallocates the pipe entirely** and reclaims the memory buffer. 


---
### UNIX Pipeline Mechanics: EOF vs SIGPIPE

A UNIX pipeline (`cmd1 | cmd2 | cmd3`) relies on an internal kernel **reference tracking counter** to manage data flow. Operating systems manage file descriptors (FDs) as unique directional pointers rather than the entire pipeline layer itself.

 1. Pipeline Component Initialization
* **Concurrent Execution:** All processes inside a multi-stage chain initialize simultaneously.
* **Reader Synchronization:** Downstream consumers (like `wc`) block on a `read()` system call from birth, waiting for up-stream producers to generate data or close FDs.

1. Terminating a Mid-Pipeline Process
- When a middle stage process in an active chain is forced to stop or killed:
* **Forward Cascade (End-of-File):** The kernel drops the dead process's *write* file descriptor counts down to zero. The immediate down-stream process detects an empty channel with zero remaining writers, triggers an **EOF (0 bytes read)** state, flushes its current buffer, and exits cleanly. This ripples forward to the end of the pipeline.
* **Backward Cascade (Broken Pipe Signal):** Processes upstream from the crash remain running initially, completely unaware of the downstream exit. 

3. Writing to a Broken Pipeline
* **Trigger:** An active upstream producer calls a `write()` system call aiming at a channel where the receiving end is completely dead.
* **Action:** The operating system kernel immediately catches the dead connection and flags an `EPIPE` error condition.
* **Result:** To prevent system memory resource leaks and redundant computation cycles, the kernel delivers a **`SIGPIPE` signal** to the upstream process. By default, `SIGPIPE` terminates the process instantly, cascading backwards until the entire front end of the pipeline is dead.

---

## Flashcards

What does IPC stand for, and why does a system need it?
??
Inter-Process Communication. Processes have separate memory spaces and cannot directly share data, so they use OS-provided channels (pipes, shared memory, signals, sockets) to exchange data and coordinate.

Why does PTT use a multi-process design?
??
PTT is a robust system that splits responsibilities across multiple processes, so if one process has a problem the others are not affected. The processes then coordinate using IPC.

What are the main IPC mechanisms covered in the notes?
??
Pipes, FIFOs (named pipes), shared memory, signals, and Unix domain sockets.

What is a pipe?
??
A one-way byte-stream channel inside the kernel, typically used between related processes such as a parent and child after `fork()`.

How does a FIFO (named pipe) differ from a pipe?
??
A pipe only exists between related processes, while a FIFO exists as a special file in the filesystem, so unrelated processes with permission can use it for decoupled communication.

What is shared memory as an IPC mechanism, and what is its main risk?
??
Multiple processes map and access the same memory region directly (no copy), which is fast. The risk is that the processes must synchronize carefully, or they will overwrite each other's data.

What do signals provide, and give an example?
??
Signals send a notification about an event to a process. For example, pressing Ctrl+C sends `SIGINT`.

What is special about Unix domain sockets?
??
They provide local (same-machine) bidirectional communication and can pass file descriptors between processes.

What is the Unix philosophy behind pipelines?
??
Make each program do one thing well and let programs work together by chaining them, e.g. `ls | wc`.

Why is piping better than using a temporary file to connect two programs?
??
With a temp file, the first program must finish writing before the second can read (sequential execution, plus an unnecessary disk round-trip). A pipe wires the first program's output directly to the second program's input, so it can start immediately as data arrives (pipelined parallel execution) and never touches the disk.

What three data structures does the kernel maintain for an open file?
??
1. The per-process file descriptor table.
2. A system-wide open-file table entry (offset, access mode, pointer to the inode).
3. The inode table entry (metadata: size, permissions, disk location).

What is a file descriptor?
??
A small integer index into a process's file descriptor table that refers to an entry in the system-wide open-file table (and therefore to an open file).

Where is the file offset stored, and what does it do?
??
It is stored in the open-file table entry, not in the descriptor itself. It works like a bookmark: each `read()` or `write()` advances it by the number of bytes transferred.

What does `read()` return when it reaches end-of-file?
??
It returns 0 bytes (the EOF marker): an empty string in Python, the integer 0 in C/C++/Java.

What is `lseek()` used for?
??
It changes the file offset manually, e.g. `lseek(fd, -900, SEEK_CUR)` jumps the offset back by 900 bytes.

Why does the `cp` program keep writing inside a loop instead of calling `write()` once?
??
Because a single `write()` may write only part of the buffer, the program must loop until all `bytesRead` bytes have been written.

After `fork()`, what does the child inherit about open files?
??
The child inherits a copy of the parent's file descriptor table; the descriptors point to the same open-file table entries, so parent and child share the same file offset.

If the parent reads from a shared file, what happens to the child's position?
??
The child is affected too, because parent and child share the same open-file table entry and therefore the same file offset.

What does `dup(fd)` do?
??
It creates a copy of a file descriptor at the lowest unused descriptor number; the new descriptor points to the same open-file table entry (same file and same offset).

Why do two `dup`'ed descriptors writing to the same file continue from the same offset?
??
Because both descriptors point to the same open-file table entry, they share one offset; the second write continues where the first left off.

What does `dup2(oldfd, newfd)` do?
??
It makes `newfd` refer to the same open file as `oldfd`; if `newfd` was already open, the kernel closes it first. It is mainly used to redirect I/O.

How does the shell implement `ls > out.txt`?
??
Before `exec`, the shell calls `dup2(fd, 1)` so that STDOUT (fd 1) now points to `out.txt`. `ls` still thinks it is writing to STDOUT, but the output goes to the file.

Why is it safe to close the original descriptor after `dup2()`?
??
`dup2()` created a second reference to the same open-file entry, so closing the original descriptor only removes one reference; the file stays open through the new descriptor.

What does `pipe(fds)` do and what does it return?
??
It creates a pipe and fills `fds[0]` (read end) and `fds[1]` (write end). It returns 0 on success, or -1 on failure (leaving the array untouched and setting `errno`).

Which end of a pipe is which, and in which direction does data flow?
??
`fds[0]` is the read end and `fds[1]` is the write end. Data flows one way only: you write to `fds[1]` and read from `fds[0]`.

What happens if a process reads from a pipe before anything is written?
??
The `read()` call blocks and the process sleeps until data is written (or the write end is closed), consuming no CPU while it waits.

Describe, step by step, how a shell runs `a | b`.
??
1. The shell calls `pipe()` and `fork()`.
2. For `a`: close the read end, `dup2` the write end onto STDOUT (fd 1), close the original write end, then `exec` `a`.
3. For `b`: close the write end, `dup2` the read end onto STDIN (fd 0), close the original read end, then `exec` `b`.
4. The parent closes both ends and waits for both children.

Why must each child close the pipe end it does not use?
??
Leaving the unused end open keeps the pipe open, so the reader would never see EOF, and it would leak a file descriptor into the `exec`'d program.

Why must the parent close both pipe ends after forking the children?
??
If the parent keeps the write end open, the kernel never observes all write ends closed, so it never delivers EOF and a reader such as `wc` hangs forever.

Why does a reader like `wc` stop reading?
??
It stops when `read()` returns 0 (EOF), which the kernel sends only after every process holding the write end has closed it.

What are the three possible outcomes of `read()` on a pipe?
??
1. It returns more than 0 bytes when data is available.
2. It returns 0 (EOF) when all write ends are closed.
3. It blocks (sleeps) when no data is available but a write end is still open.

When does a pipe deliver EOF to the reader?
??
When the last write end is closed (zero write descriptors remain anywhere), the reader's `read()` returns 0.

What happens when a process writes to a pipe whose read end has been closed?
??
The kernel sends `SIGPIPE` to the writer; by default this terminates the process (the `write()` also fails with `EPIPE`).

When is a pipe fully deallocated?
??
When every read and write descriptor for the pipe, across all processes, has reached zero; the kernel then frees the pipe's buffer.

In a pipeline, what happens downstream when a middle-stage process is killed?
??
The dead stage's write-end count drops to zero, so downstream readers immediately see EOF (0 bytes), flush, and exit cleanly — the effect cascades forward to the end of the pipeline.

In a program such as `04-capture-noclose`, what keeps the reader waiting forever, and why is no error reported?
??
The write end of the pipe is still open somewhere (a process forgot to close it), so the reader never receives EOF and blocks in `read()` indefinitely. No error is reported because nothing has failed — the reader is simply waiting for data or EOF that never comes.

Does a pipe have a limited capacity, and what happens when it is full?
??
Yes. A pipe has a fixed buffer (Linux default 64 KiB). When it is full, `write()` blocks until the reader frees space, and the blocked process uses no CPU.

Why can calling `wait()` before reading a child's output deadlock?
??
If the child's output is larger than the pipe buffer, the child blocks on `write()` while the parent blocks in `wait()` waiting for the child to exit — a deadlock. Small output fits in the buffer, so the child exits and `wait()` returns normally. Fix: read (drain) the pipe before waiting, or use non-blocking I/O or threads.

Why must a program that uses a pipe as a protocol channel write only protocol messages to stdout?
??
When stdout is wired to a pipe, everything written there is consumed by the other program as protocol, so extra output (like debug messages) would corrupt the stream. Debug output should go to stderr instead.

Why does a debugging message on stderr still reach the terminal when stdout is a pipe?
??
Only stdout (fd 1) was redirected to the pipe; stderr (fd 2) still points to the terminal, so debug output is still displayed.

Name `SIGINT`, `SIGTERM`, and `SIGKILL`, and say who sends each.
??
- `SIGINT`: sent when the user presses Ctrl+C at the terminal.
- `SIGTERM`: sent by `kill` or a process manager — a polite, catchable request to terminate.
- `SIGKILL`: sent by `kill -9`/the kernel — cannot be caught or ignored, and forcibly terminates the process.

Why do process managers send `SIGTERM` before `SIGKILL`?
??
`SIGTERM` lets the program clean up and exit gracefully; if it does not exit in time, `SIGKILL` forces termination.

Why can't `SIGKILL` be caught?
??
It is handled directly by the kernel and cannot be caught, blocked, or ignored, so a process cannot refuse or delay being killed.
