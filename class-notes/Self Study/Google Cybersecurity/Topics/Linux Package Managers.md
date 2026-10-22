---
course: Coursera Google Cybersecurity
topic: Linux
tags:
  - google
  - cybersecurity
---

# 📦 Linux Package Managers

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

---

## 📌 Key Definitions

- **Package** — a piece of software (or a collection of files/dependencies) used to form or run applications.
- **Package manager** — a tool used to **install, manage, and remove** packages and handle **dependency resolution**.

> [!WARNING] Security note
> Always use the **most recent version** of a package to ensure you have the latest **bug fixes and security patches**.

---

## 🗂️ Distributions & Their Package Managers

Linux distributions often descend from **parent distributions**, so their package managers and file extensions are shared.

| Parent distribution | Package manager / tool | File extension | Example format |
| :--- | :--- | :--- | :--- |
| **Debian** (e.g. Ubuntu, Kali Linux, Parrot) | `dpkg` / **APT** | `.deb` | `Package_Version-Release_Architecture.deb` |
| **Red Hat** (e.g. CentOS) | **RPM** / **YUM** | `.rpm` | `Package-Version-Release_Architecture.rpm` |

---

## 💻 CLI Package Management Tools

Low-level package managers handle the heavy lifting; **package management tools** are run via the shell to simplify searching, installing, and managing software.

- **APT (Advanced Package Tool)** — used with **Debian-derived** distributions; simplifies command-line package management, searching, and installation.
- **YUM (Yellowdog Updater Modified)** — used with **Red Hat-derived** distributions; works directly with `.rpm` files from the command line.

---

## 🔗 Related

- [[Linux, SQL & Python]]
- [[Linux File System]]
- [[Linux Commands]]
- [[Operating Systems]]
