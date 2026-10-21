# Course 4 ~ Module 1
# Inside the Operating System

**Topic:** The operating system at work
**Source:** Google Cybersecurity Professional Certificate

---

## Overview
This video explains how operating systems (OS) work, focusing on the processes that occur from the moment a computer is powered on to when a user interacts with an application.

## Booting the Computer
* When you press the power button, the computer's hardware is activated, initiating the booting process.
* A special microchip, either Basic Input/Output System) BIOS or (Unified Extensible Firmware Interface) UEFI, contains instructions to load the bootloader, which then starts the operating system.

## User Interaction with the OS
* Users interact with applications to perform tasks, and these applications send requests to the operating system.
* The OS interprets these requests and directs them to the appropriate hardware components, which then send information back to the OS and subsequently to the application.

## Security Implications
* Understanding these processes is crucial for security analysts, as vulnerabilities can exist in the booting process, such as in the BIOS, which is often not scanned by antivirus software.
* Tracing the flow of information between applications, the OS, and hardware is essential for investigating security events and identifying where issues may have occurred.




OS as an Energy Manager

The OS ensures efficient resource allocation, similar to how a person manages energy for different tasks.

Tasks like antivirus scans require more "energy" (resources) than simpler applications like a calculator.

The OS as an Orchestra Conductor

The OS acts as a conductor, directing various computer components (like instruments) to work in harmony.

It manages memory, storage, and CPU usage, ensuring programs receive the resources they need.

Resource Monitoring and Troubleshooting

The OS allocates and de-allocates resources for competing programs and processes.

Tools like the task manager allow users to monitor resource usage, which is crucial for troubleshooting and identifying issues like malware


# Virtualization Technology

## Overview
Operating systems can run inside standalone, software-controlled isolated containers. This architectural separation is achieved through virtualization layers rather than deploying isolated physical infrastructure.


### Key Acronyms & Components
- ==**VM (Virtual Machine)**==: A fully software-defined, virtualized replica of a physical desktop, server, or computer.
- ==**vCPU (Virtual Central Processing Unit)**==: The software-allocated execution engine that simulates the primary processing core of a physical machine.
- ==**Virtual Storage**==: Abstracted, software-defined storage volumes configured to mirror local hard drives or solid-state arrays.
- ==**Virtual Hardware**==: The holistic collection of simulated components (including networks, RAM, and chips) constructed entirely through software definitions.
-

# Understanding Virtualization

==**Virtualization**== is a technology that allows you to create multiple simulated environments from a single physical hardware system. Instead of dedicating an entire physical computer to just one task, virtualization breaks the physical system down into independent, software-defined pieces so it can do the work of several computers at once.

> [!TIP] The Apartment Analogy
> Think of a physical computer as an **apartment building**. The structure, plumbing, and electricity represent the physical hardware. Virtualization creates individual, isolated **apartments** (Virtual Machines) inside that building. Each apartment shares the same underlying foundation, but the residents live completely independent lives and cannot see into each other's spaces.

---

## Core Structural Components

- ==**OS (Operating System)**==: The foundational software that manages computer hardware and software resources (e.g., Windows, macOS, Linux). In virtualization, we break this down into two roles:
    - **Host OS**: The primary operating system running directly on the physical hardware.
    - **Guest OS**: The isolated operating system running inside a specific virtual machine. For example, you can run a Linux Guest OS inside a Windows Host OS.
- ==**Hypervisor**==: The fundamental engine of virtualization. It is a specialized software layer that sits between the physical hardware and your virtual machines.

## Hardware Allocation: Physical vs. Virtual

The ==**Hypervisor**== takes physical resources and carves them up into isolated, virtualized equivalents for each Guest OS:

| Core Term | Physical Definition | Virtual Definition |
| :--- | :--- | :--- |
| ==**CPU**== (Central Processing Unit) | The physical microchip on the motherboard that executes computing calculations and instructions. | **vCPU (Virtual CPU)**: A slice of processing time scheduled by the hypervisor, allowing multiple systems to share the physical core. |
| ==**RAM**== (Random Access Memory) | The physical memory sticks that provide a fast, temporary working space for active applications. | **Virtual Memory**: A boundary-enforced partition of physical RAM. The hypervisor creates digital walls so one VM cannot access another's memory. |


# Summary: Linux Package Managers

## Overview
* **Package:** A piece of software (or a collection of files/[[Dependencies]]) used to form or run applications. 
* **Package Manager:** A tool used to **install, manage, and remove** packages and handle dependency resolution.
* **Security Note:** Always use the most recent version of a package to ensure you have the latest **bug fixes and security patches**.

---

## Types of Package Managers & Distributions
Linux distributions often descend from parent distributions, meaning their compatible package managers and file extensions are shared.

| Parent Distribution | Package Manager / Tool | File Extension | Example Format |
| :--- | :--- | :--- | :--- |
| **Debian** *(e.g., Ubuntu, Kali Linux, Parrot)* | `dpkg` / `APT` | `.deb` | `Package_Version-Release_Architecture.deb` |
| **Red Hat** *(e.g., CentOS)* | `RPM` / `YUM` | `.rpm` | `Package-Version-Release_Architecture.rpm` |

---

