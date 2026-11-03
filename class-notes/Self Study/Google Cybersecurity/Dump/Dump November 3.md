# Course 6 Module 1

### incident response lifecycle.

​Incident lifecycle frameworks provide a structure to support incident response ​operations. ​Frameworks help organizations develop a standardized approach to their incident ​response process, so ​that incidents are managed in an effective and consistent way. ​

NIST CST is one of the frameworks

 ​To recall, the five core functions of the NIST CSF are: identify, ​protect, detect, respond, and recover. ​
 
The NIST incident response lifecycle is another NIST framework with additional substeps ​dedicated to incident response. 

​It begins with preparation. 
Next, detection and analysis, and ​
then containment, eradication and recovery, and 
finally post-incident activity. 

This lifecycle gives us a blueprint of how to effectively respond to incidents,

an incident is "an occurrence that actually or imminently ​jeopardizes, without lawful authority, the confidentiality, integrity, or ​availability of information or an information system; ​or constitutes a violation or imminent threat of violation of law, ​security policies, security procedures, or acceptable use policies

An event is an observable occurrence on a network, system, or device.

Remember, all security incidents are events, but not all events are security incidents.

An incident investigation reveals critical information about the five W's ​of an incident: ​who triggered the incident, ​what happened, ​when the incident took place, ​where the incident took place, and why the incident occurred.


incident handler's journal, ​which is a form of documentation used in incident response. ​


​Computer security incident response teams, or CSIRTs, are a specialized group of ​security professionals that are trained in incident management and response.

The goal of CSIRTs are to effectively and efficiently manage incidents, ​provide services and resources for response and recovery, and ​prevent future incidents from occurring. ​Security is a shared responsibility, which is why CSIRTs must work cross functionally ​with other departments to share relevant information



​First, there's the security analyst. ​The analyst's job is to investigate security alerts to determine if an incident ​has occurred.

​But if the incident is highly critical, ​it gets escalated to the technical lead, who provides technical leadership by ​guiding security incidents through their lifecycle. ​

During this time, the incident coordinator tracks and manages the activities of ​the CSIRT and other teams involved in the response effort. ​Their job is to ensure that incident response processes are followed and ​that teams are regularly updated on the incident status.


​Not all CSIRTs are the same. ​Depending on the organization, a CSIRT can also be referred to as an Incident ​Handling Team, or IHT, or Security Incident Response Team, SIRT. ​


Understanding the composition of incident response teams


## **computer security incident response team (CSIRT)**
is a specialized group of security professionals that are trained in incident management and response. During incident response, to be effective and efficient, there must be clear command**,** control, and communication of the situation to achieve the desired goal. 

- **Command** refers to having the appropriate leadership and direction to oversee the response.
    
- **Control** refers to the ability to manage technical aspects during incident response, like coordinating resources and assigning tasks.
    
- **Communication** refers to the ability to keep stakeholders informed.
    

Establishing a CSIRT organizational structure with clear and distinctive roles aids in achieving an effective and efficient response.

## Roles in CSIRTs 
CSIRTs are organization dependent, so they can vary in their structure and operation. Structurally, they can exist as a separate, dedicated team or as a task force that meets when necessary. CSIRTs involve both nonsecurity and security professionals. Nonsecurity professionals are often consulted to offer their expertise on the incident. These professionals can be from external departments, such as human resources, public relations, management, IT, legal, and others. Security professionals involved in a CSIRT typically include three key security related roles: 

1. **Security analyst**
### **Security analyst**

The job of the **security** **analyst** is to continuously monitor an environment for any security threats. This includes: 

- Analyzing and triaging alerts
    
- Performing root-cause investigations
    
- Escalating or resolving alerts 
    

If a critical threat is identified, then analysts escalate it to the appropriate team lead, such as the technical lead.

1. **Technical lead**

The job of the technical lead is to manage all of the technical aspects of the incident response process, 
They do this by first determining the root cause of the incident. Then, they create and implement the strategies for 

containing, eradicating, and recovering from the incident. 

Technical leads often collaborate with other teams to ensure their incident response priorities align with business priorities, such as reducing disruptions for customers or returning to normal operations.

