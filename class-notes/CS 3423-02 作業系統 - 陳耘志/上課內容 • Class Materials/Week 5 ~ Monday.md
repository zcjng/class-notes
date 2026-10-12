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


# Week 5 Main Topic: Pipe
 
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