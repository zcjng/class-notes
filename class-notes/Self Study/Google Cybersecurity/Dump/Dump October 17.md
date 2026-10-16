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

## 🔒 Evolution of Wi-Fi Security
Wi-Fi protocols have adapted over time to match the reliability and security levels of traditional wired connections.

* **WPA (Wi-Fi Protected Access):** Introduced in **2004** as a wireless security protocol to allow devices to connect securely to the internet.
* **WPA2 & WPA3:** Newer iterations that introduced critical security improvements, including more **advanced encryption** methods.

## 🛡️ Security Analyst Role
* Responsible for ensuring all wireless network connections within an organization are fully secure and up to date.
