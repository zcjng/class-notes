# Course 2 Module 3

a log is a record of events that ​occur within an organization's systems and networks.
It also includes outbound requests ​to the internet from within the network.

A firewall log is a record of attempted or ​established connections for ​incoming traffic from the internet

​A network log is a record of ​all computers and devices ​that enter and leave the network.
​It also records connections between ​devices and services on the network.

a server log is a record of ​events related to services such as websites, ​emails, or file shares. ​It includes actions such as login, ​password, and username requests.



SIEM, tool ​is an application that ​collects and analyzes log data to ​monitor critical activities 
in an organization.
SIEM tools can also be used to create dashboards.
SIEM dashboards also provide ​stakeholders with different metrics. 

​Metrics are key technical attributes ​such as response time, ​availability, and failure rate, ​which are used to assess ​the performance of a software application.


As cybersecurity continues to evolve, the need for cloud functionality has increased. SIEM tools have and continue to evolve to function in cloud-hosted and cloud-native environments. Cloud-hosted SIEM tools are operated by vendors who are responsible for maintaining and managing the infrastructure required to use the tools. Cloud-hosted tools are simply accessed through the internet and are an ideal solution for organizations that don’t want to invest in creating and maintaining their own infrastructure.

Consider the current development of interconnected devices with access to the internet, known as the Internet of Things (IoT). The more interconnected devices there are, the larger the cybersecurity attack surface and the amount of data that threat actors can exploit.

Similar to cloud-hosted SIEM tools, cloud-native SIEM tools are also fully maintained and managed by vendors and accessed through the internet. However, cloud-native tools are designed to take full advantage of cloud computing capabilities, such as availability, flexibility, and scalability.

**Security orchestration, automation, and response (SOAR)** is a collection of applications, tools, and workflows that uses automation to respond to security events.

TLDR: SIEM tools are gonna be further enchanced to improve automation for tasks and availability which means they are mostly cloud based.


- **Self-hosted SIEM tools:** These require organizations to install, operate, and maintain the tool using their own physical infrastructure. They are ideal for organizations needing to maintain physical control over confidential data.
- **Cloud-hosted SIEM tools:** These are maintained and managed by SIEM providers and are accessible via the internet. They are suitable for organizations that prefer not to invest in creating and maintaining their own infrastructure.

**Hybrid and Cloud-Native Solutions**

- **Hybrid SIEM solutions:** These combine both self-hosted and cloud-hosted SIEM tools, offering the benefits of the cloud while maintaining physical control over confidential data.
- **Cloud-native tools:** These are fully maintained and managed by the vendor and are specifically designed to leverage cloud computing capabilities like availability, flexibility, and scalability.

**Common SIEM Tools**
- **Splunk Enterprise:** A self-hosted data analysis platform that provides SIEM solutions for retaining, analyzing, and searching an organization's log data in real-time.
- **Splunk Cloud:** A cloud-hosted tool for collecting, searching, and monitoring log data, particularly useful for organizations with hybrid or cloud-only environments.
- **Chronicle:** Google's cloud-native tool designed for retaining, analyzing, and searching data, offering log monitoring, data analysis, and data collection.


---
## Open-source tools
often free to use and can be user friendly. The objective of open-source tools is to provide users with software that is built by the public in a collaborative way, which can result in the software being more secure. Additionally, open-source tools allow for more customization by users, resulting in a variety of new services built from the same open-source software package. 


**Suricata:** An open-source network analysis and threat detection software that inspects network traffic to identify suspicious behavior and generate logs.

## Proprietary tools
Proprietary tools are developed and owned by a person or company, and users typically pay a fee for usage and training. The owners of proprietary tools are the only ones who can access and modify the source code. Examples of proprietary tools include Splunk® and Google SecOps (Chronicle) SIEM tools.


# Course 2 Module 4
**Playbooks in Security**

- Playbooks help security teams address issues identified by Security Information and Event Management (SIEM) tools.
- They provide a structured approach to incident response.

