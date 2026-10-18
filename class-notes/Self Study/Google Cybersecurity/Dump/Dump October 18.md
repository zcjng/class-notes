# Network Security Core Notes: DoS, Packet Sniffing, and IP Spoofing

## 1. Denial of Service Attacks (DoS & DDoS)

### Basic Concepts

- **DoS (Denial of Service)**: An attack targeting a network or server by flooding it with network traffic to disrupt normal business operations. The goal is to overwhelm a network device so it crashes or cannot respond to legitimate users, costing time and money while exposing the target to further vulnerabilities.
    
- **DDoS (Distributed Denial of Service)**: A DoS attack utilizing multiple devices or servers across different locations to flood the target, making it far more likely to overwhelm the server.
    
- **Nature of the Attack**: In DoS, the attacker wins if _any_ part of the network is overloaded. This can occur via raw traffic volume or via carefully crafted packets designed to force hardware (like routers) to expend excessive processing time.
    

### Common Network-Level DoS Attacks

|**Attack Name**|**Protocol**|**Mechanism**|**Core Impact**|
|---|---|---|---|
|**SYN Flood**|TCP|Exploits the TCP three-way handshake (`SYN` $\rightarrow$ `SYN/ACK` $\rightarrow$ `ACK`). The attacker floods the server with `SYN` requests without completing the final handshake step.|Leaves ports open and exhausts available server ports, preventing legitimate connections.|
|**ICMP Flood**|ICMP|Repeatedly sends ICMP packets to a target server, forcing it to generate corresponding ICMP replies.|Consumes all available inbound and outbound bandwidth, causing the server to crash.|
|**Ping of Death**|ICMP|Sends an oversized ICMP packet exceeding the standard maximum limit of **64 KB**.|Overloads vulnerable systems during packet reassembly/processing, crashing the target (analogous to dropping a large rock on an anthill).|

## 2. Packet Sniffing & Protocol Analyzers

### What is Packet Sniffing?

- Data packets contain a **header** (source/destination IP addresses) and a **body** (payload such as personal information, credentials, or financial data).
    
- Packet sniffing is the practice of using software or hardware tools to intercept and inspect data packets as they travel across a network.
    
- Used legitimately by security analysts to investigate incidents, troubleshoot, and baseline traffic, but also used maliciously to spy on unauthorized communications.
    

### Operational Modes

- **Passive Packet Sniffing**: The attacker reads packets in transit without altering them (comparable to a mail carrier reading personal mail inside an envelope). Common on hub networks where all traffic is visible to all connected hosts.
    
- **Active Packet Sniffing**: The attacker alters packets in transit, modifies payload data (such as bank details), or injects protocols to reroute traffic to unintended ports.
    
- **Promiscuous Mode**: By default, a Network Interface Card (NIC) only accepts packets destined for its own MAC address. In promiscuous mode, the NIC accepts all network traffic passing through the physical medium.
    

### Common Protocol Analyzer Tools

- **SolarWinds NetFlow Traffic Analyzer**
    
- **ManageEngine OpManager**
    
- **Azure Network Watcher**
    
- **Wireshark**
    
- **tcpdump**
    

### Focus Tool: tcpdump

> A popular, lightweight, command-line network protocol analyzer built on the open-source `libpcap` library, preinstalled on many Linux distributions and available on macOS/Unix systems.

- **Standard Output Fields**:
    
    - **Timestamp**: Time format (hours, minutes, seconds, fractions of a second).
        
    - **Source IP & Port**: Originating device address and port number.
        
    - **Destination IP & Port**: Target device address and receiving port number.
        
    - _Default behavior_: Attempts to resolve IPs into hostnames and port numbers into corresponding service names.
        
- **Core Analyst Use Cases**: Establishing network baselines, identifying unauthorized traffic/access points, detecting malicious payloads, and configuring custom alerts.
    

## 3. IP Spoofing & Related Attacks

### Fundamentals

IP spoofing occurs when an attacker alters the source IP address in a packet header to impersonate a trusted or authorized system. This technique is used to bypass firewall rules that filter external traffic and hide the attacker's true origin.

### Common IP Spoofing Attack Variations

```
                  ┌──► On-path Attack (Man-in-the-Middle / Data intercept / DNS spoofing)
                  │
IP Spoofing ──────┼──► Replay Attack (Delaying or repeating captured authentication traffic)
                  │
                  └──► Smurf Attack (Hybrid DoS / Broadcast amplification via ICMP)
```

#### 1. On-Path Attack (Meddler-in-the-Middle)

- The attacker positions themselves between two communicating endpoints (e.g., a web browser and a web server).
    
- Sniffs packets to discover communicating IP and MAC addresses, then impersonates either endpoint.
    
- **DNS Spoofing Risk**: Intercepts DNS queries and returns falsified IP records, redirecting users to malicious domains.
    

#### 2. Replay Attack

- The attacker intercepts valid network transmissions and intentionally delays or repeats them later.
    
- Used to desynchronize communication sessions or impersonate an authorized entity by replaying past authentication exchanges.
    

#### 3. Smurf Attack

- A hybrid attack combining **IP spoofing** and **DDoS amplification**.
    
- The attacker spoofs the victim's IP address and sends ICMP Echo requests to a network's **broadcast address**.
    
- Every active device on that network segment sends an ICMP Echo Reply back to the spoofed victim address simultaneously, exhausting victim bandwidth and processing resources.
    

## 4. Prevention & Defensive Measures

### Defense-in-Depth Approach

No single security control stops all network attacks; defenses must be layered across protocols, infrastructure, and endpoints.

- **Data-in-Transit Encryption**:
    
    - Implement **HTTPS (SSL/TLS)** and **VPNs**.
        
    - Even if packets are intercepted via active/passive sniffing, the payload remains unreadable to eavesdroppers.
        
- **Firewall Ingress Filtering**:
    
    - Configure firewall rules to reject incoming traffic from external interfaces that claim an internal/private source IP address.
        
    - Deploy **Next-Generation Firewalls (NGFW)** capable of anomaly detection to identify broadcast storms and Smurf attack signatures.
        
- **Network Hygiene**:
    
    - Avoid unencrypted, open public Wi-Fi networks unless an encrypted VPN tunnel is established before transmitting sensitive credentials.