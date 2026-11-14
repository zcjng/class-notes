
# Appoinment 

-> uses **http (port 80)**, and **gobuster** to enumarete throught the subdirectories

```bash
# Basic directory enumeration
gobuster dir -u https://example.com -w /path/to/wordlist.txt

# DNS subdomain enumeration
gobuster dns -do example.com -w /path/to/wordlist.txt

# Virtual host discovery
gobuster vhost -u https://example.com -w /path/to/wordlist.txt

# S3 bucket enumeration
gobuster s3 -w /path/to/bucket-names.txt
```

kali stores its wordlists in 
```bash
/usr/share/wordlists
```

# Sequel

-> uses mysql (port 3306)

```bash
mysql -h {HOSTNAME} -P 3306 -u {USER/root} -p
```

-> after successfully logging in:
```sql
SHOW DATABASES;               -- lists all databases on the server
USE {DATABASE};               -- selects a database (not a table)
SHOW TABLES;                  -- lists tables in the selected database
DESCRIBE {TABLE};             -- shows a table's columns and types
SELECT * FROM {TABLE};        -- returns every row in a table
SELECT * FROM {TABLE} WHERE {COLUMN} = 'value';   -- filters rows
```


# Crocodile

-> uses ftp (port 21) and gobuster
```bash
ftp -a {HOST_MACHINE}   # use 
```


```bash
# With file extensions
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt
```
