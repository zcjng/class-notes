# Course 3 Module 2

Network Protocols 
- **Definition:** Network protocols are sets of rules that govern the order of delivery and structure of data exchanged between devices on a network.

Key Network Protocols

- **Transmission Control Protocol (TCP):** Establishes connections and streams data between devices, verifying them through a "handshake" before communication.
- **Address Resolution Protocol (ARP):** Determines the MAC address of the next router or device in the data's path, ensuring correct delivery.
- **Hypertext Transfer Protocol Secure (HTTPS):** Provides secure communication between web browsers and servers, encrypting data using SSL/TLS, HTTPS uses port 443 meanwhile HTTP uses port 80
- **Domain Name System (DNS):** Translates internet domain names (like "yummyrecipesforme.org") into IP addresses, which are used as destination addresses for data packets. Uses port 53

Protocols and Security
- **HTTPS and Encryption:** HTTPS encrypts data using SSL (Secure Sockets Layer)/TLS (Transport Layer Security), protecting sensitive information from malicious actors during transmission.

Specific Protocol Examples

- **Communication Protocols:** TCP, UDP, HTTP, and DNS facilitate data transmission and communication between devices.
- **Management Protocols:** SNMP and ICMP are used for monitoring, managing, and troubleshooting network activity.
- **Security Protocols:** HTTPS and SFTP ensure secure data transmission through encryption, though they don't conceal IP addresses.
	- **Secure File Transfer Protocol (SFTP)** is a secure protocol used to transfer files from one device to another over a network. SFTP uses secure shell (SSH), typically through TCP port 22. SSH uses Advanced Encryption Standard (AES) and other types of encryption to ensure that unintended recipients cannot intercept the transmissions.

Some protocols are assigned port numbers by the **Internet Assigned Numbers Authority (IANA).**

This process is known as **Network Address Translation (NAT)**, replaces private source IP address with its public IP address. NAT is a part of layer 2 (internet layer) and layer 3 (transport layer) of the TCP/IP model.

Private IPs, Address ranges:
- 10.0.0.0-10.255.255.255
- 172.16.0.0-172.31.255.255
- 192.168.0.0-192.168.255.255

Public IPs, Address ranges:
- 1.0.0.0-9.255.255.255
- 11.0.0.0-126.255.255.255
- 128.0.0.0-172.15.255.255
- 172.32.0.0-192.167.255.255


**Dynamic Host Configuration Protocol (DHCP)**: application layer protocol, assigns unique IP addresses, provide addresses of the appropriate DNS server and default gateway for each device. 
- DHCP servers : port 67
- DHCP clients : port 68

Address Resolution Protocol (ARP) operates on layer 2, does not have a specific port number because port numbers are associated with the layer 7 Application layer.

### Telnet 
Telnet is an application layer protocol that is used to connect with a remote system, telnet is not as secure as SSH. Telnet can be used to connect to local or remote devices and uses TCP port 23.


## Secure shell
Secure shell protocol (SSH) is used to create a secure connection with a remote system. This application layer protocol provides an alternative for secure authentication and encrypted communication. SSH operates over the TCP port 22.


## Post office protocol
Post office protocol (POP) is an application layer (layer 4 of the TCP/IP model) protocol used to manage and retrieve email from a mail server. POP3 is the most commonly used version of POP. 
- Unencrypted, plaintext authentication uses TCP/UDP port 110 
- Encrypted emails use Secure Sockets Layer/Transport Layer Security (SSL/TLS) over TCP/UDP port 995.  

## Internet Message Access Protocol (IMAP)
IMAP is used for incoming email. It downloads the headers of emails and the message content. The content also remains on the email server, which allows users to access their email from multiple devices. 
- MAP uses TCP port 143 for unencrypted email
- TCP port 993 over the TLS protocol.
Using IMAP allows users to partially read email before it is finished downloading. Since the mail is kept on the mail server, it allows a user to sync emails across multiple devices.

## Simple Mail Transfer Protocol
Simple Mail Transfer Protocol (SMTP) is used to transmit and route email from the sender to the recipient’s address. SMTP works with Message Transfer Agent (MTA) software, which searches DNS servers to resolve email addresses to IP addresses, to ensure emails reach their intended destination. 
- SMTP uses TCP/UDP port 25 for unencrypted emails, often used by high-volume spam. SMTP helps to filter out spam by regulating how many emails a source can send at a time.
- TCP/UDP port 587 using TLS for encrypted emails
![[Pasted image 20261017073657.png]]

