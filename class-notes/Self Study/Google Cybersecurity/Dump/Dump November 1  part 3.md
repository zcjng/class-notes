# Course 5 Module 4
SOcial Engineering
Malware
Web-based exploits


Social Engineering

- **Non-Technical Threat:** Cybercriminals do not always rely on sophisticated programming skills; many use **social engineering**, a psychological manipulation technique that exploits human error.
- **Objective:** These tactics trick targets into breaking standard security procedures to surrender private information, credentials, restricted access, or corporate valuables.
- **Flexible Timeframes:** Attacks can take seconds (e.g., a phone call pretending to be tech support asking for a password) or months (e.g., monitoring a temporary worker’s social media activity to exploit their lack of security training).
- **The 4-Stage Attack Lifecycle:**
    
    1. **Prepare:** Researching the target and gathering intelligence to find exploitable angles.
    2. **Establish Trust (Pretexting):** Opening communication using a disguised identity and a fabricated scenario to lower the target's defenses.
    3. **Persuade & Manipulate:** Using corporate terminology and psychological pressure to manipulate the victim into volunteering information.
    4. **Disconnect:** Swiftly breaking communication after collecting the desired data to cover tracks and disappear.
- **Managerial Protections:** The first line of defense includes formal policies, standards, and procedures, such as following the **NIST Special Publication 800-40** standard to govern robust enterprise patch management.
- **The Best Defense:** Because attackers weaponize natural human curiosity and the desire to help, the most effective countermeasure is sharing knowledge and educating others on the behavioral warning signs of an attack.

These are common types of social engineering to watch out for:

- **Baiting** is a social engineering tactic that tempts people into compromising their security. A common example is USB baiting that relies on someone finding an infected USB drive and plugging it into their device.
    
- **Phishing** is the use of digital communications to trick people into revealing sensitive data or deploying malicious software. It is one of the most common forms of social engineering, typically performed via email.
    
- **Quid pro quo** is a type of baiting used to trick someone into believing that they’ll be rewarded in return for sharing access, information, or money. For example, an attacker might impersonate a loan officer at a bank and call customers offering them a lower interest rate on their credit card. They'll tell the customers that they simply need to provide their account details to claim the deal.
    
- **Tailgating** is a social engineering tactic in which unauthorized people follow an authorized person into a restricted area. This technique is also sometimes referred to as piggybacking.
    
- **Watering hole** is a type of attack when a threat actor compromises a website frequently visited by a specific group of users. Oftentimes, these watering hole sites are infected with malicious software. An example is the _Holy Water attack of 2020_ that infected various religious, charity, and volunteer websites.

## Phishing and Its Variants

- **Definition:** **Phishing** is a highly efficient form of social engineering that uses digital communications (primarily email) to trick individuals into revealing sensitive information or deploying malware.
- **Organizational Impact:** A single compromised employee credential can grant attackers full entry into corporate networks, resulting in massive data breaches involving client lists or product secrets.

🧰 The 3 Core Tools in a Phishing Kit

Attackers with minimal technical expertise utilize pre-packaged **phishing kits** to easily deploy automated campaigns using three main tools:

1. **Malicious Attachments:** Files embedded with malware designed to infect and harm an organization's internal infrastructure upon opening.
2. **Fake-Data Collection Forms:** Disguised surveys or data inputs that mimic legitimate corporate forms but prompt users for atypical, sensitive data.
3. **Fraudulent Web Links:** Links leading to spoofed websites designed to mirror trusted brands for the sole purpose of stealing user credentials.

📡 Communication Channels (Phishing Types)

- **Standard Phishing:** Mass-distributed malicious communications targeting users primarily via **email**.
- **Smishing:** The weaponization of **text messages (SMS)** to harvest credentials or impersonate known entities. Threat actors bypass suspicion by spoofing local area codes or posing as friends/family.
- **Vishing:** The exploitation of **electronic voice/phone communications** where criminals impersonate trusted representatives (e.g., tech support, bank staff) claiming account issues to extract sensitive details.

🛡️ Multi-Layered Technical & Policy Defenses

