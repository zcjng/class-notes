
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
