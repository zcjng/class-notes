# NMAP scanning

-> nmap is a VERY crucial enumeration tool usually by
```bash
sudo nmap -sC -sV --version -oA path/to/dir {MACHINE_IP} 
```
-> uses default scripts, to enumerate services inspects what 1000 default ports are running 
```bash
sudo nmap -p- --min-rate 1000 -T4 {MACHINE_IP}
```
-> forces 1000 packets/sec with aggresive timing (T4) scans all open ports 65535 TCP ports

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
ftp -a {MACHINE_IP} # login
>ftp dir
>ftp get            # downloads file to current path before ftp
```

# Dancing

-> uses smb | server message block (port 445)

```bash
smbclient -L 10.129.209.120 # List all of the shares available on the host
smbclient //10.129.209.120/{Available Shares} # connect to the host
```

-> after connecting, we can traverse the directory, and use **get** and **more** to look and download the flag.


# Redeemer

-> uses redis (port 6379), default 1000 tcp scans wont work, so scan all 65535

```bash
redis-cli -h {HOSTNAME}      # connects to the Redis database
>INFO                  # Server details; check the "Keyspace" section at the bottom for active DBs
>SELECT {database}     # Switch to a specific DB index (e.g., SELECT 0)
>KEYS *                # List all keys in the selected DB
>SCAN 0                # Non-blocking key iteration (returns cursor + keys)
>TYPE {key}            # Check the data type before reading it
> GET {key}            # Read a 'string' key 
>HGETALL {key}         # Read a 'hash' key 
>LRANGE {key} 0 -1     # Read a 'list' key 
>SMEMBERS {key}        # Read a 'set' key 
>ZRANGE {key} 0 -1     # Read a 'zset' (sorted set) key
```