Phases of an Incident Response Playbook

- **Preparation:** This initial phase involves documenting procedures, establishing staffing plans, and educating users to build a strong foundation for incident response.
- **Detection and Analysis:** In this phase, security analysts use tools and strategies to determine if a breach has occurred and assess its potential impact.
- **Containment:** The goal here is to prevent further damage and minimize the immediate effects of a security incident.
- **Eradication and Recovery:** This phase focuses on removing all traces of the incident and restoring affected systems to a secure state.
- **Post-incident activity** focuses on what happens _after_ the incident has been contained and eradicated. It's about learning from the event and improving future responses. This includes: (reflection)

	- Documenting the incident.
	- Informing organizational leadership.
	- Applying lessons learned to enhance overall security.
	- Conducting a full-scale analysis to determine the root cause and implement improvements.

- **Coordination**, on the other hand, is about how information is shared and reported _throughout_ the entire incident response process. It ensures that all relevant parties are kept informed and that the response is unified. This includes: (communication and collaboration during the entire incident response.)
	- Reporting incidents.
	- Sharing information based on established organizational standards.
	- Ensuring compliance requirements are met.
	- Facilitating a coordinated response and resolution.

Incident and Vulnerability Response Playbooks

- These playbooks are based on an organization's business continuity plan, ensuring operations continue despite disruptions.
- They include steps like preparation, detection, analysis, containment, eradication, and recovery, minimizing errors and ensuring timely actions.


# Course 3 Module 1
Understanding Networks
- A network is essentially a collection of connected devices.
- Examples include laptops, cell phones, smart devices at home, and workstations, printers, and servers in an office.

Device Communication

- Devices on a network communicate using either cables or wireless connections.
- They use unique identifiers, specifically IP and MAC addresses, to locate each other and ensure proper communication.

Types of Networks

- **Local Area Network (LAN):** Spans a small geographical area like a home or office, such as devices connected to your home Wi-Fi.
- **Wide Area Network (WAN):** Covers a large geographical area, like a city or country, with the internet being a prime example of a WAN
- 
Core Network Infrastructure

- **Firewalls**: Serve as the first line of defense by monitoring and restricting incoming and outgoing traffic based on organizational security rules.
- **Modems**: Connect a local network to an Internet Service Provider (ISP) by converting digital signals into formats compatible with the ISP's physical infrastructure. It usually has only **one outgoing port** because you get only one public IP address.
- **Routers**: Operate at the network layer to connect different networks and direct traffic using IP addresses; they can also include built-in firewall features.
- **Wireless Access Points (WAPs)**: Emit radio waves using Wi-Fi standards to allow wireless devices to connect to the network.

Traffic Direction & Network Models

- **Switches**: Wired The preferred choice for modern local networks; they analyze MAC addresses to forward data packets _only_ to the specific intended device, improving performance and security. The port multiplier. Connects multiple wired devices within the _same_ network so they can talk to each other.
- **Hubs**: Wired Outdated devices that blindly repeat all incoming data to every connected port, making them highly vulnerable to eavesdropping.
- **Client-Server Model**: An architecture where end-user devices (clients) send requests for information or services, and centralized systems (servers)—such as DNS, file, or mail servers—fulfill those requests.

Practical Security Application

- **Network Diagrams**: Crucial visual maps that security analysts and administrators use to study network architecture, identify potential structural vulnerabilities, and plan defense strategies.'

The box you have at home is a **3-in-1 combo device**. It contains:

- A **Router** brain to talk to the modem.
- A built-in **Switch** (the multiple Ethernet ports on the back) to connect wired devices.
- A **WAP** (the antennas) to emit radio waves for Wi-Fi.  
    This is why both standalone switches and your home router have Ethernet ports—your router just has a switch built right into it.