# 📡 IEEE 802.11 & Wi-Fi Security

## 🔹 Overview of IEEE 802.11
* **Definition:** Commonly known as **Wi-Fi**, it is a suite of communication protocols that defines standards for Wireless Local Area Networks (WLANs).
* **IEEE:** Stands for the **Institute of Electrical and Electronics Engineers**, the organization responsible for maintaining these wireless standards.

## 🌐 The Rise of Wireless Networking

* **Wi-Fi Terminology:** A marketing term commissioned by the **Wi-Fi Alliance** (formerly WECA). It refers to the **IEEE 802.11** family of standards maintained by the Institute of Electrical and Electronics Engineers.

---

## 🔒 Wireless Security Protocols Evolution

### 1. WEP (Wired Equivalent Privacy)
* **Launch:** Developed in **1999** (the oldest wireless standard).
* **Goal:** Intended to provide wireless connections with the same privacy level as wired networks.
* **Current Status:** Outdated and considered **high-risk**. Encryption flaws allow malicious actors to easily break it. 
* **Analyst Note:** Still encountered on legacy hardware or due to unconfigured router defaults.

### 2. WPA (Wi-Fi Protected Access)
* **Launch:** Developed in **2003** as a transitional measure compatible with older hardware.
* **Improvements over WEP:** Introduced **TKIP** (Temporal Key Integrity Protocol), larger secret keys, and a **Message Integrity Check** to reject altered transmissions.
* **Core Vulnerability:** Susceptible to **KRACK (Key Reinstallation Attacks)**, where an attacker intercepts the authentication handshake and forces an encryption key of all zeros.

### 3. WPA2 (Wi-Fi Protected Access 2)
* **Launch:** Released in **2004**; widely considered the current security standard.
* **Improvements over WPA:** Replaced TKIP with **AES** (Advanced Encryption Standard) and introduced **CCMP** for strong encapsulation and message integrity.
* **Core Vulnerability:** Remains vulnerable to KRACK attacks.

#### ⚖️ WPA2 Deployment Modes
* **Personal Mode:** Uses a global passphrase manually applied to each device. Easy to set up; ideal for home networks but unmanageable for corporations.
* **Enterprise Mode:** Features individualized, centralized control over access via an authentication server. Users never see encryption keys, preventing attackers from recovering keys from lost devices.

### 4. WPA3 (Wi-Fi Protected Access 3)
* **Launch:** Released in **2018**; steadily growing in adoption.
* **Improvements over WPA2:** 
  * Fixes the handshake vulnerability to prevent KRACK attacks.
  * Introduces **SAE (Simultaneous Authentication of Equals)**, a password-authenticated key agreement that blocks offline data decryption attempts.
  * Upgrades baseline protection to **128-bit encryption**, with an optional **192-bit encryption** tier for Enterprise mode.


# 🛡️ Firewall Types, Operations, and Management

## 🧱 What is a Firewall?
* **Definition:** A network security device that monitors incoming and outgoing network traffic.
* **Core Function:** Blocks or allows traffic based on a predefined set of security rules determined by an organization's security policy.
* **Port Filtering:** Limits unwanted communication by explicitly opening or closing specific ports (e.g., allowing **Port 443** for HTTPS or **Port 25** for email while dropping everything else).

---

## 🎛️ Types of Firewalls by Deployment

### 1. Hardware Firewall
* **Nature:** A physical, standalone appliance.
* **Function:** Inspects every individual data packet before it is permitted to physically enter the internal network.
* **Benefit:** Serves as the most foundational line of defense against network perimeter threats.

### 2. Software Firewall
* **Nature:** A software application installed directly onto existing hardware.
* **Computer Deployment:** Analyzes traffic strictly received by that specific local machine.
* **Server Deployment:** Protects all secondary devices connected to and relying on that server.
* **Trade-offs:** Costs less than physical hardware and saves physical space, but introduces a processing and resource burden to host devices.

### 3. Cloud-Based Firewall / Firewall as a Service (FaaS)
* **Nature:** Software firewalls hosted entirely by a third-party cloud service provider.
* **Operation:** Rules are configured via the provider's management interface. 
* **Traffic Flow:** Scrubs and filters incoming traffic in the cloud *before* it can reach the onsite enterprise network.
* **Scope:** Secures local networks as well as an organization's existing cloud-native assets and processes.

---

## 🧠 State Tracking Architecture