1. **Incident coordinator**

Responding to an incident also requires cross-collaboration with nonsecurity professionals. CSIRTs will often consult with and leverage the expertise of members from external departments. The job of the incident coordinator is to coordinate with the relevant departments during a security incident. 

## Security operations center

A **security operations center (SOC)** is an organizational unit dedicated to monitoring networks, systems, and devices for security threats or attacks. A SOC is involved in various types of blue team activities, such as network monitoring, analysis, and response to incidents.

## SOC organization

A SOC is composed of SOC analysts, SOC leads, and SOC managers. Each role has its own respective responsibilities. SOC analysts are grouped into three different tiers. 

![A triangle with four labeled tiers. From bottom to top: SOC Analyst L1, SOC Analyst L2, SOC Lead L3, and Manager.](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/CMDszNSgSryqbipKuGOU2Q_222b5865df5f4a72a785cd94279d99f1_CkqInM-0iDHKapM_2jS_68ZBhx11oR2GR_fCveMT5EI39E4h_dsAwBstgiRAXmj-mLFWXeOLNnSpBeMpDrEOiP9N8P9efbhWwLqNx-nDZFmoa6ue7SuqH0iUjmnMPWmpEcZPjKUY7ONnH6qkmt7wqF1XpuxPDe1zCbwGriZcyQRpSru_CG-tzvFvMDe5cg?expiry=1793752105907&hmac=iz_UwVjj5sHNS2kkXpKdDx4o9lAKl8CKM6lpSFp_AVs)

### **Tier 1 SOC analyst**

The first tier is composed of the least experienced SOC analysts who are known as level 1s (L1s). They are responsible for:

- Monitoring, reviewing, and prioritizing alerts based on criticality or severity
    
- Creating and closing alerts using ticketing systems
    
- Escalating alert tickets to Tier 2 or Tier 3


### **Tier 2 SOC analyst**

The second tier comprises the more experienced SOC analysts, or level 2s (L2s). They are responsible for: 

- Receiving escalated tickets from L1 and conducting deeper investigations
    
- Configuring and refining security tools
    
- Reporting to the SOC Lead
    

### **Tier 3 SOC lead**

The third tier of a SOC is composed of the SOC leads, or level 3s (L3s). These highly experienced professionals are responsible for:

- Managing the operations of their team
    
- Exploring methods of detection by performing advanced detection techniques, such as malware and forensics analysis
    
- Reporting to the SOC manager
    

### **SOC manager** 

The SOC manager is at the top of the pyramid and is responsible for: 

- Hiring, training, and evaluating the SOC team members
    
- Creating performance metrics and managing the performance of the SOC team
    
- Developing reports related to incidents, compliance, and auditing
    
- Communicating findings to stakeholders such as executive management   
    

### **Other roles**

SOCs can also contain other specialized roles such as: 

- **Forensic investigators**: Forensic investigators are commonly L2s and L3s who collect, preserve, and analyze digital evidence related to security incidents to determine what happened.
    
- **Threat hunters**: Threat hunters are typically L3s who work to detect, analyze, and defend against new and advanced cybersecurity threats using threat intelligence.
    

**Note**: Just like CSIRTs, the organizational structure of a SOC can differ depending on the organization.



security plans consist of three basic elements: ​policies, standards, and procedures.


​An incident response plan is a document that outlines the procedures to take in ​each step of incident response.

there are common elements that they share. ​Incident plans have: Incident response procedures. ​These are step-by-step instructions on how to respond to incidents. 

​System information. These are things like network diagrams, ​data flow diagrams, logging, and asset inventory information. ​And other documents like contact lists, forms, and templates.



Documentation is any form of recorded content that is used for a specific purpose. This can be audio, digital, or handwritten instructions, and even videos. There is no set industry standard for documentation, so many organizations set their own documentation practices. 

Regardless, documentation is meant to provide instruction and guidance on a specific topic. There are also many types of documentation, and you may already be familiar with some of them from the previous lessons. These include playbooks, incident handler's journals, policies, plans, and final reports. 