Router Operation Modes Summary
- **Wireless Router Mode**: Default setting. Acts as the main network boss, creates the network, assigns IPs, and handles firewall security.
- **Access Point (AP) Mode**: Turns off the router brain. Takes a wired cable from the main router and simply emits Wi-Fi (Ideal for your Floor 2 setup).
- **Repeater / Range Extender Mode**: Connects to Floor 1 Wi-Fi _wirelessly_ and rebroadcasts it (cuts internet speed in half).
- **Client / Media Bridge Mode**: Turns off Wi-Fi broadcasting. Acts purely as a wireless antenna receiver to feed internet into wired-only devices (like an old desktop PC).
- **WISP Mode**: Connects wirelessly to a public hotspot (like a hotel) but builds a private, safe firewall network inside your room.



Cloud Computing Explained
- Cloud computing utilizes remote servers, applications, and network services hosted on the internet, rather than local physical devices.
- This model allows companies to save money and access more network resources by outsourcing network management to third-party providers.

Cloud Networks vs. Traditional Networks
- Cloud networks store resources and data in remote data centers accessible via the internet, meaning companies don't house the servers physically.
- Traditional networks host web servers from a business's physical location, whereas cloud networks use remote servers for online services and web applications accessible from anywhere.

The Rise of Cloud Security
- As more organizations migrate to cloud services, cloud security is becoming increasingly relevant for security professionals.
- Cloud service providers offer on-demand storage, processing power, and analytics, leading to a focus on identity-based security in addition to traditional network-based solutions.

The Three Main Cloud Service Models

- **Software as a Service (SaaS)**: Ready-to-use software suites hosted and fully managed by the provider (CSPs) that companies access remotely without any setup.
- **Infrastructure as a Service (IaaS)**: On-demand raw virtual computing components like virtual servers, storage, and containers that companies configure and manage themselves.
- **Platform as a Service (PaaS)**: Pre-configured cloud environments and tools tailored specifically for application developers to build and deploy custom apps.

Cloud Environments & Virtual Networking

- **Hybrid Cloud**: Combining a company's own physical, on-premise hardware with third-party cloud provider services.
- **Multi-Cloud**: Using services from more than one distinct cloud provider (e.g., using both AWS and Google Cloud).
- **Software-Defined Networks (SDNs)**: Completely virtualized network infrastructure (like virtual switches, routers, and firewalls) hosted on the provider's physical data center servers and configured programmatically.

Core Business Benefits

- **Reliability**: Ensures high availability, consistent uptime, and secure remote access for users with minimal connection interruptions.
- **Cost Efficiency**: Eliminates massive upfront hardware expenses; companies outsource physical equipment maintenance, upgrades, and patching to the provider.
- **Scalability (Elasticity)**: Allows businesses to instantly scale up or scale down resources via a dashboard to meet changing demands, meaning they only pay for what they use.

Data Packet Structure and Function
- Data packets are the fundamental units of information transferred across a network, similar to a physical letter.
- Each packet contains a header with destination and source IP/MAC addresses, a protocol number, the message body, and a footer to signal completion.

Network Performance and Security
- Network performance is measured by bandwidth (data received per second) and speed (rate of packet reception).
- Irregularities in bandwidth or speed can indicate a network attack, making these metrics crucial for security personnel.

Packet Sniffing

- Packet sniffing involves capturing and inspecting data packets to monitor network communication.
- This practice is used to analyze network activity and identify potential security threats.

### TCP/IP Model
**TCP (Transmission Control Protocol)**

- Establishes connections between devices and organizes data for transmission.
- Ensures data packets reach their correct destination across a network.

**IP (Internet Protocol)**

- Provides standards for routing and addressing data packets.
- Includes IP addresses, which act as unique identifiers for devices on a network.

**Ports**

- Software-based locations within an operating system that organize data sending and receiving.
- Assign port numbers to data packets, allowing devices to prioritize and process network traffic.

