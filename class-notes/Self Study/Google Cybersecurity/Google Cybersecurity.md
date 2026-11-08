---
course: Coursera Google Cybersecurity
type: MOC
tags:
  - google
  - cybersecurity
  - moc
---

# 🛡️ Google Cybersecurity — Course Hub

Central index for the **Coursera Google Cybersecurity Professional Certificate**. Use this note to jump to daily study logs, or straight into any topic note.

> [!INFO] Official program
> 9 courses · pass every graded assignment (80% to pass each course) · then pay the fee, get Financial Aid, or go through a sponsoring institution/employer/agency.

---

## 🗓️ Daily Study Logs

- [[2026-10-14]] — Wednesday · *Foundations of Cybersecurity* (incl. M3 backfill: controls, compliance, ethics, tools)
- [[2026-10-15]] — Thursday · *Play It Safe: Manage Security Risks* (+ logs, SIEM & playbooks)
- [[2026-10-16]] — Friday · *Connect and Protect* M1 — networks, cloud, TCP/IP & OSI, addressing
- [[2026-10-17]] — Saturday · *Connect and Protect* M2–M3 — protocols, Wi-Fi, firewalls, VPNs, segmentation
- [[2026-10-18]] — Sunday · *Connect and Protect* M3 finish + start of *Tools of the Trade* (Linux & SQL)
- [[2026-10-21]] — Wednesday · *Tools of the Trade* — inside the OS, virtualization, package managers, Linux file system & filtering (merged Oct 19 session)
- [[2026-10-22]] — Thursday · *Tools of the Trade* M4 — databases & SQL queries
- [[2026-10-23]] — Friday · *Assets, Threats, and Vulnerabilities* M1–M2 — risk planning, assets, NIST CSF tiers, privacy, data governance, cryptography
- [[2026-11-01]] — Sunday · *Assets, Threats, and Vulnerabilities* M2–M4 — PKI & hashing, AAA/IAM, CI/CD, CVE & OWASP, pen testing, malware, threat modeling
- [[2026-11-03]] — Tuesday · *Sound the Alarm* M1–M2 — IR lifecycle, CSIRT & SOC, IDS/IPS/EDR, SIEM/SOAR, network monitoring
- [[2026-11-04]] — Wednesday · *Sound the Alarm* M2 — packet anatomy, IPv4/IPv6 headers, PCAP, Wireshark filters
- [[2026-11-05]] — Thursday · *Sound the Alarm* M2–M4 — tcpdump, threat hunting, IoCs, triage, containment, logs
- [[2026-11-08]] — Sunday · *Sound the Alarm* M4 finish — Suricata signatures & logs, SIEM queries (Splunk/Chronicle) + Python regex

---

## 🧠 Topic Notes

### Foundations — Security Basics

- [[Introduction to Cybersecurity]] — what cybersecurity is, why it matters, threat types
- [[Security Analyst Roles & Skills]] — job titles, responsibilities, tools, PII/SPII
- [[CIA Triad]] — confidentiality, integrity, availability

### Frameworks, Controls & Standards

- [[Security Frameworks]] — what frameworks/compliance/posture are
- [[Security Controls]] — physical / technical / administrative controls
- [[NIST Cybersecurity Framework]] — the CSF core functions (Govern → Recover)
- [[NIST SP 800-53]] — US federal control catalogue
- [[ISO IEC 27001]] — international asset-security standard
- [[OWASP Security Principles]] — design principles (least privilege, defense in depth, fail securely…)
- [[Compliance & Regulations]] — FERC-NERC, FedRAMP, CIS, GDPR, PCI DSS, HIPAA, ISO, SOC 1/2
- [[Security Ethics]] — counterattack laws, ICJ conditions, PII/SPII obligations

### Tools & Automation

- [[Security Tools]] — antivirus, IDS, encryption, pen testing, packet sniffers, web vulnerabilities
- [[Logs & SIEM]] — log types, SIEM hosting options, SOAR, Splunk, Chronicle, Suricata
- [[Playbooks & Incident Response]] — IR playbook phases, chain of custody, order of volatility
- [[Linux, SQL & Python]] — automation value and the core analyst toolset

### Linux & Operating Systems (Course 4)

- [[Operating Systems]] — OS basics, OS security, legacy operating systems
- [[Inside the Operating System]] — boot process, OS request flow, resource management
- [[Virtualization]] — VMs, vCPU, hypervisor, host vs guest OS
- [[Linux Package Managers]] — APT/`dpkg` vs YUM/RPM, dependencies, patching
- [[Linux File System]] — FHS directories, absolute vs relative paths
- [[Linux Commands]] — `pwd`, `ls`, `cd`, `cat`, `head`, `tail`, `less`
- [[Linux Filtering]] — `grep`, piping, `find` with name/time options