Intrusion detection systems
Those sensors work by sending out sound waves, and when an object touches a sound wave, the waves bounce back to your sensor and trigger an alert to your phone, notifying you that an intrusion was detected. An intrusion detection system, or IDS, works in a very similar way to home intrusion sensors. An intrusion detection system is an application that monitors system and network activity, and produces alerts on possible intrusions. 


Ips

When the sensor detects that the window's glass has been shattered, it triggers a steel roll-up door to automatically replace the shattered window and prevent unauthorized entry into the store. This is what an intrusion prevention system does. Intrusion prevention systems, or IPS, have all the same capabilities as an IDS, but they can do more. 

They monitor system activity for intrusions and takes action to stop it. Many tools have the ability to perform the function of both IDS and IPS. Some popular tools are Snort, Zeek, Kismet, Sagan, and Suricata. 


Capability	IDS	IPS	EDR
Detects malicious activity	✓	✓	✓
Prevents intrusions	N/A	✓	✓
Logs activity	✓	✓	✓
Generates alerts	✓	✓	✓
Performs behavioral analysis	N/A	N/A	✓
Overview of IDS tools
An intrusion detection system (IDS) is an application that monitors system activity and alerts on possible intrusions. An IDS provides continuous monitoring of network events to help protect against security threats or attacks. The goal of an IDS is to detect potential malicious activity and generate an alert once such activity is detected. An IDS does not stop or prevent the activity. Instead, security professionals will investigate the alert and act to stop it, if necessary. 
For example, an IDS can send out an alert when it identifies a suspicious user login, such as an unknown IP address logging into an application or a device at an unusual time. But, an IDS will not stop or prevent any further actions, like blocking the suspicious user login. 
Examples of IDS tools include Zeek, Suricata, Snort®, and Sagan. 
Detection categories
As a security analyst, you will investigate alerts that an IDS generates. There are four types of detection categories you should be familiar with:
	1.	A true positive is an alert that correctly detects the presence of an attack.
	2.	A true negative is a state where there is no detection of malicious activity. This is when no malicious activity exists and no alert is triggered. 
	3.	A false positive is an alert that incorrectly detects the presence of a threat. This is when an IDS identifies an activity as malicious, but it isn't. False positives are an inconvenience for security teams because they spend time and resources investigating an illegitimate alert. 
	4.	A false negative is a state where the presence of a threat is not detected. This is when malicious activity happens but an IDS fails to detect it. False negatives are dangerous because security teams are left unaware of legitimate attacks that they can be vulnerable to. 
Overview of IPS tools
An intrusion prevention system (IPS) is an application that monitors system activity for intrusive activity and takes action to stop the activity. An IPS works similarly to an IDS. But, IPS monitors system activity to detect and alert on intrusions, and it also takes action to prevent the activity and minimize its effects. For example, an IPS can send an alert and modify an access control list on a router to block specific traffic on a server.
Note: Many IDS tools can also operate as an IPS. Tools like Suricata, Snort, and Sagan have both IDS and IPS capabilities.
Overview of EDR tools  
Endpoint detection and response (EDR) is an application that monitors an endpoint for malicious activity. EDR tools are installed on endpoints. Remember that an endpoint is any device connected on a network. Examples include end-user devices, like computers, phones, tablets, and more.
EDR tools monitor, record, and analyze endpoint system activity to identify, alert, and respond to suspicious activity. Unlike IDS or IPS tools, EDRs collect endpoint activity data and perform behavioral analysis to identify threat patterns happening on an endpoint. Behavioral analysis uses the power of machine learning and artificial intelligence to analyze system behavior to identify malicious or unusual activity. EDR tools also use automation to stop attacks without the manual intervention of security professionals. For example, if an EDR detects an unusual process starting up on a user’s workstation that normally is not used, it can automatically block the process from running.
Tools like Open EDR®, Bitdefender™ Endpoint Detection and Response, and FortiEDR™ are examples of EDR tools.
Note: Security information and event management (SIEM) tools also have detection capabilities, which you'll explore later.


This lecture explains how security incident management tools—specifically SIEM and SOAR—help security analysts monitor network activity, detect potential threats, and manage responses effectively.

