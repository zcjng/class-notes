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
