2C6M2

Here is a cleaned-up, well-structured version of your study notes. Redundancies have been merged, tone made consistent, and technical concepts organized logically.

Network Fundamentals & Packet Analysis

1. Anatomy of a Network Packet

Data traveling across a network is broken down into units called packets. Each packet typically consists of three primary sections:

- Header: Metadata used to route and process the packet. Multiple headers are often layered together depending on the protocol stack (e.g., Ethernet, IP, TCP/UDP). Common fields include source and destination IP addresses, ports, sequence numbers, and packet length.
- Payload: The actual user data being transported (e.g., the contents of an image file being uploaded to a web server).
- Footer (Trailer): Located at the very end of the frame. Used primarily at the Data Link layer (e.g., Ethernet's Frame Check Sequence) for error checking to ensure the data was not corrupted during transit.Note: Most Layer 3 and higher protocols (such as IP) do not use footers.  
    

2. Key IPv4 Header Fields

Operating at the Internet layer of the TCP/IP model, the Internet Protocol (IP) acts like a courier service, routing and delivering packets between hosts. Security analysts examine IPv4 headers to evaluate traffic behavior:

|   |   |   |
|---|---|---|
|Category|Field|Purpose|
|Sizing & Version|Version|Specifies the IP version used (IPv4 vs. IPv6).|
||Internet Header Length (IHL)|Indicates the length of the header itself (accounting for optional fields).|
||Total Length|The full size of the entire packet (header + payload) in bytes.|
|Addressing|Source Address|The IP address of the sender.|
||Destination Address|The IP address of the intended recipient.|
|Delivery & Routing|Type of Service (ToS / DiffServ)|Indicates packet priority and quality of service (QoS) requirements.|
||Time to Live (TTL)|A hop counter decremented by each router to prevent packets from looping endlessly; the packet is dropped when TTL reaches 0.|
||Protocol|Identifies the next-level protocol in the payload (e.g., ICMP, TCP, UDP).|
|Fragmentation|Identification|Unique ID used to group fragments of the same original packet.|
||Flags|Controls or identifies fragments (e.g., Don't Fragment, More Fragments).|
||Fragment Offset|Specifies where in the overall datagram this fragment belongs.|
|Integrity|Header Checksum|Verifies the integrity of the header; dropped if errors are detected.|

3. Capturing Network Traffic

How Protocol Analyzers Work

Network protocol analyzers (packet sniffers) capture raw traffic and decode it into a human-readable format:

1. Ingestion via Network Interface Card (NIC): By default, a NIC only processes frames addressed directly to it or to broadcast/multicast groups. To capture all local traffic, the interface must be set to a capture mode:

- Promiscuous Mode: Captures all wired frames visible on the network segment.
- Monitor Mode: Captures wireless frames without associating with an access point.
- Placement: Analyzers must be positioned on the correct segment, switch port (e.g., via SPAN/port mirroring), or network tap to see target traffic.

3. Decoding: The analyzer receives the raw binary stream (1s and 0s) and parses the protocols into structured fields for analysts to read, filter, and inspect.

Caution & Legality: Enabling promiscuous mode on shared networks captures sensitive credentials and confidential data. Unauthorized packet interception is illegal in most jurisdictions.

4. Packet Capture (PCAP) Libraries & Formats

A packet capture file stores raw network traffic for offline inspection. Common libraries and file extensions include:

- Libpcap: The standard portable C/C++ capture library for Unix-like systems (Linux, macOS). Used natively by tools like tcpdump.
- WinPcap: An older Windows port of Libpcap. Mostly deprecated and rarely used in modern environments.
- Npcap: The modern Windows packet capture architecture developed by the Nmap project. It supports loopback captures, raw 802.11 frame captures, and updated security controls.
- PCAPng (Next Generation): A modern capture file format capable of storing packets alongside metadata, interface statistics, decryption secrets, and comments within the same capture file.

5. Security Incident Investigation

For security analysts, PCAP files serve as an evidentiary source to:

- Detect Indicators of Compromise (IoCs): Identify unencrypted credentials, malicious command-and-control (C2) traffic, anomalous beacons, and port scans.
- Reconstruct Event Timelines: Walk step-by-step through an attack chain to establish exactly what an attacker accessed or exfiltrated.
- Filter Targeted Artifacts: Narrow down millions of frames to specific communications using filter expressions (e.g., filtering by IP, port, or protocol).



**Internet Protocol (IP)** includes a set of standards used for routing and addressing data packets as they travel between devices on a network.

### **IPv4**

- **Version**: This field indicates the IP version. For an IPv4 header, IPv4 is used. 
    
- **Internet Header Length (IHL)**: This field specifies the length of the IPv4 header including any Options.
    
- **Type of Service (ToS)**: This field provides information about packet priority for delivery.
    
- **Total Length**: This field specifies the total length of the entire IP packet including the header and the data.
    
- **Identification**: Packets that are too large to send are fragmented into smaller pieces. This field specifies a unique identifier for fragments of an original IP packet so that they can be reassembled once they reach their destination.
    
- **Flags**: This field provides information about packet fragmentation including whether the original packet has been fragmented and if there are more fragments in transit.
    
- **Fragment Offset**: This field is used to identify the correct sequence of fragments.
    
- **Time to Live (TTL)**: This field limits how long a packet can be circulated in a network, preventing packets from being forwarded by routers indefinitely.
    
- **Protocol**: This field specifies the protocol used for the data portion of the packet.
    
- **Header Checksum**: This field specifies a checksum value which is used for error-checking the header.
    
- **Source Address**: This field specifies the source address of the sender.
    
- **Destination Address**: This field specifies the destination address of the receiver.
    
- **Options**: This field is optional and can be used to apply security options to a packet.

### **IPv6**

IPv6 adoption has been increasing because of its large address space. There are eight fields in the header:

- **Version**: This field indicates the IP version. For an IPv6 header, IPv6 is used.
    
- **Traffic Class**: This field is similar to the IPv4 Type of Service field. The Traffic Class field provides information about the packet's priority or class to help with packet delivery.
    
- **Flow Label**: This field identifies the packets of a flow. A flow is the sequence of packets sent from a specific source. 
    
- **Payload Length**: This field specifies the length of the data portion of the packet.
    
- **Next Header**: This field indicates the type of header that follows the IPv6 header such as TCP.
    
- **Hop Limit**: This field is similar to the IPv4 Time to Live field. The Hop Limit limits how long a packet can travel in a network before being discarded.
    
- **Source Address**: This field specifies the source address of the sender.
    
- **Destination Address**: This field specifies the destination address of the receiver.

## Wireshark

**Wireshark** is an open-source network protocol analyzer. It uses a graphical user interface (GUI), which makes it easier to visualize network communications for packet analysis purposes. Wireshark has many features to explore that are beyond the scope of this course. You'll focus on how to use basic filtering to isolate network packets so that you can find what you need.

![Wireshark's interface.](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/SrU-DA-pR1eTtgJe4lBOjQ_c841a1d648aa40069fe7e47e84ea79f1_CS_R-125_Wireshark-homepage.png?expiry=1793924164772&hmac=wDPVZfVcIgH1pe2Ht8Z_QPecvRo29Rq9ULDg3fQ-iCM)

### Display filters

Wireshark's display filters let you apply filters to packet capture files. You can filter packets based on information such as protocols, IP addresses, ports, and virtually any other property found in a packet. Here, you'll focus on display filtering syntax and filtering for protocols, IP addresses, and ports.

### Comparison operators

You can use different comparison operators to locate specific header fields and values. Comparison operators can be expressed using either abbreviations or symbols. For example, this filter using the == equal symbol in this filter ip.src == 8.8.8.8 is identical to using the eq abbreviation in this filter ip.src eq 8.8.8.8.


|**Operator type**|**Symbol**|**Abbreviation**|
|---|---|---|
|Equal|==|eq|
|Not equal|!=|ne|
|Greater than|>|gt|
|Less than|<|lt|
|Greater than or equal to|>=|ge|
|Less than or equal to|<=|le|

You can combine comparison operators with Boolean logical operators like and and or

### Contains operator

The contains operator is used to filter packets that contain an exact match of a string of text. 

### Matches operator

The matches operator is used to filter packets based on the regular expression (regex) that's specified.

### **Filter for protocols**

Protocol filtering is one of the simplest ways you can use display filters. You can simply enter the name of the protocol to filter. For example, to filter for DNS packets simply type dns in the filter toolbar. Here is a list of some protocols you can filter for:

- dns
    
- http
    
- ftp
    
- ssh
    
- arp
    
- telnet
    
- icmp