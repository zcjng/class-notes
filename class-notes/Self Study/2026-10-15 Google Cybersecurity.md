---
course: Coursera Google Cybersecurity
tags:
  - google
day: Thursday
---
## 📚 Today's Study Log - 
- 




## 🗑️ Dumps
Security frameworks are guidelines used for building plans to help mitigate risk and threats to data and privacy
- Security controls are safeguards designed to reduce specific security risks. 
Types of Security Controls (Continued)

• Encryption: Converts data from a readable format (plaintext) to an encoded, unreadable format (ciphertext) to ensure confidentiality.
• Authentication: Verifies the identity of a user or system. This ranges from basic username/password logins to advanced multi-factor authentication (MFA) using biometrics.

Authorization and the CIA Triad

• Authorization: Grants access to specific resources within a system, ensuring that only authorized individuals can access particular data.
• Core Model: These security controls are fundamental elements of the CIA triad, which is a core security model that security teams use to protect organizations.

Examples of physical controls:
Gates, fences, and locks
Security guards
Closed-circuit television (CCTV), surveillance cameras, and motion detectors
Access cards or badges to enter office spaces

Examples of technical controls:
Firewalls
MFA
Antivirus software

Examples of administrative controls:
Separation of duties
Authorization
Asset classification

Cyber Threat Framework (CTF), common language

International Organization for Standardization/International Electrotechnical Commission (ISO/IEC) 27001 enables organizations of all sectors and sizes to manage the security of assets, such as financial information, intellectual property, employee data, and information entrusted to third parties.


CIA triad is a model that helps inform how organizations consider risk when setting up systems and security policies.

Confidentiality is the idea that only authorized users can access specific assets or data. Like least privilege

Integrity is the idea that the data is verifiably correct, authentic, and reliable. Uses Cryptography uses mathematical techniques to protect the security of information
Encryption can be used to prevent access and ensure data, such as messages on an organization's internal chat platform, cannot be tampered with.  

Availability is the idea that data is accessible to those who are authorized to use it. 

**NIST Cybersecurity Framework (CSF)**

The NIST CSF is a voluntary framework offering standards, guidelines, and best practices for managing cybersecurity risk.

It consists of five core functions: Identify, Protect, Detect, Respond, and Recover, which guide security professionals in handling incidents.

**NIST Special Publication (SP) 800-53**

NIST SP 800-53 provides a unified framework for protecting the security of information systems within the U.S. federal government.

Its security controls help maintain the Confidentiality, Integrity, and Availability (CIA) triad for government systems.

nist framework 
1. ⚖️ Govern (GV) — The Brand New Addition
This functions as the umbrella covering all other pillars. It focuses on establishing your organization’s cybersecurity business strategy, policies, and roles.
• In plain terms: Deciding who is in charge, establishing the security rules, and aligning cyber risk with overall business goals.

2. 🔍 Identify (ID)

Understanding your unique business environment to pinpoint your assets and risks. You cannot protect what you do not know exists.
• In plain terms: Taking inventory of all your software, hardware, and data, and finding your weakest security links before hackers do.

3. 🛡️ Protect (PR)

Developing and implementing the safeguards to prevent a cyberattack or contain its impact.
• In plain terms: Training employees, setting up firewalls, using strong passwords, and restricting system access so only authorized users can enter.

4. 🚨 Detect (DE)

Actively monitoring your environment to spot potential security incidents right away.
• In plain terms: Setting up continuous security alerts and log monitoring so that if an unauthorized user breaches your defenses, you know immediately.

5. 💥 Respond (RS)

Executing a precise plan of action the moment a security event is detected.
• In plain terms: Isolating infected computers, shutting down compromised networks, and communicating the breach to the correct stakeholders to stop the damage from spreading.

6. 🔄 Recover (RC)

Restoring any capabilities or services that were impaired due to a cyber incident.
• In plain terms: Getting your business systems safely back online, restoring lost data from backups, and learning from the event to improve future resilience.


