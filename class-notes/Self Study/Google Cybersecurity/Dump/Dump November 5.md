# Packet captures with tcpdump

Overview of tcpdump

- tcpdump is a command-line packet sniffer pre-installed on many Linux distributions and available for Unix-like systems such as macOS.
- It allows users to filter network traffic by IP address, protocol, or port number without using a graphical user interface.

Basic Command Syntax

- Elevated privileges are often required, typically using `sudo` to run the tool.
- Common flags include `-i` to specify the network interface, `-v` for verbose output, and `-c` to set the count of packets to capture.

Packet Header Details

- Key packet data includes timestamps for timelines, the IP version, Time to Live (TTL), fragmentation flags, and the protocol type.
- The output displays source and destination IP addresses, ports, checksum verification, and TCP flags like PUSH and ACK.

# Overview of tcpdump

**What is tcpdump and Basic Syntax**

- Runs via command line with elevated root or `sudo` privileges to sniff live traffic and save it to packet capture (`.pcap`) files.
- Uses the standard syntax `sudo tcpdump [-i interface] [option(s)] [expression(s)]`, requiring the `-i` flag to specify the target network interface.

**Essential Command Options**

- The `-w` flag writes captured packets directly to a `.pcap` file, while `-r` reads and displays saved capture files.
- Flags like `-v` (verbosity), `-c` (packet count limit), and `-n` / `-nn` (disabling automatic name and port resolution to prevent unintended reverse DNS lookups) help customize the output.

**Filtering and Output Interpretation**

- Filter expressions isolate traffic by protocol (such as `ip6`) or combine targets using boolean operators like `and`, `or`, and `not`.
- The captured text output displays key packet details, including the timestamp, source and destination IP addresses, source and destination port numbers, and TCP connection parameters.

# Course 6 M 3

# The detection and analysis phase of the lifecycle 

Detection Phase

- Detection tools like IDS and SIEM gather and analyze data from regular business events to identify unusual or unauthorized activity.
- While all incidents are events, not all events are incidents; an alert is generated only when an incident is suspected.

Analysis Phase

- Analysts use critical thinking and investigate indicators of compromise to validate incoming alerts.
- This verification process determines whether unauthorized activity, such as account compromise, has truly taken place.

Challenges in Detection and Analysis

- Tools cannot catch everything due to inherent limitations and incomplete deployment across an organization.
- Analysts often face high alert volumes, frequently caused by misconfigured rules creating false positives or genuine spikes from newly exploited vulnerabilities.

# Cybersecurity incident detection methods

Threat Hunting

- Threat hunting is a proactive, human-driven search across a network to discover malicious activity and advanced threats that automated detection tools miss.
- It helps uncover evasive threats like fileless malware by combining active human analysis, threat intelligence, indicators of compromise, and machine learning.

Threat Intelligence

- Threat intelligence provides evidence-based context regarding existing or emerging attacker tactics, techniques, and procedures (TTPs) from sources like industry reports, government advisories, and data feeds.
- Threat Intelligence Platforms (TIPs) help organizations aggregate, centralize, and analyze large volumes of threat data to prioritize risks and add context to detections.

Cyber Deception

- Cyber deception uses deliberate decoy strategies to mislead malicious actors, increase detection rates, and study attacker behavior.
- Honeypots serve as decoy systems or files designed to look legitimate and vulnerable, alerting security teams the moment an unauthorized actor attempts access.

# Ongoing Monitoring of CI/CD

Common CI/CD Indicators of Compromise (IoCs)

- Unauthorized or suspicious code changes, such as unexpected commits, late-night modifications, or unreviewed large deletions.
- Anomalous deployment patterns, unexpected pipeline execution failures, compromised dependencies, and attempts to expose hardcoded secrets.

Logging and Auditing Methods

- Execution and code commit logs establish normal operational baselines to detect deviations like unexpected step order or unusual build durations.
- Access and deployment logs track user activity and release behaviors to spot unapproved account actions or strange login locations.

Automated Threat Detection and Integration

- SIEM platforms analyze pipeline data with machine learning and rule-based alerts for known IoCs like malicious file hashes or C2 server connections.
- Continuous vulnerability scanning and real-time alerts ensure security teams are immediately notified about secret exposure attempts and exploitable CVEs.

# Indicators of compromise

Indicators of Compromise vs. Indicators of Attack

- Indicators of compromise (IoCs) represent observable forensic evidence (such as file hashes or IP addresses) identifying the "who" and "what" of an attack that has already occurred.
- Indicators of attack (IoAs) focus on real-time behavioral evidence (such as an unknown process opening a network connection) to uncover the "why" and "how" of an ongoing attack.

The Concept of the Pyramid of Pain