Organizations counteract phishing attempts by combining technical filters with user awareness:

- **Anti-Phishing Policies & Training:** Continuous education programs that instruct employees how to spot suspicious email red flags and properly follow security guidelines.
- **Blocklists & Allowlists:** Defensive filters used to universally ban known malicious email addresses (blocklist) or exclusively permit trusted, verified IP addresses to deliver internal company mail (allowlist).
- **Intrusion Prevention Systems (IPS):** Automated monitoring tools used by analysts to scrutinize email traffic patterns, quarantine suspicious anomalies, and generate actionable event logs.

### **Early persuasion tactics**

One of the earliest instances of phishing was aimed at a popular chat service called AOL Instant Messenger (AIM). Users of the service began receiving emails asking them to verify their accounts or provide personal billing information. The users were unaware that these messages were sent by malicious actors pretending to be service providers.

This was one of the first examples of mass phishing, which describes attacks that send malicious emails out to a large number of people, increasing the likelihood of baiting someone into the trap.

During the AIM attacks, malicious actors carefully crafted emails that appeared to come directly from AOL. The messages used official logos, colors, and fonts to trick unsuspecting users into sharing their information and account details.

Attackers used the stolen information to create fraudulent AOL accounts they could use to carry out other crimes anonymously. AOL was forced to adapt their security policies to address these threats. The chat service began including messages on their platforms to warn users about phishing attacks.


- **Spear phishing** is a subset of email phishing in which specific people are purposefully targeted, such as the accountants of a small business.
    
- **Whaling** refers to a category of spear phishing attempts that are aimed at high-ranking executives in an organization.

- **Angler phishing** is a technique where attackers impersonate customer service representatives on social media. This tactic evolved from people’s tendency to complain about businesses online. they use fraudulent accounts that appear similar to those of actual businesses. Threat actors intercept complaints from places like message boards or comment sections and contact the angry customer via social media., they then trick the angry customers into sharing sensitive information with the promise of fixing their problem.

Malware Types and Behaviors

- **Definition:** **Malware** (short for malicious software) is any software designed to harm devices, disrupt networks, or gain unauthorized control over an infected system without the user's knowledge or permission.
- **Transmission:** It can spread physically via infected media (like USB drives) or digitally over network and internet connections.

🦠 5 Common Types of Malware

- **Virus:** Malicious code that hides inside trusted applications and replicates by cloning itself into other files.
    - It **requires human action** (user activation, like opening an infected file) to trigger the infection and spread.
- **Worm:** Highly contagious malware that duplicates and spreads across network-connected systems entirely on its own.
    - _It **does not require human action**; it uses the infected device as a host to actively scan and infect other connected devices.

A well known example is the Blaster worm, also known as Lovesan, Lovsan, or MSBlast. In the early 2000s, this worm spread itself on computers running Windows XP and Windows 2000 operating systems. It would force devices into a continuous loop of shutting down and restarting. Although it did not damage the infected devices, it was able to spread itself to hundreds of thousands of users around the world. Many variants of the Blaster worm have been deployed since the original and can infect modern computers.


- **Trojan (Trojan Horse):** Malware disguised as a legitimate, useful, or harmless file or application to trick the target into installing it.
    - Attackers frequently use trojans as a baseline entry point to secretly install other payloads, such as ransomware.
- **Ransomware:** A malicious attack that encrypts an organization's or individual's data and demands a financial payment to restore access.
    - Unlike stealthier malware, it **explicitly makes itself known** to the victim in order to collect the ransom. Paying the fee does not guarantee the attacker won't return for more money.
- **Spyware:** Malware used to covertly gather, log, and sell a user's sensitive information (such as login credentials, account PINs, and browsing habits) without consent.
    - Unlike legitimate businesses that collect telemetry data with user opt-out options, spyware operates completely in secret for criminal gain.

## Adware

Advertising-supported software, or **adware**, is a type of legitimate software that is sometimes used to display digital advertisements in applications. Software developers often use adware as a way to lower their production costs or to make their products free to the public—also known as freeware or shareware. In these instances, developers monetize their product through ad revenue rather than at the expense of their users.

