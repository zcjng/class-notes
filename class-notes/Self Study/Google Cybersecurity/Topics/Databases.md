---
course: Coursera Google Cybersecurity
topic: SQL
tags:
  - google
  - cybersecurity
---

# 🗄️ Databases

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-22|Related day: 2026-10-22]]

A **database** is an organized collection of information or data designed for **scaling, speed, and simultaneous multi-user access**.

---

## 📊 Databases vs. Spreadsheets

| Feature | Spreadsheets (e.g. Google Sheets) | Databases |
| :--- | :--- | :--- |
| **Target user** | Single user or small teams | Multiple simultaneous users |
| **Data capacity** | Limited; designed for less data | Massive amounts of data |
| **Capability** | Manual tracking, simple formatting | Highly complex tasks and queries |

> [!INFO] Security analyst context
> You'll frequently query databases to investigate **login attempts**, **software/update statuses**, and inventories of **machines and their owners**.

---

## 🗃️ Relational Databases

A **relational database** is a structured system containing **multiple tables connected through shared columns**.

### Anatomy of a Table

- **Fields** — the **columns**, specifying the type of information stored (`employee_id`, `device_id`, `username`…).
- **Rows / records** — the individual entries filled with data for each column (e.g. employee `1000` in `Marketing`).

---

## 🔑 Database Keys

Tables connect when they share a common column — these connecting columns are **keys**.

### Primary Key (PK)
- A column where **every row has a unique entry**.
- **No duplicates**, **no null/empty values**, and a table can have only **one** primary key.
- *Example:* `employee_id` uniquely identifies each worker.

### Foreign Key (FK)
- A column that acts as a **primary key in a different table**.
- **Can** contain duplicates and **can** contain empty (null) values; a table can have **multiple** foreign keys.
- *Example:* `employee_id` in a Machines table links hardware back to its owner.

---

## 🔗 Related

- [[SQL Basics]]
- [[SQL Filtering]]
- [[SQL Joins]]
- [[Linux, SQL & Python]]
