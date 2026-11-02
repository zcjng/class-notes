---
course: Coursera Google Cybersecurity
topic: SQL
tags:
  - google
  - cybersecurity
---

# 🔗 SQL Joins

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-22|Related day: 2026-10-22]]

Joins combine data fields across multiple tables by linking them through a **shared relational key**.

---

## 🔍 INNER JOIN — strict match only

Returns records **only if a matching value exists in both tables** — the absolute intersection.

- Rows without a matching key are completely hidden.
- `SELECT *` returns columns present in both tables **twice**.
- **Dot notation** is required when a column name exists in both tables: `table_name.column_name`.

```sql
SELECT username, operating_system, employees.device_id
FROM employees
INNER JOIN machines ON employees.device_id = machines.device_id;
```

---

## 📐 OUTER JOINS — expanding results

Preserve unmatched rows from one or both tables; missing entries are filled with **`NULL`**.

### LEFT JOIN — left-table priority

All records from the **first (left)** table, but only matching records from the right.

> [!TIP] Security context
> Great for finding employees who **don't yet have a corporate machine assigned**.

```sql
SELECT *
FROM employees
LEFT JOIN machines ON employees.device_id = machines.device_id;
```

### RIGHT JOIN — right-table priority

All records from the **second (right)** table, but only matching records from the left.

```sql
SELECT *
FROM employees
RIGHT JOIN machines ON employees.device_id = machines.device_id;
```

> [!NOTE] Pro tip
> A `LEFT JOIN` and a `RIGHT JOIN` produce the same output if you **swap the table order**.

### FULL OUTER JOIN — complete merge

Returns **all records from both tables**, matched or not.

```sql
SELECT *
FROM employees
FULL OUTER JOIN machines ON employees.device_id = machines.device_id;
```

---

## 🎯 Quick reference

| Join type | Records returned | Visual concept | Table order matters? |
| :--- | :--- | :--- | :---: |
| **`INNER JOIN`** | Only entries matching **both** sides | Intersection slice | No |
| **`LEFT JOIN`** | All left entries + matching right entries | Left circle full + overlap | Yes |
| **`RIGHT JOIN`** | All right entries + matching left entries | Right circle full + overlap | Yes |
| **`FULL OUTER JOIN`** | All entries across **both** tables | Both circles filled | No |

---

## 🔗 Related

- [[SQL Basics]]
- [[SQL Filtering]]
- [[Databases]]