Malicious adware falls into a sub-category of malware known as a **potentially unwanted application (PUA)**. A PUA is a type of unwanted software that is bundled in with legitimate programs which might display ads, cause device slowdown, or install other software. Attackers sometimes hide this type of malware in freeware with insecure design to monetize ads for themselves instead of the developer. This works even when the user has declined to receive ads.

### Scareware
Another type of PUA is **scareware**. This type of malware employs tactics to frighten users into infecting their own device. Scareware tricks users by displaying fake warnings that appear to come from legitimate companies. Email and pop-ups are just a couple of ways scareware is spread. Both can be used to deliver phony warnings with false claims about the user's files or data being at risk.

## Fileless malware

**Fileless malware** does not need to be installed by the user because it uses legitimate programs that are already installed to infect a computer. This type of infection resides in memory where the malware never touches the hard drive. This is unlike the other types of malware, which are stored within a file on disk. Instead, these stealthy infections get into the operating system or hide within trusted applications.

**Pro tip:** Fileless malware is detected by performing memory analysis, which requires experience with operating systems. 

## Rootkits

A **rootkit** is malware that provides remote, administrative access to a computer. Most attackers use rootkits to open a backdoor to systems, allowing them to install other forms of malware or to conduct network security attacks.

This kind of malware is often spread by a combination of two components: a dropper and a loader. A **dropper** is a type of malware that comes packed with malicious code which is delivered and installed onto a target system. For example, a dropper is often disguised as a legitimate file, such as a document, an image, or an executable to deceive its target into opening, or dropping it, onto their device. If the user opens the dropper program, its malicious code is executed and it hides itself on the target system.

Multi-staged malware attacks, where multiple packets of malicious code are deployed, commonly use a variation called a loader. A **loader** is a type of malware that downloads strains of malicious code from an external source and installs them onto a target system. Attackers might use loaders for different purposes, such as to set up another type of malware---a botnet.

## Botnet

A **botnet**, short for “robot network,” is a collection of computers infected by malware that are under the control of a single threat actor, known as the “bot-herder.” Viruses, worms, and trojans are often used to spread the initial infection and turn the devices into a bot for the bot-herder. The attacker then uses file sharing, email, or social media application protocols to create new bots and grow the botnet. When a target unknowingly opens the malicious file, the computer, or bot, reports the information back to the bot-herder, who can execute commands on the infected computer.


## Cryptojacking
Cryptojacking is a form of malware that installs ​software to illegally mine cryptocurrencies.
​By far the most telling sign of ​a cryptojacking infection is slowdown. ​Other signs include increased CPU usage, ​sudden system crashes, and fast draining batteries.
​Another sign is unusually high electricity costs ​related to the resource- intensive process ​of crypto mining. ​

It's also good to know that there are ​certain measures you can take to ​reduce the likelihood of experiencing ​a malware attack like cryptojacking. ​These defenses include things like using ​browser extensions designed to block malware, ​using ad blockers, disabling ​JavaScript, and staying alert on the latest trends. ​


This lesson explains how web-based exploits—specifically injection attacks and cross-site scripting (XSS)—take advantage of coding vulnerabilities in web applications to compromise sensitive user data.

  

Web-Based Exploits and Injection Attacks

- Web-based exploits target coding flaws in applications that interact with multiple users and networks, often running malicious code in the background without user awareness.

- Vulnerabilities arise when applications fail to properly validate and sanitize user inputs, such as form fields or interactive elements.

  

Cross-Site Scripting (XSS) Overview

- Cross-site scripting is an injection attack where malicious code, commonly written in HTML or JavaScript, is inserted into a trusted website.

- Successful attacks grant cybercriminals access to elements loaded on the page, including session cookies, geolocation, and hardware like webcams.

  

Types of XSS Attacks

- Reflected XSS sends malicious scripts through links to a server that reflects the code back to the user's browser, while 
- Stored XSS injects scripts permanently directly onto the server itself.

- DOM-based XSS executes purely within the browser's Document Object Model—often via manipulated URL parameters—without needing the script to be processed by the server first.