## Package Management Tools (CLI)
While low-level package managers handle the heavy lifting, **package management tools** are run via the command-line interface (shell) to simplify tasks like searching, installing, and managing software.

* **[[APT]] (Advanced Package Tool):** 
  * Used with **Debian-derived** distributions.
  * Simplifies command-line package management, searching, and installation.
* **[[YUM]] (Yellowdog Updater Modified):** 
  * Used with **Red Hat-derived** distributions.
  * Works directly with `.rpm` files to manage software from the command line.



# Linux File System Navigation & Commands

## 📂 1. The Filesystem Hierarchy Standard (FHS) & Paths

* **Root Directory (`/`)**: The highest-level directory in Linux from which all other directories branch.
* **Standard FHS Directories**:
  * `/home` — User home directories.
  * `/bin` — Binary executables and programs.
  * `/etc` — System configuration files.
  * `/tmp` — Temporary files (accessible to all users, often targeted in security scenarios).
  * `/mnt` — Mount point for external media (USB drives, hard drives).
* **File Paths**:
  * **Absolute Path**: The full path starting from the root directory (e.g., `/home/analyst/projects`).
  * **Relative Path**: The path relative to your current working directory (e.g., `../projects`).
  * **Shortcuts**: 
    * `~` represents the user's home directory (e.g., `~/logs`).
    * `.` represents the current directory.
    * `..` represents the parent directory.

---

## 🧭 2. Key Navigation Commands

| Command | Description | Example |
| :--- | :--- | :--- |
| **`pwd`** | Prints the absolute path of your current working directory. | `pwd` -> `/home/analyst` |
| **`whoami`** | Returns the username of the currently logged-in user. | `whoami` -> `analyst` |
| **`ls`** | Lists files and directories in the current (or specified) path. | `ls projects` |
| **`cd`** | Changes your current working directory. | `cd /home/analyst/logs` or `cd ..` |

---

## 📖 3. Common File Reading Commands

| Command | Description | Useful Pro Tip / Flag |
| :--- | :--- | :--- |
| **`cat`** | Displays the entire content of a file. | Great for smaller text files. |
| **`head`** | Displays the **beginning** of a file (default: 10 lines). | Use `-n [num]` to specify lines (e.g., `head -n 5 file.txt`). |
| **`tail`** | Displays the **end** of a file (default: 10 lines). | Essential for checking the most recent entries in log files. |
| **`less`** | Views file content **one page at a time**. | Use `Space` (forward), `b` (back), arrow keys, and `q` (quit). |




# Filtering for Information in Linux

## Overview
Filtering is a crucial skill for security analysts, allowing them to select data that matches specific criteria (such as file extensions, text strings, or modification times) to quickly locate relevant information.

---

## 1. `grep` Command
The `grep` command searches a specified file and returns all lines containing a specified string or text pattern.
- **Common arguments:** Takes two arguments—the specific string to search for, and the specific file to search through.
- **Examples:**
  - `grep OS updates.txt` — Scans `updates.txt` and returns all lines containing `OS`.
  - `grep error time_logs.txt` — Scans `time_logs.txt` and returns all lines containing `error`.

---

## 2. Piping (`|`)
Piping sends the standard output of one command as standard input to another command for further processing.
- **Symbol:** The pipe character (`|`) is often located on the same key as the backslash (`\`).
- **Usage with `grep`:** Helps find directories and files containing specific words in their names.
- **Example:**
  - `ls /home/analyst/reports | grep users` — Lists the contents of the `reports` directory and filters the output to only show items containing `users`.
- **Note:** Piping is a general form of redirection in Linux used whenever you want one command's output to become another's input.

---

## 3. `find` Command
The `find` command searches for directories and files that meet specified criteria (such as file name, size, or last modified time).
- **First argument:** Specifies where to start searching (e.g., `find /home/analyst/projects`).
- **Options:** Modify command behavior and commonly begin with a hyphen (`-`).

### `-name` and `-iname`
Used to find file or directory names containing a specific string (entered in quotes with wildcard asterisks `*` for zero or more unknown characters).
- `-name`: Case-sensitive search.
- `-iname`: Case-insensitive search.
- **Examples:**
  - `find /home/analyst/projects -name "*log*"` — Finds files containing `log` (case-sensitive).
  - `find /home/analyst/projects -iname "*log*"` — Finds files containing `log` regardless of casing (`log`, `Log`, `LOG`, etc.).

### `-mtime` and `-mmin`
Used to find files or directories last modified within a certain time frame.
- **`-mtime` (days):**
  - `-mtime -3`: Modified within the past 3 days (less than three days ago).
  - `-mtime +1`: Modified more than 1 day ago.
  - `-mtime -1`: Modified less than 1 day ago.
- **`-mmin` (minutes):** Can be used instead of `-mtime` to search based on minutes rather than days.

---

## Key Takeaways
- **`grep`** filters text *inside* files line by line.
- **Piping (`|`)** chains commands together, redirecting output from one command as input to another.
- **`find`** locates files and directories based on attributes like name, path, and time.
- **Wildcards (`*`)** represent unknown characters and are essential when searching with patterns.
- **Case Sensitivity:** `-name` is case-sensitive, while `-iname` is case-insensitive.