SIEM Fundamentals and Dashboard Role
- SIEM (Security Information and Event Management) collects and analyzes log data across thousands of network systems to provide a high-level, real-time overview of critical activities.
- Much like a car's dashboard warning lights that notify drivers of internal issues, a SIEM consolidates complex data flows into a centralized platform to alert analysts to potential threats.

The SIEM Data Process
- Collection and Aggregation: Logs from various sources like firewalls, databases, applications, and IDS/IPS are gathered and centralized into one place.
- Normalization and Analysis: Raw data is cleaned to remove non-essential attributes and standardized for consistency, then evaluated against configured rule sets to trigger security alerts.

Automating with SOAR
- SOAR (Security Orchestration, Automation, and Response) complements SIEM by using automated workflows and applications to respond to security events rather than just reporting them.
- SOAR also tracks and manages cases by centralizing multiple related incidents into a single workflow for efficient handling.


SIEM PROCESS


Collect and aggregate data
SIEM tools require data for them to be effectively used. During the first step, the SIEM collects event data from various sources like firewalls, servers, routers, and more. This data, also known as logs, contains event details like timestamps, IP addresses, and more. Logs are a record of events that occur within an organization’s systems. After all of this log data is collected, it gets aggregated in one location. Aggregation refers to the process of consolidating log data into a centralized place. Through collection and aggregation, SIEM tools eliminate the need for manually reviewing and analyzing event data by accessing individual data sources. Instead, all event data is accessible in one location—the SIEM. 
￼
Parsing can occur during the first step of the SIEM process when data is collected and aggregated. Parsing maps data according to their fields and their corresponding values. For example, the following log example contains fields with values. At first, it might be difficult to interpret information from this log based on its format:
April 3 11:01:21 server sshd[1088]: Failed password for user nuhara from 218.124.14.105 port 5023
In a parsed format, the fields and values are extracted and paired making them easier to read and interpret:
	•	host = server
	•	process = sshd
	•	source_user = nuhara
	•	source ip = 218.124.14.105
	•	source port = 5023
Normalize data
SIEM tools collect data from many different sources. This data must be transformed into a single format so that it can be easily processed by the SIEM. However, each data source is different and data can be formatted in many different ways. For example, a firewall log can be formatted differently than a server log.
￼
Collected event data should go through the process of normalization. Normalization converts data into a standard, structured format that is easily searchable. 
Analyze data
After log data has been collected, aggregated, and normalized, the SIEM must do something useful with all of the data to enable security teams to investigate threats. During this final step in the process, SIEM tools analyze the data. Analysis can be done with some type of detection logic such as a set of rules and conditions. SIEM tools then apply these rules to the data, and if any of the log activity matches a rule, alerts are sent out to cybersecurity teams.
Note: A part of the analysis process includes correlation. Correlation involves the comparison of multiple log events to identify common patterns that indicate potential security threats.
SIEM tools 
There are many SIEM tools. The following are some SIEM tools commonly used in the cybersecurity industry:
	•	AlienVault® OSSIM™
	•	Chronicle
	•	Elastic
	•	Exabeam
	•	IBM QRadar® Security Intelligence Platform
	•	LogRhythm
	•	Splunk

# Course 5 Module 2

This lecture introduces network traffic analysis and explains how security professionals monitor traffic flow to identify potential malicious activity.

Understanding Network Traffic
- Network traffic is the amount of data moving across a network, while network data refers to the data transmitted between devices.
- Large organizations generate high volumes of traffic, requiring security analysts to understand normal flow patterns to recognize deviations.

Detecting Abnormalities and Indicators of Compromise
- Similar to road traffic, knowing expected patterns helps security professionals quickly identify unexpected, abnormal traffic flow.
- Abnormalities can reveal indicators of compromise (IoCs), which provide observable evidence of a potential security incident.

Recognizing Data Exfiltration
- Data exfiltration involves the unauthorized transfer of data, such as credentials or intellectual property, out of a system.
- Observing unusual spikes in outbound network traffic from a host can signal potential exfiltration that requires investigation.