This lecture explores SQL injection attacks, detailing how they exploit vulnerabilities in web-to-database communications and how to defend against them.

  

Understanding SQL Injection

- A SQL injection occurs when attackers insert malicious SQL code into web input fields, tricking the server into executing unexpected database queries.

- These attacks exploit a lack of input sanitization, particularly in areas like login forms where raw user input is directly concatenated into database statements.

  

Potential Impact of Attacks

- Threat actors target backend queries to access sensitive stored information or alter database tables without authorization.

- Attackers can exploit these flaws to elevate their privileges and potentially obtain full administrative rights over the database.

  

Defensive Strategies and Mitigation

- Sanitizing user input by screening for specific SQL characters prevents unexpected code from reaching the database.

- Utilizing prepared statements enables the server to validate and execute the structure of SQL commands before safely handling user-provided data.  


## SQL injection categories

There are three main categories of SQL injection: 

- In-band
    
- Out-of-band
    
- Inferential
    

In the following sections, you'll learn that each type describes how a SQL injection is initiated and how it returns the results of the attack.

### **In-band SQL injection**

In-band, or classic, SQL injection is the most common type. An in-band injection is one that uses the _same communication channel_ to launch the attack and gather the results.

For example, this might occur in the search box of a retailer's website that lets customers find products to buy. If the search box is vulnerable to injection, an attacker could enter a malicious query that would be executed in the database, causing it to return sensitive information like user passwords. The data that's returned is displayed back in the search box where the attack was initiated.

### **Out-of-band SQL injection**

An out-of-band injection is one that uses a _different communication channel_  to launch the attack and gather the results.

For example, an attacker could use a malicious query to create a connection between a vulnerable website and a database they control. This separate channel would allow them to bypass any security controls that are in place on the website's server, allowing them to steal sensitive data

**Note:** Out-of-band injection attacks are very uncommon because they'll only work when certain features are enabled on the target server.

### **Inferential SQL injection**

Inferential SQL injection occurs when an attacker is unable to directly see the results of their attack. Instead, they can interpret the results by analyzing the _behavior_ of the system.

For example, an attacker might perform a SQL injection attack on the login form of a website that causes the system to respond with an error message. Although sensitive data is not returned, the attacker can figure out the database's structure based on the error. They can then use this information to craft attacks that will give them access to sensitive data or to take control of the system.


Threat modeling is a structured process used by security teams to identify assets, recognize vulnerabilities, and anticipate potential attacks across systems and processes.

  
# Threat Modeling & PASTA Framework

## 1. Threat Modeling Overview
Threat modeling identifies, prioritizes, and neutralizes security risks across applications, systems, and business processes before flaws reach production.

> [!INFO] **Core Objective**
> Proactively locate critical assets, identify potential adversaries, analyze exposure paths, and engineer effective defenses rather than relying solely on reactive patching.

### Risk Treatment Strategies
* **Avoid:** Redesign or eliminate the high-risk feature or workflow entirely.
* **Reduce (Mitigate):** Implement countermeasures, security controls, or patches to lower the probability or impact.
* **Transfer:** Offload risk to a third party (e.g., cyber insurance, outsourcing credit card processing to Stripe).
* **Accept:** Formally acknowledge the residual risk when the cost of mitigation outweighs the expected loss.

---

## 2. Scoping, Assets & Attack Trees

| Concept | Purpose | Example |
| :--- | :--- | :--- |
| **Asset Inventory & Classification** | Categorizes data, hardware, and services by sensitivity. | Marking user PII, workout biometrics, and auth tokens as "Critical Assets". |
| **Threat Actor Profiling** | Catalogs adversaries, motives, and capabilities. | Distinguishing opportunistic script kiddies from organized cybercrime syndicates. |
| **Attack Trees** | Hierarchical diagrams detailing explicit paths to a target. | Tracing an attack from public mobile endpoints down to the backend database. |

Stage 1: Define Objectives

 Focus: Anchor technical security to business priorities, risk appetite, and legal requirements.

 Key Activities:

 Define organizational and operational goals.

 Catalog compliance drivers (e.g., GDPR, HIPAA, PCI-DSS).

 Estimate baseline financial and reputational impact of a compromise.

 Fitness App Example: Ensuring user GPS tracks and biometrics are never exposed; meeting privacy regulations to avoid regulatory penalties and public churn.