### SQL & Databases (Course 4)

- [[Databases]] — relational structure, primary & foreign keys
- [[SQL Basics]] — `SELECT`, `FROM`, `ORDER BY`
- [[SQL Filtering]] — `WHERE`, `LIKE`, `BETWEEN`, `AND`/`OR`/`NOT`
- [[SQL Joins]] — inner, left, right, full outer joins

### Threats & Attacks

- [[Threat Actors]] — external/internal threats, APTs, insider threats
- [[Phishing & Social Engineering]] — phishing families & watering-hole attacks
- [[Malware History]] — Brain virus, Morris worm, CERTs
- [[Network Attacks]] — interception, backdoors, DoS/DDoS, sniffing, IP spoofing, brute force

### Networking

- [[Network Fundamentals]] — LAN/WAN, switches, hubs, routers, WAPs, router modes
- [[TCPIP & OSI Models]] — data packets, the 4-layer TCP/IP stack, the 7 OSI layers
- [[IP Addressing & ARP]] — public/private IPs, NAT, DHCP, MAC, ARP spoofing, IPv4 vs IPv6
- [[Network Protocols]] — TCP, DNS, HTTPS, SSH, email protocols & their ports
- [[Wi-Fi Security]] — IEEE 802.11, WEP → WPA3, KRACK, SAE
- [[Firewalls]] — hardware/software/FaaS, stateful vs stateless, NGFW
- [[VPNs]] — encrypted tunnels, encapsulation, WireGuard vs IPSec
- [[Proxy Servers]] — forward, reverse, and email proxies
- [[Network Segmentation]] — security zones, DMZ, subnetting, CIDR

### Cloud

- [[Cloud Computing]] — SaaS/IaaS/PaaS, hybrid & multi-cloud, SDNs, business benefits
- [[Cloud Security]] — IAM, shared responsibility model, zero-days, cloud hardening

### Risk, Auditing, Domains & Vulnerabilities

- [[Risk Management]] — NIST RMF steps & risk-response strategies
- [[Security Audits]] — internal audits, elements, checklists, Botium Toys case
- [[CISSP Eight Domains]] — the CBK domains
- [[Common Vulnerabilities]] — ProxyLogon, ZeroLogon, Log4Shell, PetitPotam…
- [[Vulnerability Assessment]] — VMs, sandboxes, hashing/salting, MFA, password policies

### Assets, Risk & Data (Course 5)

- [[Security Risk Planning]] — assets + threats + vulnerabilities; likelihood × impact; risk matrix
- [[Asset Management & Classification]] — asset types, classification levels, ownership, data aggregation
- [[Security Plans & Policies]] — policies → standards → procedures; regulations vs. compliance vs. frameworks
- [[Information Privacy]] — right to choose, principle of least privilege, privacy vs. security, audits vs. assessments
- [[Data Governance]] — lifecycle, owner/custodian/steward, PII/SPII/PHI, three states of data
- [[Cryptography]] — plaintext/ciphertext, ciphers, Caesar cipher, brute force, key management

### Cryptography, Hashing & Access (Course 5)

- [[Encryption & PKI]] — symmetric vs. asymmetric, 3DES/AES/RSA/DSA, Kerckhoff's principle, certificates & CAs
- [[Hashing & Password Storage]] — one-way digests, MD5/SHA, collisions, rainbow tables, salting
- [[Access Control & AAA]] — authentication factors, SSO/LDAP/SAML, MFA, MAC/DAC/RBAC, sessions, IAM

### Vulnerabilities & Application Security (Course 5)

- [[Vulnerability Management]] — vulnerability vs. exposure vs. exploit, CVE/CNA/NVD/CVSS, scanners, patching, EOL
- [[OWASP Top 10]] — the ten most-targeted web vulnerability categories
- [[Penetration Testing]] — red/blue/purple teams, open/closed/partial knowledge, bug bounties
- [[Threat Modeling]] — risk treatment, PASTA's 7 stages, STRIDE, Trike, VAST
- [[Web Exploits & Injection]] — reflected/stored/DOM XSS, SQLi categories, prepared statements
- [[Defense in Depth]] — the castle approach, five layers, attack surfaces, hardening
- [[CICD Security]] — CI/CD stages, DevSecOps, SAST/DAST/SCA, pipeline vulnerabilities

### Threats & Malware (Course 5)

