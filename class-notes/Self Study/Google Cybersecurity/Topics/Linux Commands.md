---
course: Coursera Google Cybersecurity
topic: Linux
tags:
  - google
  - cybersecurity
---

# 💻 Linux Commands

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

---

## 🧭 Navigation Commands

| Command | Description | Example |
| :--- | :--- | :--- |
| **`pwd`** | Prints the **absolute path** of the current working directory | `pwd` → `/home/analyst` |
| **`whoami`** | Returns the username of the currently logged-in user | `whoami` → `analyst` |
| **`ls`** | Lists files and directories in the current (or specified) path | `ls projects` |
| **`cd`** | Changes the current working directory | `cd /home/analyst/logs` or `cd ..` |

---

## 📖 File Reading Commands

| Command | Description | Pro tip / flag |
| :--- | :--- | :--- |
| **`cat`** | Displays the **entire** content of a file | Great for smaller text files |
| **`head`** | Displays the **beginning** of a file (default: 10 lines) | `-n [num]` to specify lines, e.g. `head -n 5 file.txt` |
| **`tail`** | Displays the **end** of a file (default: 10 lines) | Essential for the most recent entries in log files |
| **`less`** | Views content **one page at a time** | `Space` forward · `b` back · arrows · `q` quit |

> [!TIP] Analyst workflow
> `tail` on a log file is your fastest first look at the newest events. For long files, `less` avoids flooding the terminal.

---

## 🔗 Related

- [[Linux File System]]
- [[Linux Filtering]]
- [[Linux Package Managers]]
- [[Logs & SIEM]]