Stage 2: Define Technical Scope

 Focus: Establish the boundaries of what is being modeled and enumerate every technical asset.

 Key Activities:

 Map all client targets, endpoints, and microservices.

 Catalog databases, network interfaces, and third-party dependencies.

 Define system architecture perimeters and hardware/IoT connections.

 Fitness App Example:

 Client: iOS and Android mobile binaries.

 Network: BLE link to heart-rate monitors; HTTPS/REST API traffic.

 Backend: Cloud database storing biometric timeseries and user profiles.

Stage 3: Application Decomposition

 Focus: Deconstruct system architecture with developers to understand how data moves across barriers.

 Key Activities:

 Map Data Flow Diagrams (DFDs) identifying processes, data stores, and entities.

 Identify and draw trust boundaries.

 Inventory existing security mechanisms (encryption keys, auth middleware, token validation).

 Fitness App Example:

 Visualizing step-count and GPS data flow: Wearable \rightarrow Phone App \rightarrow Public Gateway \rightarrow Workout Service \rightarrow Database.

 Auditing JWT signing logic and on-device SQLite database encryption (SQLCipher).

Stage 4: Threat Analysis

 Focus: Shift to an attacker mindset and ingest threat intelligence to pinpoint plausible adversaries.

 Key Activities:

 Extract threat data from intelligence feeds, past incident logs, and industry reports.

 Profile adversary motives, funding, and attack tooling.

 Define probable attack vectors.

 Fitness App Example: Identifying threat vectors like automated credential stuffing bots attacking the login portal, or local network eavesdropping on open gym Wi-Fi.

Stage 5: Vulnerability & Flaws Analysis

 Focus: Pinpoint technical weaknesses, design gaps, and misconfigurations across the scoped assets.

 Key Activities:

 Run static (SAST) and dynamic (DAST) vulnerability assessments.

 Review code for architectural flaws and logic defects.

 Map found vulnerabilities to CVEs and CWE categories.

 Fitness App Example: Discovering dynamic SQL assembly in the workout search route (⁠/api/v1/user/workouts?search=⁠), missing rate-limiting on reset passwords, and tokens cached in plain text.

Stage 6: Attack Modeling & Simulation

 Focus: Validate whether uncovered vulnerabilities can actually be chained and exploited under current defenses.

 Key Activities:

 Build attack trees and exploit path flowcharts.

 Execute penetration testing and simulation against identified vulnerabilities.

 Test whether WAFs, filters, or access controls successfully break the attack chain.

 Fitness App Example: Simulating an exploit path from reverse-engineering the APK to extract API routes \rightarrow bypassing perimeter validation \rightarrow executing SQL injection to dump user profile tables.

Stage 7: Risk & Impact Analysis

 Focus: Synthesize simulation findings into quantified residual risk and concrete, prioritized mitigations.

 Key Activities:

 Calculate probability vs. business impact (residual risk scoring).

 Eliminate false positives from exploit testing results.

 Deliver prioritized security recommendations to engineering teams and executives.

 Fitness App Example:

 Critical Priority: Switch to parameterized SQL queries/ORM and deploy WAF rules immediately.

 Medium Priority: Enforce certificate pinning in the next sprint cycle to mitigate gym Wi-Fi sniffing.

 Low/Accept: Accept transient caching of non-sensitive route metadata in memory.



# Application Security & Threat Modeling

## Overview
Threat modeling is the proactive process of identifying assets, uncovering vulnerabilities, and evaluating how each is exposed to potential threats. It strategically bridges vulnerability management, threat intelligence, and incident response to protect systems before vulnerabilities reach production.

> [!INFO] **Core Objective**
> Uncover design flaws and weaknesses early to reduce operational and business risk across any system, software architecture, or workflow.

---

## The Critical Need for Application Security
Applications (both web and mobile) serve as the primary gateway connecting global customers, partners, and corporate infrastructure. The immense scale and volume of sensitive data they process make the application layer an attractive and high-value target for adversaries.

