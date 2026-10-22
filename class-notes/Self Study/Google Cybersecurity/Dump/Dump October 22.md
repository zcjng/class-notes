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