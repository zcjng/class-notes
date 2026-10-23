SQL Injection might be possible in:

- In UPDATE statements, within the updated values or the WHERE clause.
- In INSERT statements, within the inserted values.
- In SELECT statements, within the table or column name.
- In SELECT statements, within the ORDER BY clause.

For queries that searches up the table using WHERE we can inject 
- `'--`   and `'+OR+1=1--` to find unlisted products.
It can also be used for log in pages
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```
You can directly modify the username parameter in the log in page website

**UNION** can be used to retrieve data from other tables, execute one or more additional **SELECT** 

```sql
SELECT a, b FROM table1 UNION SELECT c, d FROM table2
```

Requirements:
- The individual queries must return the same number of columns. 
	(How many columns are being returned from original query)
	- We can increment columns using **ORDER BY**
	- We can also use **UNION SELECT NULL**
- The data types in each column must be compatible between the individual queries.
	(Which columns returned are of a suitable data type)
