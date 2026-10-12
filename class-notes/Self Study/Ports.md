# 🌐 Computer Networking: IP Addresses, Ports, and Sockets

## 📌 Core Concepts

### The Address vs. The Service
* **IP Address:** Determines the physical or logical **location** of a host on a network (e.g., `43.14.250.12`).
* **Port Number:** Determines **which service or program** on that server a connection wants to use.
* **The Rule:** Ports will always be associated with an IP address. Combined together, they form a **Socket**.

> [!TIP]
> **The Apartment Analogy** 🏢
> Think of an **IP Address** as the street address of an apartment building. The **Port Number** is the specific apartment number inside that building. A data packet needs both to reach the correct destination program.

---

## 🔀 Common Port Numbers
Ports have unique numbers ranging from **0 to 65535**.

| Port Number | Protocol / Service | Common Use Case |
| :--- | :--- | :--- |
| **21** | **FTP** (File Transfer Protocol) | Transferring files between a client and a server. |
| **25** | **SMTP** (Simple Mail Transfer Protocol) | Routing and sending email traffic between mail servers. |
| **80** | **HTTP** (Hypertext Transfer Protocol) | Unencrypted webpage traffic. |
| **443** | **HTTPS** (HTTP Secure) | Encrypted, secure webpage traffic. |

---

## 🏷️ IANA Port Categories
Port numbers are officially assigned and managed by the **IANA** (Internet Assigned Numbers Authority). They are broken down into 3 distinct categories:

* 🟢 **0 – 1023 | System or Well-Known Ports:** Reserved for core internet protocols and widespread system services.
* 🟡 **1024 – 49151 | User or Registered Ports:** Used by specific applications, vendors, or corporations for registered services.
* 🔵 **49152 – 65535 | Dynamic or Private Ports:** Used temporarily as temporary "ephemeral" exit ports by your operating system when initiating outbound connections.

---

## 🖥️ Local Servers & Network Monitoring

### Hosting Services Locally
Your computer can act as its own server to provide background network services. For example:
`192.168.0.12:80`
* **192.168.0.12** is a local IP address.
* **:80** means it is a web server listening on port 80, waiting to accept incoming traffic connections.

### Monitoring Active Connections
You can use the native command-line utility `netstat` to view your current system connections.

```bash
netstat -an
```

* `-a`: Displays all active connections and the listening ports.
* `-n`: Displays addresses and port numbers numerically (preventing the system from attempting to resolve names, making the output load instantly).