- [[Malware Types & Behaviors]] — virus, worm, trojan, ransomware, spyware, PUA, fileless, rootkits, botnets

### Detection & Response (Course 6)

- [[Incident Response Lifecycle]] — the four NIST phases, event vs. incident, the 5 W's, IR plans
- [[CSIRT & SOC Teams]] — CSIRT roles, command/control/communication, SOC tiers L1–L3
- [[Incident Triage]] — assess, prioritize, collect, escalate
- [[Business Continuity]] — containment/eradication/recovery, BCP vs. DRP, hot/warm/cold sites
- [[Post-Incident Activity]] — final report, lessons learned, chain of custody
- [[IDS & IPS]] — IDS vs. IPS vs. EDR, true/false positives, HIDS vs. NIDS, signature vs. anomaly
- [[Threat Hunting & Intelligence]] — proactive hunting, TTPs, TIPs, OSINT, honeypots
- [[Indicators of Compromise]] — IoC vs. IoA, the Pyramid of Pain, CI/CD IoCs

### Network Forensics & Logs (Course 6)

- [[Network Traffic Analysis]] — baselines, flow/payload/temporal monitoring, exfiltration, SOC vs. NOC
- [[Packet Analysis]] — packet anatomy, IPv4/IPv6 headers, capture modes, PCAP libraries
- [[Wireshark]] — display filters, operators, protocol filters, follow-stream
- [[tcpdump]] — syntax, flags, filter expressions
- [[Log Management & Formats]] — forwarders, ingestion, indexing, overlogging, retention, JSON/Syslog/XML/CSV/CEF
- [[Suricata]] — signature anatomy (action/header/options), rule files, `eve.json` vs `fast.log`
- [[SIEM Queries]] — Splunk SPL, Chronicle UDM vs. raw log search, YARA-L

### Python (Course 7 — partial)

- [[Regular Expressions]] — `re.findall()`, character classes, quantifiers, analyst use cases

---

## 📚 Course List

1. [Foundations of Cybersecurity](https://www.coursera.org/learn/foundations-of-cybersecurity/home/week/1)
2. [Play It Safe: Manage Security Risks](https://www.coursera.org/learn/manage-security-risks/home/week/1)
3. [Connect and Protect: Networks and Network Security](https://www.coursera.org/learn/networks-and-network-security/home/week/1)
4. [Tools of the Trade: Linux and SQL](https://www.coursera.org/learn/linux-and-sql/home/week/1)
5. [Assets, Threats, and Vulnerabilities](https://www.coursera.org/learn/assets-threats-and-vulnerabilities/home/week/1)
6. [Sound the Alarm: Detection and Response](https://www.coursera.org/learn/detection-and-response/home/week/1)
7. [Automate Cybersecurity Tasks with Python](https://www.coursera.org/learn/automate-cybersecurity-tasks-with-python/home/week/1)
8. [Put It to Work: Prepare for Cybersecurity Jobs](https://www.coursera.org/learn/prepare-for-cybersecurity-jobs/home/week/1)
9. [Accelerate Your Job Search with AI](https://www.coursera.org/learn/accelerate-your-job-search-with-ai/home/module/1)

> [!SUCCESS] Progress
> **Courses 1–6 — complete.** Course 7 (*Automate Cybersecurity Tasks with Python*) — only the regex module, since Python was already familiar. Course 8 (*Put It to Work*) — skipped as unnecessary.

---

## 🔗 Useful Links & Resources

- [Coursera support — Google Cybersecurity community](https://www.coursera.support/s/google-cybersecurity-community)
- [Cybersecurity Certificate glossary](https://docs.google.com/document/d/1Feb8pHRY-blnpaLOohds2esd6IWdCIp-ikG7G_omSj4/template/preview?usp=sharing&resourcekey=0-YHcAISkCiqGDq5KwO6yNeQ)

### ✅ How to Obtain the Certificate

To receive the official Google Cybersecurity Certificate you must:

- Pass **all graded assignments** in all **9 courses** (each graded assignment feeds a cumulative score; the passing grade per course is **80%**).

AND **one** of the following:

- Pay the [course certificate fee](https://www.coursera.support/s/article/209818963-Payments-on-Coursera?language=en_US),
- Be approved for [Coursera Financial Aid](https://www.coursera.support/s/article/209819033-Apply-for-Financial-Aid-or-a-Scholarship?language=en_US), **or**
- Complete the certificate through an educational institution, employer, or agency that is sponsoring your participation.

### 💼 Program Selling Points

- Learn incident detection and Python scripting for automation
- Build **portfolio projects**
- Can connect you with **200+ employers**