> [!EXAMPLE] **High-Impact Vulnerability: Log4Shell (CVE-2021-44228)**
> An unpatched vulnerability in the popular Apache Log4j Java logging library allows unauthenticated remote code execution (RCE). A single flaw in a widely used dependency can grant attackers complete administrative takeover across millions of internet-connected hosts globally.

---

## Defending the Application Layer: The DevSecOps Lifecycle
Application-layer defense requires continuous testing across development pipelines. Threat modeling is driven collaboratively by **DevSecOps** (Development, Security, and Operations) teams.

> [!TIP] **SDLC Integration**
> Threat modeling is not a single gate—it must be executed **before, during, and after** development across all stages of the Software Development Life Cycle (SDLC).

### The 6-Step Threat Modeling Cycle
1. **Define the Scope:** Establish boundaries, system inventories, and business purpose.
2. **Identify Threats:** Profile potential threat actors and attack scenarios.
3. **Characterize the Environment:** Map data flows, trust boundaries, and dependencies.
4. **Analyze Threats:** Map attack vectors against discovered vulnerabilities.
5. **Mitigate Risks:** Engineer countermeasures, controls, or architecture changes.
6. **Evaluate Findings:** Assess residual risk, test controls, and feed lessons learned back into the cycle.

---

## Common Threat Modeling Frameworks

| Framework | Core Approach | Best Suited For |
| :--- | :--- | :--- |
| **STRIDE** | Attack-vector categorization | Granular software and feature-level threat identification |
| **PASTA** | Risk-centric, evidence-based simulation | Aligning technical vulnerabilities with business objectives |
| **Trike** | Security- and defensive-centric | Access control, permissions, and privilege models |
| **VAST** | Visual, agile, and automated | Enterprise scale, automated pipelines, and fast-paced agile teams |

### 1. STRIDE
Developed by Microsoft, STRIDE categorizes threats into six core attack vectors:
* **S - Spoofing:** Pretending to be someone or something else (violates *Authentication*).
* **T - Tampering:** Modifying code or data without authorization (violates *Integrity*).
* **R - Repudiation:** Denying that an action was performed without proof (violates *Non-repudiation*).
* **I - Information Disclosure:** Exposing confidential data to unauthorized parties (violates *Confidentiality*).
* **D - Denial of Service:** Degrading or interrupting system availability (violates *Availability*).
* **E - Elevation of Privilege:** Gaining unearned authorization or administrative access (violates *Authorization*).

### 2. PASTA (Process for Attack Simulation and Threat Analysis)
* **Design:** Risk-centric, 7-stage framework created by OWASP contributors and supported by VerSprite.
* **Mechanism:** Relies on tangible artifacts (e.g., vulnerability scans, architecture diagrams, threat intelligence) to simulate attacker paths and quantify real-world business impact.

### 3. Trike
* **Design:** Open-source, security-focused methodology and tooling platform.
* **Mechanism:** Models system use cases, permission models, acceptable risk levels, and privilege boundaries to ensure assets remain within defined security states.

### 4. VAST (Visual, Agile, and Simple Threat Modeling)
* **Design:** Underpinned by the ThreatModeler® platform.
* **Mechanism:** Visual and automated approach designed to integrate seamlessly into rapid Agile and DevOps workflows, scaling assessments across large enterprise development teams.

---

## Practical Threat Modeling: The 5 Guiding Questions
While complex diagrams (DFDs) and attack trees require practice, effective threat modeling fundamentally revolves around asking five core questions:

1. **What are we working on?** (Scope, components, and data flows)
2. **What kinds of things can go wrong?** (Threat identification and attack vectors)
3. **What are we doing about it?** (Controls, countermeasures, and risk mitigation)
4. **Have we addressed everything?** (Coverage checks and residual risk evaluation)
5. **Did we do a good job?** (Retrospective and process assessment)

> [!WARNING] **Mindset Shift**
> You do not need decades of architecture experience to contribute effectively to a threat model. Value starts with adopting an **attacker mindset**—critically inspecting assumptions about how data is received, processed, and stored.
