
Course 4 Module 4
 
# Introduction to Databases & Relational Structure

## 📌 What is a Database?
A **database** is an organized collection of information or data designed for scaling, speed, and simultaneous multi-user access.

### Databases vs. Spreadsheets

| Feature | Spreadsheets (e.g., Google Sheets) | Databases |
| :--- | :--- | :--- |
| **Target User** | Single user or small teams | Multiple simultaneous users |
| **Data Capacity** | Limited; designed for less data | Massive amounts of data |
| **Capability** | Manual tracking and simple formatting | Performs highly complex tasks and queries |

> [!INFO] Security Analyst Context
> You will frequently access databases to investigate **login attempts**, **software/update statuses**, and inventory lists of **machines and their owners**.

---

## 🗄️ Relational Databases
A **relational database** is a structured system containing multiple tables that are connected to one another through shared columns.

### Anatomy of a Table
* **Fields:** The **columns** of a table specifying the type of information stored (e.g., `employee_id`, `device_id`, `username`).
* **Rows / Records:** The specific entries filled with data corresponding to the columns (e.g., Employee `1000` working in `Marketing`).

---

## 🔑 Database Keys (Establishing Relationships)
Tables are connected together when they share a common column. These connecting columns are known as **Keys**.

> [!SUCCESS] Key Relationship Example
> * **Employees Table** contains a unique column named `employee_id`.
> * **Machines Table** links to it by using that same `employee_id` column.

### 1. Primary Key (PK)
* **Definition:** A column where every row has a **unique entry**.
* **Rules:** 
	* Strictly **no duplicate values**.
	* Strictly **no null or empty values**.
	* A table can only have **one** primary key.
* *Example:* `employee_id` in an Employees table uniquely identifies each distinct worker.

### 2. Foreign Key (FK)
* **Definition:** A column in a table that acts as a primary key in a *different* table.
* **Rules:** 
	* **Can** contain duplicate values.
	* **Can** contain empty (null) values.
	* A table can have **multiple** foreign keys.
* *Example:* `employee_id` inside a Machines table allows you to connect a hardware asset back to its corresponding owner.


## SQL (Structured Query Language)
A programming language used to create, interact with, and request information from a database

A query is a request for data from a database table or a combination of tables.





## 🔑 Core Keywords: SELECT & FROM
Every basic SQL query requires these two essential components:

### 1. `SELECT` (Specifies Columns)
Indicates exactly which data fields you want to return.
* **Single Column:** `SELECT customerid`
* **Multiple Columns:** Separate with commas (`SELECT customerid, city`)
* **All Columns:** Use an asterisk wildcard (`SELECT *`)

> [!WARNING] Performance Warning
> Avoid using `SELECT *` on massive, enterprise-level production databases. It can severely drag system performance and generate unreadable outputs.

### 2. `FROM` (Specifies Table)
Indicates the exact table name you want to query.
* **Semicolon ( ; ):** Placed at the very end of the statement to signal to the SQL engine that the query is complete.
* **Formatting:** Line breaks are optional and ignored by SQL, but highly recommended to keep code readable.

```sql
-- Clean multi-line layout
SELECT employee_id, device_id
FROM employees;

-- Single-line layout (runs exactly the same)
SELECT * FROM customers;
```

---

## 📊 Organizing Output: ORDER BY
The `ORDER BY` clause sequences your query results based on the values in one or more specified columns. It is always placed at the **very end** of the query.

### 1. Ascending Order (Default)
If you don't specify a sorting direction, SQL automatically sorts in ascending order.
* **Text / Alphabetical:** Sorts from **A to Z** (e.g., sorting by `city`).
* **Numeric:** Sorts from **smallest to largest** (e.g., `1` to `100`).

```sql
SELECT customerid, city, country
FROM customers
ORDER BY city;
```

### 2. Descending Order (`DESC`)
To flip the sorting order, append the `DESC` keyword directly after the column name.
* **Text / Alphabetical:** Sorts from **Z to A**.
* **Numeric:** Sorts from **largest to smallest**.

```sql
SELECT customerid, city, country
FROM customers
ORDER BY city DESC;
```

### 3. Sorting by Multiple Columns
You can pass multiple columns separated by a comma. SQL will sort by the first column first, and then use the second column to break any ties.

```sql
-- Sorts globally by country, then alphabetically by city within each country
SELECT customerid, city, country
FROM customers
ORDER BY country, city;
```

---

## 🎯 Summary Sheet

| Keyword | Purpose | Placement |
| :--- | :--- | :--- |
| **`SELECT`** | Defines *what columns* to pull | First line of the query |
| **`FROM`** | Defines *which table* to pull from | Follows `SELECT` |
| **`ORDER BY`** | Sorts the rows alphabetically or numerically | End of the query |
| **`DESC`** | Forces a reverse / descending sort direction | Right after the column in `ORDER BY` |


# SQL Filtering Foundations: WHERE & LIKE

