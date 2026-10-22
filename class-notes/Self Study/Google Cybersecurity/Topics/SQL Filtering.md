---
course: Coursera Google Cybersecurity
topic: SQL
tags:
  - google
  - cybersecurity
---

# 🔍 SQL Filtering

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-22|Related day: 2026-10-22]]

---

## 🛡️ Why filtering matters in security

Security analysts work with massive, complicated logs. Filtering cuts the noise to:

- Isolate login attempts for a **specific user**.
- View all system events at the **exact time of an incident**.
- Identify which devices run an **outdated application version**.

---

## 🔎 Exact Filtering — `WHERE`

Applies a strict condition; rows that don't meet it are hidden. Follows `FROM`; the semicolon moves to the end of the `WHERE` line.

```sql
SELECT employee_id, first_name, title
FROM employees
WHERE title = 'IT Staff';
```

*(Text strings in a `WHERE` clause must be wrapped in **single quotes**.)*

---

## 🎨 Pattern Filtering — `LIKE` & wildcards

Swap `=` for **`LIKE`** when you don't know the exact value or want a range of similar values.

| Wildcard | Matches | Example | Possible matches |
| :--- | :--- | :--- | :--- |
| **`%`** | **Any number** of characters (0 or more) | `'IT%'` | `IT Staff`, `IT Manager`, `IT` |
| **`_`** | **Exactly one** character | `'N_'` | `NY`, `NV`, `NS`, `NT` (not `NYPD`) |

### Pattern reference

| Pattern | Meaning | Example match |
| :--- | :--- | :--- |
| `'a%'` | Must **start** with "a" | `apple123`, `art`, `a` |
| `'%a'` | Must **end** with "a" | `pizza`, `Z6ra`, `a` |
| `'%a%'` | Contains "a" **anywhere** | `Again`, `back`, `a` |
| `'a_'` | Starts with "a" + **exactly 1** character | `as`, `an`, `a7` |
| `'a__'` | Starts with "a" + **exactly 2** characters | `ant`, `add` |
| `'_a_'` | Exactly 3 characters, "a" in the middle | `Car`, `ban` |

```sql
SELECT *
FROM employees
WHERE title LIKE 'IT%';

SELECT *
FROM invoices
WHERE state LIKE 'N_';
```

---

## 📊 Numbers, Dates & Times

Security data is not just text — you'll analyze **counts** of failed logins, **bytes transmitted**, **timestamps**, login times, patch dates, and connection durations.

### Comparison operators

| Operator | Meaning | Behavior |
| :---: | :--- | :--- |
| `<` | Less than | Exclusive |
| `>` | Greater than | Exclusive |
| `=` | Equal to | Exact |
| `<=` | Less than or equal to | Inclusive |
| `>=` | Greater than or equal to | Inclusive |
| `<>` or `!=` | Not equal to | Excludes the exact value |

> [!NOTE] Date formatting
> Numbers are typed raw (`WHERE login_count > 5`); **dates/times must be wrapped in single quotes** in `YYYY-MM-DD` format.

```sql
SELECT first_name, last_name
FROM employees
WHERE birthdate > '1970-01-01';
```

### `BETWEEN` — range filtering

Syntax: `BETWEEN ValueA AND ValueB`. **Strictly inclusive** — boundary matches are captured.

```sql
-- Hired from Jan 1 2002 up to and including Jan 1 2003
SELECT first_name, last_name
FROM employees
WHERE hiredate BETWEEN '2002-01-01' AND '2003-01-01';
```

---

## 🧠 Logical Operators

| Operator | Evaluates true if… | Visual concept |
| :---: | :--- | :--- |
| **`AND`** | **Both** conditions match | Only the overlap / intersection |
| **`OR`** | **Either** condition matches | Combines both groups |
| **`NOT`** | The condition **does not** match | Everything except the target |

### `AND`

```sql
SELECT first_name, last_name, email
FROM customers
WHERE supportrepid = 5 AND country = 'USA';
```

### `OR`

> [!WARNING] Syntax trap
> Write out the full column name for **every** condition, even when checking the same column twice.

```sql
-- CORRECT
WHERE country = 'Canada' OR country = 'USA';
-- WRONG: WHERE country = 'Canada' OR 'USA';
```

### `NOT`

Placed directly after `WHERE`; reverses the condition.

```sql
SELECT first_name, last_name, country
FROM customers
WHERE NOT country = 'USA';
```

`WHERE NOT country = 'USA'` is equivalent to `country <> 'USA'` or `country != 'USA'`.

### Combining operators

```sql
-- All international customers outside North America
SELECT first_name, last_name, country
FROM customers
WHERE NOT country = 'USA' AND NOT country = 'Canada';
```

---

## 🔗 Related

- [[SQL Basics]]
- [[SQL Joins]]
- [[Databases]]
- [[Logs & SIEM]]
