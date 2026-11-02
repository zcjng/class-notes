---
course: Coursera Google Cybersecurity
topic: SQL
tags:
  - google
  - cybersecurity
---

# 🧾 SQL Basics

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-22|Related day: 2026-10-22]]

**SQL (Structured Query Language)** is a programming language used to **create, interact with, and request information** from a database.

A **query** is a request for data from a database table or a combination of tables.

---

## 🔑 `SELECT` — specifies columns

Indicates exactly which data fields to return.

- **Single column:** `SELECT customerid`
- **Multiple columns:** separate with commas — `SELECT customerid, city`
- **All columns:** asterisk wildcard — `SELECT *`

> [!WARNING] Performance warning
> Avoid `SELECT *` on massive enterprise production databases — it can severely drag performance and produce unreadable output.

## 🔑 `FROM` — specifies the table

Indicates the table to query.

- **Semicolon (`;`)** at the very end signals the query is complete.
- Line breaks are ignored by SQL but strongly recommended for readability.

```sql
-- Clean multi-line layout
SELECT employee_id, device_id
FROM employees;

-- Single-line layout (runs exactly the same)
SELECT * FROM customers;
```

---

## 📊 `ORDER BY` — sorting output

Sequences results by one or more columns. Always placed at the **very end** of the query.

- **Ascending (default)** — text **A→Z**, numbers **smallest→largest**.
- **Descending (`DESC`)** — text **Z→A**, numbers **largest→smallest**.

```sql
SELECT customerid, city, country
FROM customers
ORDER BY city;          -- A → Z

SELECT customerid, city, country
FROM customers
ORDER BY city DESC;     -- Z → A
```

### Sorting by multiple columns

Pass multiple columns comma-separated: SQL sorts by the first, then uses the second to break ties.

```sql
-- Sorts globally by country, then alphabetically by city within each country
SELECT customerid, city, country
FROM customers
ORDER BY country, city;
```

---

## 🎯 Summary

| Keyword | Purpose | Placement |
| :--- | :--- | :--- |
| **`SELECT`** | *Which columns* to pull | First line |
| **`FROM`** | *Which table* to pull from | Follows `SELECT` |
| **`ORDER BY`** | Sorts rows alphabetically or numerically | End of query |
| **`DESC`** | Reverse/descending sort direction | Right after the column in `ORDER BY` |

---

## 🔗 Related

- [[Databases]]
- [[SQL Filtering]]
- [[SQL Joins]]