OWASP  Open Worldwide Application Security Project® security principles

Minimizing Attack Surface and Least Privilege

• Minimize Attack Surface Area: Reduce potential vulnerabilities by disabling unnecessary software features, restricting asset access, and enforcing stronger password policies.
• Principle of Least Privilege: Grant users only the minimum access required for their tasks to limit potential damage from security breaches.

2. Defense in Depth and Separation of Duties

• Defense in Depth: Implement multiple security controls—like multi-factor authentication, firewalls, and intrusion detection systems—to create layered protection.
• Separation of Duties: Prevent fraudulent activities by ensuring no single individual has excessive privileges that could be misused.

3. Keeping Security Simple and Fixing Issues Correctly

• Keep Security Simple: Avoid overly complex security solutions that can become unmanageable and hinder collaboration.
• Fix Security Issues Correctly: Identify the root cause of security incidents, correct vulnerabilities, and conduct tests to ensure successful repairs.

Establish secure defaults
This principle means that the optimal security state of an application is also its default state for users; it should take extra work to make the application insecure. 

Fail securely
Fail securely means that when a control fails or stops, it should do so by defaulting to its most secure option. For example, when a firewall fails it should simply close all connections and block all new ones, rather than start accepting everything.

Don’t trust services
Many organizations work with third-party partners. These outside partners often have different security policies than the organization does. 


Understanding Security Audits

A security audit reviews an organization's security controls, policies, and procedures against a set of expectations.

• Internal security audits, often conducted by an organization's compliance officer and security team, help improve security and avoid fines.

Elements of Internal Security Audits

**Establishing Scope and Goals:** This involves identifying people, assets, policies, and technologies that impact security, and outlining objectives for improvement.

• **Conducting a Risk Assessment:** This step focuses on identifying potential threats, risks, and vulnerabilities to determine necessary security measures.

Completing a Controls Assessment

This involves reviewing existing assets and evaluating potential risks to ensure internal controls are effective.

Controls are categorized into administrative (human-related policies), technical (hardware/software solutions), and physical (measures to prevent physical access).

Assessing Compliance and Communicating Results

Organizations must adhere to compliance regulations, such as GDPR and PCI DSS, to secure private data.

The audit's findings, including risks, compliance adherence, and recommendations for improving security, must be communicated to stakeholders.

Value of Internal Audits

Internal audits are crucial for identifying security gaps and determining areas for improvement within an organisation 

Factors that affect audits
Factors that determine the types of audits an organization implements include: 

Industry type

Organization size

Ties to the applicable government regulations

A business’s geographical location

A business decision to adhere to a specific regulatory compliance


There are three main categories of controls to review during an audit, which are administrative and/or managerial, technical, and physical controls. To learn more about specific controls related to each category, click the following link and select “Use Template.” 

Link to template: Control categories


Audit checklist
It’s necessary to create an audit checklist before conducting an audit. A checklist is generally made up of the following areas of focus:

**Identify the scope of the audit**

The audit should:

List assets that will be assessed (e.g., firewalls are configured correctly, PII is secure, physical assets are locked, etc.) 

Note how the audit will help the organization achieve its desired goals

Indicate how often an audit should be performed

Include an evaluation of organizational policies, protocols, and procedures to make sure they are working as intended and being implemented by employees

**Complete a risk assessment**

A risk assessment is used to evaluate identified organizational risks related to budget, controls, internal processes, and external standards (i.e., regulations).

**Conduct the audit**

When conducting an internal audit, you will assess the security of the identified assets listed in the audit scope.

**Create a mitigation plan**

A mitigation plan is a strategy established to lower the level of risk and potential costs, penalties, or other issues that can negatively affect the organization’s security posture. 

**Communicate results to stakeholders**

The end result of this process is providing a detailed report of findings, suggested improvements needed to lower the organization's level of risk, and compliance regulations and standards the organization needs to adhere to.  