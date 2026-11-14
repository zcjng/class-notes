# NMAP scanning

-> nmap is a VERY crucial enumeration tool usually by
```bash
sudo nmap -sC -sV --version -oA path/to/dir {MACHINE_IP} 
```

# Meow

-> uses **telnet (port 23)** to gain root by
```bash
telnet {MACHINE_IP}
```
-> prompts for a log-in, unsecured can use:
- username: root,  no password.

# Fawn

-> uses ftp (port 21) to gain root by
```bash
ftp -?
ftp -a {MACHINE_IP} | -> login
>ftp dir
>ftp get            | -> downloads file to current path before ftp
```

# Dancing

-> uses smb | server message block (port 445)

```bash
smbclient -L 10.129.209.120 | -> List all of the shares available on the host
smbclient //10.129.209.120/{Available Shares} | -> connect to the host
```

-> after connecting, we can traverse the directory, and use **get** and **more** to look and download the flag.