- Developed by David J. Bianco, the model maps indicators of compromise to the level of difficulty ("pain") an adversary faces when security defenders block those specific indicators.
- Blocking items at the base requires little effort for an attacker to overcome, whereas blocking indicators toward the peak makes continuing the attack significantly harder.

Levels of the Pyramid of Pain

- The lower and middle levels cover technical indicators: hash values (easiest to change), IP addresses, domain names, network artifacts, and host artifacts.
- The highest levels include tools (software used by attackers) and tactics, techniques, and procedures (TTPs), which represent behavioral patterns that are the hardest to detect but inflict the most disruption when blocked.


# Analyze indicators of compromise with investigative tools

Context and Threat Intelligence

- Relying on single IoCs like isolated IP addresses misses the broader picture, as malicious actors can easily adapt and evade detection.
- Threat intelligence provides evidence-based context that helps analysts connect related artifacts and build a narrative to inform response actions.

Crowdsourcing and Open-Source Intelligence (OSINT)

- Crowdsourcing enables the global security community, including organizations like ISACs, to share threat data and prevent attackers from reusing the same tactics across multiple targets.
- OSINT involves analyzing publicly available information to generate actionable intelligence that improves detection tools and anti-virus capabilities.

Investigative Tools and VirusTotal

- VirusTotal enables analysts to inspect suspicious files, URLs, and IPs through vendor detection verdicts, static details, related artifacts, sandboxed behavioral data, and community feedback.
- Other public investigative tools include Jotti's malware scan, Urlscan.io for URL analysis, and MalwareBazaar for malware samples.

# The benefits of documentation 

This video highlights the core benefits of documentation in cybersecurity operations, emphasizing how it enables teams to scale, maintain consistency, and respond effectively to incidents.

Operational Scalability and Clarity

- Documenting rule activations, severity levels, and false positives allows security operations to scale beyond just a few analysts.
- Playbooks and clear instructions remove ambiguity, helping team members understand their roles and execute workflows efficiently during an incident.

Standardization and Quality Control

- Documentation establishes policies, processes, and standard procedures that guide members of an organization through tasks.
- Setting explicit rules and guidelines ensures a consistent quality of work across all security operations.

Transparency and Continuous Maintenance

- Creating records of events provides transparency and serves as vital evidence for insurance claims, regulatory investigations, and legal matters.
- Because threats and regulations continually evolve, documentation must be regularly reviewed, updated, and maintained to address new security gaps.

# Document evidence with chain of custody forms


Understanding Chain of Custody

- Chain of custody is the formal process of documenting evidence possession and control across an incident’s lifecycle.
- It creates a transparent paper trail detailing who handled the evidence, as well as why, when, and where it was transferred.

Key Elements and Handling Procedures

- Before handling digital media, evidence is write-protected and verified with a cryptographic hash function to detect any future tampering.
- Standard forms document the evidence description (such as IP or MAC addresses) alongside a log of transfer details, dates, and handling personnel.

Legal Importance and Broken Custody

- A broken chain of custody occurs when evidence is logged inconsistently or contains missing entries.
- Maintaining an unbroken record is essential to prove the evidence's integrity and reliability so it can be admitted in legal proceedings against threat actors.

# Best practices for effective documentation

Core Benefits of Documentation

- Provides transparency to fulfill compliance, insurance, and legal needs, such as maintaining a chain of custody for evidence.
- Delivers standardization to ensure consistent incident response processes, knowledge transfer, and clarity regarding why specific actions were taken.

Best Practices for Effective Documentation

- Tailor the depth and technical language of the content to match the specific needs and expertise of the target audience.
- Keep documents concise by establishing the purpose upfront, allowing readers to quickly identify key findings.

Maintenance and Continuous Improvement

- Review and update documentation regularly to address the continuously evolving threat landscape and newly discovered vulnerabilities.
- Conduct post-incident reviews to identify procedural gaps and ensure incident response plans remain current.

# The value of cybersecurity playbooks


Purpose and Benefits of Playbooks

- Provide clear, step-by-step instructions and checklists to reduce guesswork, uncertainty, and hesitation during an incident.
- Bring structure and order to chaotic situations, ensuring security teams act swiftly and effectively during attacks like DDoS, ransomware, or malware.

Types of Playbooks

- Non-automated playbooks require security analysts to perform all steps manually, such as detecting indicators of compromise and gathering logs.
- Automated and semi-automated playbooks leverage tools like SIEM and SOAR to automate repetitive or time-consuming tasks, helping to lower resolution times and increase productivity.

Maintenance and Updates

- Playbooks must be maintained and regularly updated to remain effective against continuously evolving security threats.
- The post-incident activity phase serves as an ideal time to review incidents and introduce necessary updates to playbooks.

