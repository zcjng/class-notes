---
course: Coursera Google Cybersecurity
topic: Linux
tags:
  - google
  - cybersecurity
---

# 🔎 Linux Filtering

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-21|Related day: 2026-10-21]]

Filtering lets analysts select data matching specific criteria — file extensions, text strings, modification times — to quickly locate relevant information.

---

## 1️⃣ `grep`

Searches a specified file and returns all lines containing a specified string or text pattern. Takes **two arguments**: the string to search for, and the file to search through.

```bash
grep OS updates.txt      # all lines containing "OS"
grep error time_logs.txt # all lines containing "error"
```

---

## 2️⃣ Piping (`|`)

Sends the **standard output** of one command as **standard input** to another command.

- The pipe character (`|`) is usually on the same key as the backslash (`\`).
- Example: `ls /home/analyst/reports | grep users` — lists the `reports` directory and filters to items containing `users`.
- Piping is a general form of **redirection** in Linux: whenever one command's output should become another's input.

---

## 3️⃣ `find`

Searches for directories and files meeting specified criteria (name, size, last modified time).

- **First argument:** where to start searching, e.g. `find /home/analyst/projects`.
- **Options** modify behavior and usually begin with a hyphen (`-`).

### `-name` / `-iname`

Match file or directory names containing a string — quoted, with `*` wildcards for zero or more unknown characters.

- `-name` — **case-sensitive**
- `-iname` — **case-insensitive**

```bash
find /home/analyst/projects -name "*log*"   # case-sensitive
find /home/analyst/projects -iname "*log*"  # log, Log, LOG...
```

### `-mtime` / `-mmin`

Find files/directories modified within a time frame.

- `-mtime -3` — modified within the past 3 days
- `-mtime +1` — modified more than 1 day ago
- `-mtime -1` — modified less than 1 day ago
- `-mmin` — same idea, but in **minutes** instead of days

---

## 🎯 Key Takeaways

- **`grep`** filters text **inside files**, line by line.
- **Piping (`|`)** chains commands — output of one becomes input of another.
- **`find`** locates files/directories by attributes: name, path, time.
- **Wildcards (`*`)** stand for unknown characters and are essential for pattern searches.

---

## 🔗 Related

- [[Linux Commands]]
- [[Linux File System]]
- [[Logs & SIEM]]
- [[Linux, SQL & Python]]
