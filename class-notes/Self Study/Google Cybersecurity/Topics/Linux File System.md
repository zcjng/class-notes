---
course: Coursera Google Cybersecurity
topic: Linux
tags:
  - google
  - cybersecurity
---

# 📂 Linux File System

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

---

## 🗂️ The Filesystem Hierarchy Standard (FHS)

- **Root directory (`/`)** — the highest-level directory in Linux; every other directory branches from it.
- **Standard FHS directories:**

| Directory | Purpose |
| :--- | :--- |
| `/home` | User home directories |
| `/bin` | Binary executables and programs |
| `/etc` | System configuration files |
| `/tmp` | Temporary files — **accessible to all users, often targeted in security scenarios** |
| `/mnt` | Mount point for external media (USB drives, hard drives) |

---

## 🧭 File Paths

- **Absolute path** — the full path starting from the root directory, e.g. `/home/analyst/projects`.
- **Relative path** — the path relative to your current working directory, e.g. `../projects`.

### Shortcuts

- `~` — the user's **home directory** (e.g. `~/logs`)
- `.` — the **current** directory
- `..` — the **parent** directory

---

## 🔗 Related

- [[Linux Commands]]
- [[Linux Filtering]]
- [[Linux Package Managers]]
- [[Linux, SQL & Python]]
