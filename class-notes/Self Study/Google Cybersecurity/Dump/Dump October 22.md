RWX permissions

On dir.
R - Able to see the contents inside dir
W - New files can be created in dir
X - Enter into dir to access the files

 apropos -a for 2 strings
 
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