🌐 Network Monitoring & Protection
1. 📊 Baselines: Establishing the "Normal"
A baseline is a standard reference point representing expected, legitimate behavior across systems, devices, and traffic flows.
￼
 Purpose: Enables anomaly detection by highlighting deviations.
￼
 Data Points Analyzed:
￼
 Source & destination IP addresses
￼
 Data transfer volume
￼
 Timestamps and transmission frequency
￼
 Standard port/protocol combinations
2. 🔍 Network Monitoring Vectors
Once a baseline is established, network components are monitored across three primary dimensions:
📡 A. Flow Analysis
Examines packet volume, protocols, and port numbers.
￼
 Protocol-to-Port Alignment: Verifies that protocols use expected ports (e.g., standard HTTPS on port ⁠443⁠).
￼
 Command and Control (C2) Detection: Attackers often mask malicious traffic on unexpected ports (e.g., tunneling HTTPS over port ⁠8088⁠) to communicate with compromised internal hosts.
📦 B. Packet Payload Information
Examines the actual message body/data transported within a packet.
￼
 Data Inspection: Inspects content (decrypting where legally/technically feasible).
￼
 Exfiltration Detection: Spots sensitive files or database dumps moving outbound to external addresses.
⏱️ C. Temporal Patterns
Monitors traffic frequency relative to time windows.
￼
 Expected Window: Standard operating hours (e.g., 9:00 AM – 5:00 PM local time).
￼
 Deviation: Unscheduled off-hours volume spikes (e.g., 2:00 AM transfer surge) signaling unauthorized access, automated scripts, or ongoing breaches.
3. 🛡️ SOC vs. NOC Comparison
Organizations often divide operational oversight between two specialized units:
Dimension	Security Operations Center (SOC)	Network Operations Center (NOC)
Primary Mission	Security, threat detection & incident response	Network stability, performance & availability
Key Focus	Indicators of Compromise (IoCs), intrusion	Network outages, latency, packet loss, uptime
Mindset	Adversarial / Defensive	Operational / Reliability
4. 🧰 Network Monitoring Toolset
🚨 Intrusion Detection Systems (IDS)
Automated alert generators.
￼
 Continuously scans traffic and matches against predefined signatures or baseline deviations.
￼
 Flags malicious patterns such as malware signatures, exploit payloads, and phishing communication.
🔬 Network Protocol Analyzers (Packet Sniffers)
Deep packet inspection and forensics.
￼
 Common Tools: ⁠tcpdump⁠ (CLI), ⁠Wireshark⁠ (GUI).
￼
 Function: Captures raw network traffic (⁠.pcap⁠) for manual analysis, frame dissection, and root-cause reconstruction.
📌 Summary Checklist
￼
 [ ] Define the Baseline: Identify normal traffic volumes, operational hours, and authorized ports.
￼
 [ ] Inspect Mismatches: Flag protocols running over non-standard ports (potential C2 channels).
￼
 [ ] Audit Off-Hours Traffic: Investigate temporal anomalies outside standard business operations.
￼
 [ ] Layer Tools: Combine automated alerts (IDS) with granular packet capture inspection (⁠pcap⁠ via Wireshark/tcpdump).



Monitoring network traffic is essential for detecting, preventing, and responding to data exfiltration attacks by understanding both attacker tactics and defensive measures.

The Attacker's Perspective
- Attackers gain initial access through tactics like phishing, then use lateral movement (pivoting) to explore the network and find valuable assets.
- Target data is identified, compressed or packaged to bypass controls, and exfiltrated using methods such as compromised email accounts.

Preventing and Securing Access
- Organizations defend against entry by enforcing security controls like multi-factor authentication (MFA) and maintaining comprehensive asset inventories.
- Security teams actively monitor network activity for early warning signs, such as multiple external logins from unfamiliar IP addresses.

Detecting and Responding to Exfiltration
- Network monitoring and SIEM tools detect unusual data collection indicators, including large internal transfers, external uploads, and unexpected file writes.
- Security teams investigate generated alerts and stop the attack by taking containment actions, such as blocking malicious IP addresses with firewall rules.