# The role of triage in incident response

Concept of Security Triage

- Triage in cybersecurity functions like medical triage in emergency rooms, helping teams allocate limited resources to the most critical threats first.
- Incidents are prioritized based on their potential threat to system confidentiality, integrity, and availability (e.g., ransomware takes precedence over a single phishing email).

The Triage Workflow

- When an alert triggers, analysts first verify whether it is a false positive or related to an ongoing incident.
- If verified as a true positive, the alert is assigned a priority level according to organizational guidelines to dictate the response actions.

Contextual Investigation

- Analysts collect and analyze supporting evidence, such as system logs, to make well-informed decisions.
- Adding context through critical questions (e.g., checking for multiple failed attempts or unusual login times/locations) prevents inaccurate assumptions.

# The triage process

The Triage Steps

- **Receive and assess**: Analysts verify whether incoming alerts (such as from an IDS) are genuine threats or false positives by reviewing alert details, history, and known vulnerabilities.
- **Assign priority**: Valid alerts are ranked by urgency and impact, taking into account functional impact on systems, information impact on data, and the recoverability of the affected assets.

Investigation and Escalation

- **Collect and analyze**: Analysts perform comprehensive analysis by gathering multi-source evidence, conducting external research, and documenting the process.
- **Escalation**: When high-severity or complex issues arise, analysts escalate the incident to level-two analysts or managers with advanced response capabilities.

Benefits to the Security Team

- **Resource management**: Prioritizing alerts prevents teams from spending critical time on low-priority issues and shortens response times for severe threats.
- **Standardized approach**: Utilizing standard playbooks and iterative processes ensures consistent incident handling and that only verified alerts undergo deep investigation.

# The containment, eradication, and recovery phase of the lifecycle


Containment

- Limits and prevents further damage by executing predefined incident response plans.
- Often involves isolating affected devices, such as disconnecting a compromised computer from the network to stop malware from spreading.

Eradication

- Involves completely removing all components and traces of the incident from affected systems.
- Includes actions such as running vulnerability tests and patching identified security flaws.

Recovery

- Focuses on restoring impacted systems, services, and operations back to normal business functions.
- Involves tasks such as reimaging systems, resetting passwords, and updating network configurations like firewall rules.

# Business continuity considerations

Business Continuity Planning (BCP)

- A BCP documents procedures to maintain or quickly restore essential operations during significant disruptions, minimizing legal, financial, and reputational harm.
- It differs from a disaster recovery plan, which focuses specifically on restoring information systems following major technical failures or natural disasters.

Impacts on Critical Operations

- Severe security incidents, like ransomware attacks, can encrypt critical records and undermine operations across vital infrastructure, such as healthcare systems.
- BCPs reduce service interruptions to safeguard essential organizational functions, public safety, and national or economic security.

Recovery Strategies and Site Resilience

- Organizations use site resilience to ensure continuous infrastructure availability through alternate operational locations.
- Recovery options include hot sites (fully operational duplicates ready immediately), warm sites (configured environments that can be quickly prepared), and cold sites (backup facilities requiring additional setup to function).

# The post-incident activity phase of the lifecycle

Post-Incident Activity and Documentation

- The post-incident phase centers on reviewing the handling of an incident to identify areas for improvement and minimize the risk of recurrence.
- A final report is created to provide a comprehensive review, including an event timeline, incident details, and recommendations for future prevention.

Lessons Learned Meetings

- Held typically within two weeks of an incident, this meeting brings together all involved parties to discuss what happened, evaluate actions taken, and reference the final report.
- The discussion focuses on key questions regarding discovery, containment, recovery, and what could have been handled differently.

Focus on Improvement Over Blame

- Reviews often uncover human errors made before detection or during response, such as missing recovery steps or falling for phishing links.
- Security teams treat these errors as learning opportunities to refine processes and strengthen defenses rather than assigning individual blame.

# Post-incident review

Lessons Learned Meeting

- Involves all participating parties within two weeks of remediation to review what happened and assess response performance without placing blame.
- Requires advance preparation, such as establishing an agenda and designating roles like a moderator and scribe, to facilitate cross-departmental collaboration.

Actionable Recommendations

- Identifies gaps in procedures, errors during response actions, and ineffective security controls to prevent repeat occurrences.
- Results in prioritized updates, such as revising incident response playbooks or adopting new security tools.

The Final Report

- Provides a comprehensive overview structured around the 5 W's (who, what, where, why, and when), typically including an executive summary, chronological timeline, investigative findings, and recommendations.
- Should be tailored to the target audience, using appropriate language for non-technical stakeholders and executives who may review it.

# Course 6 M 4

# The importance of logs
Log Fundamentals and Purpose