## 🛡️ Why Filtering Matters in Security
Security analysts work with massive, complicated security logs. Filtering allows you to cut through the noise to:
* Isolate login attempts for a **specific user**.
* View all system events that occurred at the **exact time of an incident**.
* Identify which network devices are running an **outdated application version**.

---

## 🔍 Exact Filtering: The WHERE Clause
The `WHERE` keyword applies a strict condition to your query. Rows that do not meet the condition are completely hidden from the output.

* **Placement:** Follows the `FROM` clause.
* **The Semicolon ( ; ):** Moves to the very end of the `WHERE` statement line.
* **Operator:** Uses the equals sign (`=`) for exact text or numeric matches.

```sql
SELECT employee_id, first_name, title
FROM employees
WHERE title = 'IT Staff';
```
*(Note: Text strings inside a WHERE clause must be wrapped in single quotes).*

---

## 🎨 Pattern Filtering: LIKE & Wildcards
When you don't know the exact value or want to capture a range of similar values (e.g., finding both `IT Staff` and `IT Manager`), you swap the `=` operator for **`LIKE`** and use **wildcards**.

### 🧩 The Two Essential Wildcards

| Wildcard | What it Matches | Example Pattern | Possible Matches |
| :--- | :--- | :--- | :--- |
| **`%`** (Percent) | **Any number** of characters (0 or more) | `'IT%'` | `IT Staff`, `IT Manager`, `IT` |
| **`_`** (Underscore) | **Exactly one** single character | `'N_'` | `NY`, `NV`, `NS`, `NT` (but not `NYPD`) |

### 📋 Pattern Reference Sheet

| Pattern Syntax | Logical Meaning | Example Matched Output |
| :--- | :--- | :--- |
| **`'a%'`** | Must **start** with "a" | `apple123`, `art`, `a` |
| **`'%a'`** | Must **end** with "a" | `pizza`, `Z6ra`, `a` |
| **`'%a%'`** | Contains "a" **anywhere** inside it | `Again`, `back`, `a` |
| **`'a_'`** | Starts with "a" and has **exactly 1** character after it | `as`, `an`, `a7` |
| **`'a__'`** | Starts with "a" and has **exactly 2** characters after it | `ant`, `add` |
| **`'_a_'`** | Has **exactly 3** characters, with "a" in the middle | `Car`, `ban` |

---

### 💻 Code Examples

#### Example 1: Finding all IT-related roles
```sql
SELECT *
FROM employees
WHERE title LIKE 'IT%';
```

#### Example 2: Filtering logs by explicit state abbreviation patterns
```sql
SELECT *
FROM invoices
WHERE state LIKE 'N_';
```

# SQL Filtering: Numbers, Dates, & Times

## 📊 Non-String Data Types in Security
Security analysts do not just handle text strings. You will constantly analyze numeric and chronological data points:

### 1. Numeric Data
* The **count** of failed login attempts or specific alert categories.
* The **volume of bytes** transmitted from a source or received by a destination (identifying data exfiltration).

### 2. Date and Time Data
* **Timestamps** automatically appended to system logs.
* Core timelines like **login times**, **patch deployment dates**, and network **connection durations**.

---

## ⚙️ Comparison Operators
To filter numbers and dates inside a `WHERE` clause, use standard arithmetic operators.

| Operator | Meaning | Behavior |
| :---: | :--- | :--- |
| **`<`** | Less than | Exclusive (Does not include the target value) |
| **`>`** | Greater than | Exclusive (Does not include the target value) |
| **`=`** | Equal to | Matches the exact value |
| **`<=`** | Less than or equal to | Inclusive (Includes the target value) |
| **`>=`** | Greater than or equal to | Inclusive (Includes the target value) |
| **`<>`** or **`!=`** | **Not equal to** | Excludes the exact value |

### 💡 Formatting Rule for Dates
While numbers are typed raw (`WHERE login_count > 5`), **dates and times must be wrapped in single quotes** using the `YYYY-MM-DD` standard format (`WHERE birthdate > '1970-01-01'`).

```sql
-- Finds employees born strictly AFTER January 1, 1970
SELECT first_name, last_name
FROM employees
WHERE birthdate > '1970-01-01';
```

---

## 🗺️ Range Filtering: The BETWEEN Operator
The `BETWEEN` operator isolates data within a specified structural range. It uses the syntax: `BETWEEN ValueA AND ValueB`.

* **Behavior:** It is **strictly inclusive**. It will capture entries matching the boundaries exactly.

```sql
-- Finds employees hired from Jan 1, 2002 up to and including Jan 1, 2003
SELECT first_name, last_name
FROM employees
WHERE hiredate BETWEEN '2002-01-01' AND '2003-01-01';
```


# SQL Filtering: Logical Operators (AND, OR, NOT)

Logical operators allow security analysts to chain multiple conditions together to isolate complex security events or specific groups of users.

---

