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