- Logs record details of what, where, and when events happen, allowing analysts to establish timelines and detect malicious activity.
- Log analysis involves examining log entries to pinpoint specific events of interest and troubleshoot system issues.

Collection and Management

- Log forwarders automatically collect data from multiple sources and forward it to a central repository.
- SIEM tools aggregate, normalize, and process large volumes of diverse log formats in real time for efficient searching.

Log Sources and Entry Components

- Common sources include network devices, operating systems, applications, security tools (IDS/IPS), and authentication systems.
- Key entry fields typically include actions taken (such as ALLOW), source IP addresses, and timestamps used to correlate events.

# Best practices for log collection and management

**Fundamentals and Types of Logs**

- Logs record system and device events, providing critical details around the "5 W's" (who, what, when, where, and why) during incident investigations.
- Common log sources include network devices, operating systems, applications, security systems, and authentication events, which can be configured for standard or verbose detail.

**Log Management and Overlogging Risks**

- Log management involves the full lifecycle of collecting, storing, analyzing, and disposing of data, requiring organizations to strategically determine what needs to be recorded.
- Overlogging can lead to high storage costs, degraded system performance, difficulty identifying key events, and potential compliance issues regarding personally identifiable information (PII).

**Retention and Protection**

- Log retention policies are often guided by regulatory standards across sectors, such as FISMA in the public sector, HIPAA in healthcare, and PCI DSS in finance.
- Sending logs directly to a centralized log server rather than keeping them on local machines helps protect log integrity from unauthorized tampering by attackers.

# Variations of logs

Understanding Logs

- Logs act like receipts by recording activities and events across systems and networks, typically including timestamps, IP addresses, and event descriptions.
- They originate from diverse sources such as operating systems and network devices, varying from short summaries to verbose entries.

Common Log Formats: Syslog and JSON

- Syslog operates as both a protocol and a format containing three parts: a header, structured data in key-value pairs, and a message.
- JSON (JavaScript Object Notation) organizes data using key-value pairs enclosed in curly brackets, valued for its simplicity and readability.

Common Log Formats: XML and CSV

- XML (eXtensible Markup Language) stores and transmits data using tags instead of key-value pairs.
- CSV (Comma Separated Values) structures data records by separating distinct data fields with commas.

# Overview of log file formats

JSON and Syslog Formats

- JSON stores lightweight, readable data using key-value pairs, arrays, and objects enclosed in curly brackets.
- Syslog serves as a protocol, forwarding service, and native Unix log format consisting of a header, structured data, and a message.

XML and CSV Formats

- XML structures Windows data hierarchically using tags, elements, and attributes.
- CSV organizes data by separating values with commas, relying on positional order rather than explicit field names.

Common Event Format (CEF)

- CEF standardizes log entries into pipe-separated fields detailing device vendor, product, version, and event severity.
- Optional extensions provide additional context using key-value pairs, and entries are frequently transported via Syslog.

# Security monitoring with detection tools

Telemetry and Evidence

- Telemetry refers to the collection and transmission of data—such as packet captures—used alongside logs as evidence during security investigations.
- Detection technologies monitor systems to turn raw device activity and telemetry into structured logs for analysis.

Host-Based vs. Network-Based IDS

- Host-based intrusion detection systems (HIDS) are installed directly on endpoints like laptops or servers to monitor local host activity for threats.
- Network-based intrusion detection systems (NIDS) analyze network traffic across specific points, using sensors to gain broad visibility into incoming and outgoing data.

Detection Methods and Logging

- Signature analysis uses predefined rules to identify events of interest, triggering an alert when monitored activity matches a known pattern.
- Detected events are recorded as IDS logs, which can be forwarded to centralized log repositories like a SIEM for further analysis.

# Detection tools and techniques

Types of Intrusion Detection Systems

- Host-based intrusion detection systems (HIDS) are installed as agents on individual endpoints (like computers or servers) to monitor local traffic, file systems, resource usage, and user activity for abnormal behavior.
- Network-based intrusion detection systems (NIDS) are deployed at specific network locations to monitor and inspect traffic across multiple devices, creating a multi-layered defense when paired with HIDS.

Signature-Based Analysis

- Detects threats by matching activity against known patterns or sequences of malicious code (signatures), resulting in a low false-positive rate for known attacks.
- Requires constant updates to a signature database and cannot detect unknown threats, zero-day attacks, or attacks with modified code.

Anomaly-Based Analysis

- Establishes a baseline of normal behavior during a training phase, then alerts on any current activity in the detection phase that deviates from that baseline.
- Capable of catching new and evolving threats, but typically produces a higher rate of false positives and may fail to identify threats if an attacker is already present during the training phase.