## 🛠️ The Three Core Logical Operators

### 1. `AND` (Intersection)
* **Rule:** **Both** conditions on either side of the operator must be met simultaneously for a row to be returned.
* **Security Use Case:** Pinpointing an active threat that requires multiple indicators to match.

```sql
-- Finds customers who are in the USA AND handled by Support Rep ID 5
SELECT first_name, last_name, email
FROM customers
WHERE supportrepid = 5 AND country = 'USA';
```

### 2. `OR` (Union)
* **Rule:** At least **one** of the conditions (the first, the second, or both) must be met for a row to be returned.
* **Syntax Warning:** You must write out the full column name for *every* condition, even if you are checking the same column twice.

```sql
-- CORRECT Syntax (Repeats the 'country' column name)
SELECT first_name, last_name, email
FROM customers
WHERE country = 'Canada' OR country = 'USA';

-- ❌ INCORRECT Syntax (Will fail)
-- WHERE country = 'Canada' OR 'USA';
```

### 3. `NOT` (Negation)
* **Rule:** Reverses a condition. It returns all records that do **not** match the specified value.
* **Placement:** Placed directly after the `WHERE` keyword.

```sql
-- Pulls all customers except those living in the USA
SELECT first_name, last_name, country
FROM customers
WHERE NOT country = 'USA';
```

> [!TIP] Pro Tip: Alternative Syntax
> Using `WHERE NOT country = 'USA'` performs exactly the same as using comparison operators:
> * `WHERE country <> 'USA'`
> * `WHERE country != 'USA'`

---

## 🧠 Combining Logical Operators
You can chain multiple logical operators together to handle complex exclusions or multi-country filtering profiles.

```sql
-- Safely isolates all international customers outside of North America (USA & Canada)
SELECT first_name, last_name, country
FROM customers
WHERE NOT country = 'USA' AND NOT country = 'Canada';
```

---

## 🎯 Logical Operators Summary Sheet

| Operator | Evaluates To True If... | Visual Concept |
| :---: | :--- | :--- |
| **`AND`** | **Both** conditions match | Only the overlap / intersection |
| **`OR`** | **Either** condition matches | Combines both groups entirely |
| **`NOT`** | The condition **does not** match | Everything *except* the target |


# SQL Joins: Connecting Tables

SQL Joins combine data fields across multiple tables by linking them together through a shared, common column (the relational key).

---

## 🔍 1. INNER JOIN (Strict Match Only)
An **Inner Join** returns records **only if a matching value exists in both tables**. It isolates the absolute intersection of your data.

* **Behavior:** If a row in the left table doesn't have a matching key in the right table, it is completely hidden.
* **Wildcard Note:** If you use `SELECT *`, columns that exist in both tables will be returned **twice** in your output.

```sql
SELECT username, operating_system, employees.device_id
FROM employees
INNER JOIN machines ON employees.device_id = machines.device_id;
```
> ⚠️ **Syntax Rule:** When pulling a column name that exists in *both* tables (like `device_id`), you must specify the source table using dot notation: `table_name.column_name`.

---

## 📐 2. OUTER JOINS (Expanding Results)
Outer joins preserve unmatched data rows from one or both tables instead of throwing them away. Missing matching entries are automatically filled with **`NULL`**.

### A. LEFT JOIN (Left-Table Priority)
Returns **all** records from the first (Left) table, but **only** matching records from the second (Right) table.

* **Security Context:** Finding which employees do *not* have a corporate machine assigned to them yet.

```sql
SELECT *
FROM employees
LEFT JOIN machines ON employees.device_id = machines.device_id;
```

### B. RIGHT JOIN (Right-Table Priority)
Returns **all** records from the second (Right) table, but **only** matching records from the first (Left) table.

```sql
SELECT *
FROM employees
RIGHT JOIN machines ON employees.device_id = machines.device_id;
```

> 🔄 **Pro Tip:** A `LEFT JOIN` and a `RIGHT JOIN` can produce the exact same data outputs if you simply swap the table names before and after the join keyword.

### C. FULL OUTER JOIN (Complete Merge)
Completely merges both tables. It returns **all records from both tables**, regardless of whether a match exists or not. 

```sql
SELECT *
FROM employees
FULL OUTER JOIN machines ON employees.device_id = machines.device_id;
```

---

## 🎯 Joins Quick-Reference Cheat Sheet

| Join Type | Records Returned | Visual Concept | Table Order Matter? |
| :--- | :--- | :--- | :---: |
| **`INNER JOIN`** | Only entries that match **both** sides | The intersection slice | ❌ No |
| **`LEFT JOIN`** | All Left entries + matching Right entries | Left circle full + overlap |  Yes |
| **`RIGHT JOIN`** | All Right entries + matching Left entries | Right circle full + overlap |  Yes |
| **`FULL OUTER JOIN`**| All entries across **both** tables completely | Both circles completely filled | ❌ No |