### Stateful Firewalls
* **Behavior:** Proactively tracks the connection state and information history of passing data packets.
* **Security Level:** Higher protection; dynamically analyzes characteristics and behavioral context to flag and block suspicious trends.

### Stateless Firewalls
* **Behavior:** Operates strictly on static, preconfigured rules established by an administrator.
* **Data Handling:** Does not store analyzed packet information or track connection states.
* **Security Level:** Lower protection; completely blind to broader behavioral anomalies or emerging threat trends.

---

## 🚀 Next-Generation Firewalls (NGFW)
* **Capabilities:** Delivers significantly higher security than standard stateful configurations.
* **Advanced Features:** Performs deep packet inspection (DPI) and active intrusion protection.
* **Threat Intelligence:** Connects directly to cloud-based threat intelligence services for real-time definitions update against zero-day and emerging exploits.

# 🔒 Virtual Private Networks (VPNs) & Encapsulation

## 🌐 The Privacy Risk of Standard Traffic
* **ISP Exposure:** When connecting to the internet normally, your Internet Service Provider (ISP) receives and forwards all network requests directly to destination servers.
* **Data Leakage:** Standard internet requests include your public IP address, private data, and physical location.
* **Interception Threat:** If unencrypted traffic is intercepted, malicious actors can tie your online activity to your physical location and steal highly sensitive information like banking details and credit card numbers.

---

## 🛡️ What is a VPN?
* **Definition:** A network security service that creates an encrypted tunnel between your local device and a remote VPN server.
* **Anonymization:** It masks your public IP address and hides your true virtual location.
* **Confidentiality:** It encrypts your data as it travels across the public internet, making it unreadable without the correct cryptographic key.

---

## 📦 Data Encapsulation Explained
A fundamental routing paradox exists: encrypting an entire data packet secures the information, but routers can no longer read the destination IP and MAC addresses in the header to deliver it. VPNs solve this using **encapsulation**.

* **The Process:** Encapsulation wraps sensitive, fully encrypted data packets inside completely separate, unencrypted outer packets.
* **How It Works:**
  1. **Encryption:** The VPN software encrypts your original data packet (including your real IP address and private data payload).
  2. **Encapsulation:** It wraps that encrypted packet inside a new, public packet containing the destination address of the VPN server.
  3. **Routing:** Network routers read the outer packet to successfully forward it across the internet.
  4. **Decryption:** Once it hits the VPN server, the outer layer is stripped, the inner packet is processed, and it is safely forwarded to the final web destination using the VPN's IP address instead of yours.


# 🌐 Network Segmentation & Security Zones

## 🧩 What is Network Segmentation?
* **Definition:** A security technique that divides a unified network into distinct, smaller segments.
* **Granular Security:** Each separate network segment maintains its own unique access permissions and security rules.
* **Blast Radius Mitigation:** Acts as a containment barrier; if a single segment is compromised or contaminated, administrators can isolate it to prevent lateral movement to the rest of the network.
* **Examples:**
  * **Hospitality:** A hotel hosting an unsecured public guest Wi-Fi network completely isolated from the encrypted staff network.
  * **Education:** A university separating network traffic into a faculty subnet and a student subnet.

---

## 🗂️ Classifications of Security Zones

### 🔴 1. Uncontrolled Zone
* **Definition:** Any external network that falls completely outside of the organization's control, monitoring, or administration.
* **Primary Example:** The public internet.

### 🟢 2. Controlled Zone
* **Definition:** The collective subnets and internal networks protected from the uncontrolled zone. It is structurally divided into three distinct layers:

	#### A. Demilitarized Zone (DMZ)
	* **Nature:** The network perimeter and outermost layer of the controlled zone.
	* **Purpose:** Houses public-facing services that must securely interact with the internet.
	* **Contained Assets:** Web servers, public proxy servers, external DNS servers, and external email or file-transfer servers.
	
	#### B. Internal Network
	* **Nature:** The core corporate network layer situated behind the DMZ.
	* **Purpose:** Houses the private servers, endpoints, and standard business data that the organization must protect from public exposure.
	
	#### C. Restricted Zone
	* **Nature:** The deepest, most isolated layer located entirely within the internal network.
	* **Purpose:** Protects highly confidential information and critical assets.
	* **Access Control:** Restricted strictly to specific internal employees with elevated privileges.

---

## 🧱 Perimeter Architecture & Firewall Deployment
To maximize defense-in-depth, firewalls are strategically deployed to isolate each security zone:

1. **External Firewall:** Filters and scrubs incoming traffic coming from the uncontrolled internet into the DMZ.
2. **Internal Firewall:** Sits between the DMZ and the internal network, blocking threats that penetrate the outer perimeter from spreading deeper.
3. **Restricted Firewall:** Guards the boundary of the restricted zone, ensuring even compromised internal network devices cannot access highly confidential data.
![[Pasted image 20261017080635.png]]

## 🛡️ Security Analyst Operations
* **Policy Regulation:** Responsible for implementing and enforcing access control policies across all zone-boundary firewalls.
* **Traffic Restriction:** Controls transit pathways by tightly restricting allowed IP addresses and communication ports.
* **Example Rule:** Configuring perimeter firewalls to only allow inbound **HTTPS (Port 443)** traffic directly to public web servers hosted inside the DMZ, while dropping all other traffic types.

**Subnetting** is the process of taking one large network and **dividing it into several smaller, organized groups** called **subnets**. 
	Subnetting divides up a network address range into smaller subnets within the network. These organized subnets are defined by the unique combination of the **IP address** and the **network mask** assigned to every device.

## 🌐 The Shift from Classful to Classless Addressing
* **Classful Addressing (1980s):** An outdated system that grouped IP addresses into strict categories (**Class A through Class E**). 
* **The Problem:** Each class had a rigid, limited number of available IP addresses. As the internet exploded in the 1990s, these fixed pools rapidly depleted.
* **Classless Inter-Domain Routing (CIDR):** Introduced to replace classful networking, dramatically expanding the efficiency and availability of the modern IPv4 address space.

---
## 🛠️ What is Classless Inter-Domain Routing (CIDR)?
* **Definition:** A flexible method of assigning subnet masks to IP addresses to split networks into customizable, smaller chunks.
* **Purpose:** Allows cybersecurity and network professionals to perform precise network segmentation, reduce the massive size of routing tables, and allocate IP addresses without wasting empty blocks.

## 📝 Understanding CIDR Notation
CIDR addresses look exactly like standard IPv4 addresses, but they add a trailing slash (`/`) followed by a specific number.
* **IP Network Prefix:** The number following the slash (e.g., the `24` in `/24`). It defines how many bits of the address are dedicated to the network identifier.
* **Notation Example:** `198.51.100.0/24`
* **Network Range:** The single `/24` example above encompasses the entire block of 256 sequential IP addresses ranging from **198.51.100.0 to 198.51.100.255**.

## 🧊 What is a Proxy Server?
* **Definition:** A dedicated server that acts as an intermediary, fulfilling client requests by forwarding them to other servers.
* **Network Position:** Sits directly between the public internet and the internal private network.
* **IP Anonymization:** Operates using a public IP address completely distinct from the internal network. This hides private host IPs from external discovery.
* **Security Mechanics:** When an external client receives a response, they see a distorted proxy IP or no IP at all, completely obscuring the organization's real backend server.

---

## 🛡️ Core Security and Performance Functions

* **Inbound Inspection:** Evaluates incoming internet connection requests to determine if they are safe before letting traffic interact with the network.
* **Content Filtering:** Enforces web policies by actively blocking employee access to unsafe, unauthorized, or malicious websites.
* **Caching (Temporary Memory):** Stores frequently requested data locally. By serving common requests directly from its cache, it minimizes direct external contact with internal servers, reducing the target's attack surface.

---

## 🗂️ Classifications of Proxy Servers

### 1. Forward Proxy Server
* **Direction:** Regulates and restricts **outgoing** traffic (Internal User ➡️ Internet).
* **Objective:** Intercepts traffic from internal employees, verifies it against organizational policy, hides the individual user's IP address, and forwards approved requests out to the internet.

### 2. Reverse Proxy Server
* **Direction:** Regulates and restricts **incoming** internet access to internal systems (Internet ➡️ Internal Server).
* **Objective:** Accepts traffic from external parties, validates the requests, and forwards them to backend servers.
* **Application:** Ideal for shielding internal web servers containing confidential data, ensuring their true IP addresses are never exposed to the public.

### 3. Email Proxy Server
* **Direction:** Inspects inbound and outbound electronic mail messaging.
* **Objective:** Specifically flags and filters spam and phishing attempts.
* **Verification:** Validates sender headers to ensure the source address was not forged, significantly mitigating business email compromise (BEC) and impersonation attacks.
* **Real-World Use:** Multi-layered anti-spam filtering architectures are frequently deployed on enterprise or ISP email proxies to scrub messages thoroughly before delivering them to a user's inbox.


---