## 4 Layers of TCP/IP
- **Network Access Layer:** This layer handles the creation of data packets and their transmission, involving hardware devices, physical cables, and switches.
- **Internet Layer:** IP addresses are attached to data packets here to specify sender and receiver locations, and this layer determines how networks connect.
	- **Internet Protocol (IP)**. IP sends the data packets to the correct destination and relies on the Transmission Control Protocol/User Datagram Protocol (TCP/UDP) to deliver them to the corresponding service. IP packets allow communication between two networks. They are routed from the sending network to the receiving network. TCP in particular retransmits any data that is lost or corrupt.
    
	- **Internet Control Message Protocol (ICMP)**. The ICMP shares error information and status updates of data packets. This is useful for detecting and troubleshooting network errors. The ICMP reports information about packets that were dropped or that disappeared in transit, issues with network connectivity, and packets redirected to other routers.

**Controlling Data Flow and Interaction**

- **Transport Layer:** Protocols in this layer manage the flow of network traffic, including error control to ensure smooth data transmission.
	- The **Transmission Control Protocol (TCP)** is an internet communication protocol that allows two devices to form a connection and stream data. It ensures that data is reliably transmitted to the destination service. TCP contains the port number of the intended destination service, which resides in the TCP header of a TCP/IP packet.
	- The **User Datagram Protocol (UDP)** is a connectionless protocol that does not establish a connection between devices before transmissions. It is used by applications that are not concerned with the reliability of the transmission. Data sent over UDP is not tracked as extensively as data sent using TCP. Because UDP does not establish network connections, it is used mostly for performance sensitive applications that operate in real time, such as video streaming.
- **Application Layer:** This layer defines how data packets interact with receiving devices, supporting functions like file transfers and email services.
	- Hypertext transfer protocol (HTTP)
	- Simple mail transfer protocol (SMTP)
	- Secure shell (SSH)
	- File transfer protocol (FTP)
	- Domain name system (DNS)
![[Pasted image 20261016085430.png]]

## OSI Model
The **OSI model** is a standardized concept that describes the seven layers computers use to communicate and send data over the network. Network and security professionals often use this model to communicate with each other about potential sources of problems or security threats when they occur.

![[Pasted image 20261016090022.png]]

- **Layer 7: Application Layer**: The user-facing interface. Uses protocols like **HTTP/HTTPS**, **SMTP**, and **DNS** to connect software applications to the internet.
- **Layer 6: Presentation Layer**: The translator. Handles data **formatting, encryption (like SSL), compression**, and character translation so different systems can understand each other.
- **Layer 5: Session Layer**: The manager. Opens, maintains, checkpoints, and terminates **communication sessions** between two devices.
- **Layer 4: Transport Layer**: The delivery service. Breaks large data into smaller pieces (**segmentation**), manages transfer speeds, and uses protocols like **TCP** and **UDP**.
- **Layer 3: Network Layer**: The traffic router. Moves data packets **between completely different networks** using **IP addresses** and routers.
- **Layer 2: Data Link Layer**: The local delivery. Organizes and moves data **within a single local network** using **MAC addresses**, network cards, and switches.
- **Layer 1: Physical Layer**: The raw hardware. Translates data into a stream of **0s and 1s (binary)** to transmit them over physical cables, wires, hubs, and modems.

---

The Core Difference: Network Layer (Layer 3) vs. Data Link Layer (Layer 2)

The fundamental difference comes down to **scope**: **Layer 2 stays local**, while **Layer 3 goes global**.

- **Data Link Layer (Layer 2)**:
    - **Scope**: Only handles communication **inside the exact same local network (LAN)**.
    - **Addressing**: Uses **MAC addresses** (the permanent physical hardware address of your device).
    - **Main Device**: **Switches**. A switch uses Layer 2 data to send a packet directly from your laptop to your home printer or file server. It cannot talk to the outside world.
- **Network Layer (Layer 3)**:
    - **Scope**: Handles communication **between multiple, separate networks** across the globe.
    - **Addressing**: Uses **IP addresses** (logical coordinates assigned to a network location).
    - **Main Device**: **Routers**. A router reads Layer 3 data to take a packet leaving your local home network and figure out how to navigate it across the internet to Google's external network.