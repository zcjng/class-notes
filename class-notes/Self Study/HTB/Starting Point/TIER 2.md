

# Vaccine

-> uses primarily ftp (port 20) and http (port 80)

firstly use ftp to grab the backup.zip

-> use zip2john and john to grab the zip encrypted pass and decode the source code 
```bash
zip2john backup.zip > hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt pass.txt
```

-> now you can login and see a database

```
```

