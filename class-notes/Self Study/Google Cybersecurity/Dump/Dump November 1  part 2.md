# Course 5 module 3
critical area of modern software development: CI/CD pipelines.
Continuous Integration, Continuous Delivery, and Continuous Deployment (CI/CD) pipelines are essential for modern software development. They help teams deliver software faster and more efficiently. But, like any powerful tool, CI/CD pipelines can also introduce security risks if not properly managed.


CI/CD automates the entire software release process, from code creation to deployment. This automation is what enables modern development teams to be agile and respond quickly to user needs.

### **Continuous Integration (CI): Building a Solid Foundation**

**Continuous Integration (CI)** is all about frequently merging code changes from different developers into a central location. This triggers automated processes like building the software and running tests. CI catches problems through an automated process: every time code is integrated, the system automatically builds and tests it. This immediate feedback loop reveals integration problems as soon as they occur. CI helps catch integration problems early, leading to higher quality code. Think of it as the foundation of the pipeline.

### **Continuous Delivery (CD): Ready to Release**

**Continuous Delivery** means your code is always ready to be released to users. After passing automated tests, code is automatically deployed to a staging environment (a practice environment) or prepared for final release. Typically, a manual approval step is still needed before going live to production, which provides a control point.

### **Continuous Deployment (CD): Fully Automated Releases**

**Continuous Deployment** automates the entire release process. Changes that pass all automated checks are automatically deployed directly to the live production environment, with no manual approval. This is all about speed and efficiency.

CD allows you to build security checks right into your deployment pipeline. This ensures that only thoroughly vetted software versions are released.

These automated security checks can include:

- Dynamic Application Security Testing (DAST): Automated tests that find vulnerabilities in running applications in realistic staging environments.
    
- Security Compliance Checks: Automated checks that ensure software meets your organization’s security rules and policies.
    
- Infrastructure Security Validations: Checks that make sure the systems hosting your software are secure.

## **Why a secure CI/CD Pipelines is Non-Negotiable**

To grasp the power of CI/CD is vital.  Pipeline protection is not optional; it is essential. Consider these points:

- **Secure Automation:** CI/CD automates repetitive tasks: building, testing, deploying. When automation is implemented securely, this reduces errors from manual work, speeds processes, and importantly, reduces human errors that create vulnerabilities. However, insecure automation automates the introduction of vulnerabilities at scale.
    
- **Improved Code Quality Via Security Checks:** Automated tests in CI/CD rigorously check code before release. Crucially, this includes automated security tests. This leads to fewer bugs and security weaknesses in final software, but only if security tests integrate effectively within the pipeline.
    
- **Faster Time to Market for Security Updates:** CI/CD accelerates releases. This enables faster delivery of new features, bug fixes, _and security updates_, improving response time to both user needs and security threats. This rapid deployment of security updates is a significant security advantage of a well-secured CI/CD pipeline.
    
- **Enhanced Collaboration and Feedback with Safety Focus:** CI/CD encourages collaboration between development, security, testing, and operations teams. Quick feedback loops aid identification and resolution of vulnerabilities early in development. This collaborative environment is essential to build security into the pipeline and address vulnerabilities proactively.
    
- **Reduced Risk:** Frequent, smaller releases, a result of CI/CD, are less risky than large, infrequent releases. If issues arise (including security issues), pinpointing and fixing the problem becomes easier. This also applies to security vulnerabilities; smaller, frequent releases limit the potential impact of a security flaw introduced in any single release, provided security monitoring and testing remain continuous.

## **Common CI/CD Pipeline Vulnerabilities: What to Watch Out For**


### **Insecure Dependencies: Risks from Third-Party Code**

CI/CD pipelines often use many third-party libraries and components. If these components have known vulnerabilities (Common Vulnerabilities and Exposures, or CVEs), those vulnerabilities can be unknowingly added to your application during the automated build process.

**Action Step:** Regularly scan and update your dependencies. Make sure you’re using secure versions of all external components.

### **Misconfigured Permissions: Controlling Access**

Weak access controls in CI/CD tools, code repositories, and related systems are a significant vulnerability. Unauthorized access can allow attackers to modify code, pipeline configurations, or inject malicious content.

**Action Step:** Implement strong access management using Role-Based Access Control (RBAC). Ensure only authorized individuals can access and change critical pipeline elements.

### **Lack of Automated Security Testing: Missing Critical Checks**

Failing to include automated security testing in your CI/CD pipeline is a serious error. Without tools like SAST and DAST, you are almost guaranteed to release software full of vulnerabilities that will go undetected until after it's live, leading to significantly higher costs and effort to fix..

**Action Step:** Integrate automated security testing (SAST and DAST) into your CI/CD pipeline. This should be a core part of your secure CI/CD strategy.

### **Exposed Secrets: Protecting Sensitive Information**

Hardcoding sensitive data like API keys, passwords, and tokens directly into code or pipeline settings is a serious security mistake. If exposed, these secrets can lead to major security breaches.

**Action Step:** Never hardcode secrets. Use secure vaults or dedicated secrets management tools to store and manage sensitive information. Enforce this practice across your team.

### **Unsecured Build Environments: Protecting the Pipeline Infrastructure**

The CI/CD environment itself (the servers and systems that run your pipeline) needs to be secure. If this environment is vulnerable, attackers can compromise it to alter builds, inject malicious code, or steal sensitive data.

**Action Step:** Harden your build environments. Use secure containers or virtual machines to minimize the risk of a compromised pipeline.

## **Building a Secure CI/CD Pipeline: Defense in Depth**
- **Integrate Security from the Start: Embrace DevSecOps:** Adopt a **DevSecOps** mindset. This means building security into _every_ stage of development, from planning to deployment and beyond. This naturally includes embedding security checks into your CI/CD pipeline.
    
- **Implement Strong Access Controls:** Use strict permission policies based on the principle of least privilege. Only grant necessary access to code, pipeline settings, and deployment configurations. Use tools like Multi-Factor Authentication (MFA) and Role-Based Access Control (RBAC) to secure your CI/CD environment.
    
- **Automate Security Testing Everywhere:** Make automated security scans and tests a fundamental part of your build and deployment process. Tools like SAST, Software Composition Analysis (SCA), and DAST are not optional extras – they are essential for a secure CI/CD pipeline so you can catch vulnerabilities early.
    
- **Keep Dependencies Updated:** Maintain a current inventory of all third-party dependencies, libraries, and CI/CD plugins. Regularly update these components to patch security vulnerabilities (CVEs). Tools like [Dependabot](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide) and [Snyk](https://snyk.io/) can automate dependency management.
    
- **Secure Secrets Management:** Never hardcode sensitive information in your code or pipeline configurations. Require the use of dedicated secrets management tools like HashiCorp Vault or AWS Secrets Manager. Securely store, access, and rotate secrets throughout the CI/CD process.



defense-in-depth model, a multi-layered security approach designed to minimize risk by placing redundant protective controls around an organization's critical assets.

The Castle Approach

- Resembles medieval castle architecture, where multiple barriers such as moats, walls, and watchtowers ensure protection even if an outer barrier is breached.
- Employs unique security controls at each level to identify vulnerabilities and stop attacks when one system fails.

The Outer Layers (Perimeter, Network, and Endpoint)

- The perimeter layer authenticates external users (e.g., using usernames and passwords) to filter external access.
- The network layer handles authorization and traffic control via firewalls, 
- The endpoint layer protects network-connected devices using tools like antivirus software.

The Core Layers (Application and Data)

- The application layer safeguards technological interfaces using controls such as multi-factor authentication (MFA).
- The data layer protects critical information—such as personally identifiable information—using controls like asset classification.


📝 Executive Summary

Information security requires a global, collaborative effort that extends far beyond individual security teams. A key part of this collective defense involves tracking and sharing information via online public libraries. While often grouped together, **vulnerabilities** and **exposures** have distinct definitions: a vulnerability is an inherent weakness within a system, whereas an exposure is a operational mistake or condition that allows a threat to exploit that system.

The most prominent library for documenting these flaws is the **CVE List**, an openly accessible dictionary created in 1999 by the MITRE Corporation—a US government-sponsored, non-profit research collective. The CVE list standardizes how security issues are identified and categorized worldwide. Anyone can report a flaw, but before it receives an official CVE ID, it must undergo strict evaluation by a **CVE Numbering Authority (CNA)**. To qualify for an ID, a submission must pass four specific criteria:

1. **Independence:** The issue must be fixable on its own without requiring a fix to a separate problem.
2. **Recognition:** The reporter must formally acknowledge it as a potential security risk.
3. **Evidence:** The submission must include supporting proof.
4. **Single Codebase Impact:** The flaw must only affect one program's source code (e.g., affecting Chrome for desktop but not Chrome for Android).

Once a vulnerability is assigned an ID, other public databases review it to assess its severity. A primary example is the **NIST National Vulnerability Database (NVD)**, which uses the **Common Vulnerability Scoring System (CVSS)**. CVSS applies a static base score ranging from 0 to 10 to measure a flaw's impact at the time of evaluation. Scores below 4.0 are deemed low risk and do not need immediate remediation, while scores above 9.0 indicate critical risks requiring urgent intervention. Security teams leverage these standardized CVE entries and CVSS scores to architect their vulnerability management strategies, allowing them to prioritize critical patches and efficiently safeguard company assets.

---

📖 Abbreviations & Key Terms Reference List

**Core Concepts**

- **Vulnerability:** An inherent weakness or flaw within a system's structure or software.
- **Exposure:** A mistake or environmental condition that opens a system up to being exploited by a threat.
- **Base Score:** A static rating on a scale of 0 to 10 that reflects the severity of a vulnerability at the exact moment it is evaluated.

**Organizations & Authorities**

- **MITRE Corporation:** A US government-sponsored collection of non-profit research and development centers that created the CVE list in 1999.
- **CNA (CVE Numbering Authority):** Volunteer organizations with proven security expertise that analyze, verify, and distribute information on eligible CVE submissions.
- **NIST (National Institute of Standards and Technology):** The organization responsible for maintaining the National Vulnerability Database.

**Systems & Frameworks**

- **CVE (Common Vulnerabilities and Exposures):** An openly accessible, standardized public dictionary used to identify and categorize known security flaws.
- **NVD (National Vulnerability Database):** A popular public database (maintained by NIST) that reviews CVEs and calculates their severity scores.
- **CVSS (Common Vulnerability Scoring System):** The standardized measurement framework used to score the impact and severity of a vulnerability on a scale from 0 to 10.


## What is OWASP?

OWASP is a nonprofit foundation that works to improve the security of software. OWASP is an open platform that security professionals from around the world use to share information, tools, and events that are focused on securing the web.

## The OWASP Top 10

One of OWASP’s most valuable resources is the OWASP Top 10. The organization has published this list since 2003 as a way to spread awareness of the web’s most targeted vulnerabilities. The Top 10 mainly applies to new or custom made software. Many of the world's largest organizations reference the OWASP Top 10 during application development to help ensure their programs address common security mistakes.

**Pro tip:** OWASP’s Top 10 is updated every few years as technologies evolve. Rankings are based on how often the vulnerabilities are discovered and the level of risk they present.

**Note:** Auditors also use the OWASP Top 10 as one point of reference when checking for regulatory compliance.

## Common vulnerabilities

Businesses often make critical security decisions based on the vulnerabilities listed in the OWASP Top 10. This resource influences how businesses design new software that will be on their network, unlike the CVE® list, which helps them identify improvements to existing programs. These are the most regularly listed vulnerabilities that appear in their rankings to know about:

### **Broken access control**

Access controls limit what users can do in a web application. For example, a blog might allow visitors to post comments on a recent article but restricts them from deleting the article entirely. Failures in these mechanisms can lead to unauthorized information disclosure, modification, or destruction. They can also give someone unauthorized access to other business applications.

### **Cryptographic failures**

Information is one of the most important assets businesses need to protect. Privacy laws such as General Data Protection Regulation (GDPR) require sensitive data to be protected by effective encryption methods. Vulnerabilities can occur when businesses fail to encrypt things like personally identifiable information (PII). For example, if a web application uses a weak hashing algorithm, like MD5, it’s more at risk of suffering a data breach.

### **Injection**

Injection occurs when malicious code is inserted into a vulnerable application. Although the app appears to work normally, it does things that it wasn’t intended to do. Injection attacks can give threat actors a backdoor into an organization’s information system. A common target is a website’s login form. When these forms are vulnerable to injection, attackers can insert malicious code that gives them access to modify or steal user credentials.

### **Insecure design**

Applications should be designed in such a way that makes them resilient to attack. When they aren’t, they’re much more vulnerable to threats like injection attacks or malware infections. Insecure design refers to a wide range of missing or poorly implemented security controls that should have been programmed into an application when it was being developed.

### **Security misconfiguration**

Misconfigurations occur when security settings aren’t properly set or maintained. Companies use a variety of different interconnected systems. Mistakes often happen when those systems aren’t properly set up or audited. A common example is when businesses deploy equipment, like a network server, using default settings. This can lead businesses to use settings that fail to address the organization's security objectives.

### **Vulnerable and outdated components**

Vulnerable and outdated components is a category that mainly relates to application development. Instead of coding everything from scratch, most developers use open-source libraries to complete their projects faster and easier. This publicly available software is maintained by communities of programmers on a volunteer basis. Applications that use vulnerable components that have not been maintained are at greater risk of being exploited by threat actors.

### **Identification and authentication failures**

Identification is the keyword in this vulnerability category. When applications fail to recognize who should have access and what they’re authorized to do, it can lead to serious problems. For example, a home Wi-Fi router normally uses a simple login form to keep unwanted guests off the network. If this defense fails, an attacker can invade the homeowner’s privacy.

### **Software and data integrity failures**

Software and data integrity failures are instances when updates or patches are inadequately reviewed before implementation. Attackers might exploit these weaknesses to deliver malicious software. When that occurs, there can be serious downstream effects. Third parties are likely to become infected if a single system is compromised, an event known as a supply chain attack.

A famous example of a supply chain attack is the [SolarWinds cyber attack (2020)](https://www.gao.gov/blog/solarwinds-cyberattack-demands-significant-federal-and-private-sector-response-infographic) where hackers injected malicious code into software updates that the company unknowingly released to their customers.

### **Security logging and monitoring failures**

In security, it’s important to be able to log and trace back events. Having a record of events like user login attempts is critical to finding and fixing problems. Sufficient monitoring and incident response is equally important.

### **Server-side request forgery**

Companies have public and private information stored on web servers. When you use a hyperlink or click a button on a website, a request is sent to a server that should validate who you are, fetch the appropriate data, and then return it to you.

![A hacker using their victim's computer to steal data from a web server.](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/DT6Hkom1RcugsbS_BFgoLw_7e1de97a7fdc4e00b7c9b14a0ad5fcf1_Pfjezx6XbwJ_TAJbWmCtLMpwLX4YD2xP4Se2IJeQF4uTD6BtqNXlcpObbGHeJuzWlxr5A3WqWQzZ2K6E6kFLsX3JiI0bxbFtWFjNUCxZTgs7ftpEgVEcI87zvBxN2flvGXs37nW_RJF0QVLY7798FCXB9pAH_uUI3zYLbXOiOpGcdhti00aMTwl7xbMFpg?expiry=1793654485700&hmac=6r0b5K64El_kJFTYT34YBFIZhYAHFY0DMzjqHiCqBmI)

Server-side request forgeries (SSRFs) are when attackers manipulate the normal operations of a server to read or update other resources on that server. These are possible when an application on the server is vulnerable. Malicious code can be carried by the vulnerable app to the host server that will fetch unauthorized data.\


## Information vs intelligence
_Information_ refers to the collection of raw data or facts about a specific subject. _Intelligence_, on the other hand, refers to the analysis of information to produce knowledge or insights that can be used to support decision-making.

For example, new information might be released about an update to the operating system (OS) that's installed on your organization's workstations. Later, you might find that new cyber threats have been linked to this new update by researching multiple cybersecurity news resources. The analysis of this information can be used as intelligence to guide your organization's decision about installing the OS updates on employee workstations.

OSINT plays a significant role in **information security (InfoSec)**, which is the practice of keeping data in all states away from unauthorized users.

They might utilize OSINT to monitor online forums and hacker communities for discussions about emerging vulnerabilities. If they come across a forum post discussing a newly discovered weakness in a popular software that the company uses, the team can quickly assess the risk, prioritize patching efforts, and implement necessary safeguards to prevent an attack.

Here are some of the ways OSINT can be used to generate intelligence:

- To provide insights into cyber attacks
    
- To detect potential data exposures
    
- To evaluate existing defenses
    
- To identify unknown vulnerabilities
    

Collecting intelligence is sometimes part of the vulnerability management process. Security teams might use OSINT to develop profiles of potential targets and make data driven decisions on improving their defenses.

Several tools also exist that can be used in your intelligence gathering process. Here are just a few examples of tools that you can explore:

- [VirusTotal](https://www.virustotal.com/gui/home/upload) is a service that allows anyone to analyze suspicious files, domains, URLs, and IP addresses for malicious content.
    
- [MITRE ATT&CK®](https://attack.mitre.org/) is a knowledge base of adversary tactics and techniques based on real-world observations.
    
- [OSINT Framework](https://osintframework.com/) is a web-based interface where you can find OSINT tools for almost any kind of source or platform.
    
- [Have I been Pwned](https://haveibeenpwned.com/) is a tool that can be used to search for breached email accounts.


A **vulnerability assessment** is an internal review process that organizations use to evaluate their security systems, identify weak points, and prevent potential attacks before they occur. It also serves as a benchmark to ensure that existing security controls comply with regulatory standards. While it mirrors the global CVE process of identifying and scoring flaws, a vulnerability assessment is entirely managed internally by an organization's own security analysts and IT teams.

Because organizations manage a massive volume of digital assets, security teams must strategically choose which areas to focus on. Once a target area is selected, the assessment follows a structured, four-step lifecycle:

1. **Identification:** The team uses scanning tools and manual testing to capture the current state of the security system. This phase typically uncovers a large volume of findings.
2. **Vulnerability Analysis:** Analysts act as digital detectives to investigate the findings, testing each vulnerability to uncover the root source of the problem.
3. **Risk Assessment:** To manage the fact that discovered flaws usually outnumber available personnel, a severity score is assigned to each vulnerability. This score is calculated using two primary factors: the potential severity of the impact and the likelihood of exploitation. This allows the organization to prioritize its limited resources effectively.
4. **Remediation:** In this final phase, the security staff and IT teams collaborate to resolve the prioritized flaws. Remediation efforts are tailored to the severity scores and commonly include updating operating systems, enforcing new security procedures, or deploying system patches.

---

🔀 The 4-Step Vulnerability Assessment Process

Because this framework relies on a sequential lifecycle, the steps must be executed in order:

1. **Identification:** Utilizing scanning tools and manual testing to find flaws and document the current state of the security system.
2. **Vulnerability Analysis:** Investigating the identified vulnerabilities to locate the root cause or source of the issue.
3. **Risk Assessment:** Scoring and prioritizing the vulnerabilities based on their impact severity and likelihood of exploitation.
4. **Remediation:** Collaborating with IT teams to address, patch, or fix the prioritized flaws.


## What is a vulnerability scanner?
A **vulnerability scanner** is software that automatically compares known vulnerabilities and exposures against the technologies on the network. In general, these tools scan systems to find misconfigurations or programming flaws.

Scanning tools are used to analyze each of the five attack surfaces that you learned about in [the video about the defense in depth strategy](https://www.coursera.org/learn/assets-threats-and-vulnerabilities/lecture/IdvXj/defense-in-depth-strategy):
- **Perimeter layer**, like authentication systems that validate user access
    
- **Network layer**, which is made up of technologies like network firewalls and others
    
- **Endpoint layer**, which describes devices on a network, like laptops, desktops, or servers
    
- **Application layer**, which involves the software that users interact with
    
- **Data layer**, which includes any information that’s stored, in transit, or in use

When a scan of any layer begins, the scanning tool compares the findings against databases of security threats. At the end of the scan, the tool flags any vulnerabilities that it finds and adds them to its reference database. Each scan adds more information to the database, helping the tool be more accurate in its analysis.

**Note:** Vulnerability databases are also routinely updated by the company that designed the scanning software.

## Performing scans

Vulnerability scanners are meant to be non-intrusive.
**Note:** While vulnerability scanners are non-intrusive, there are instances when a scan can inadvertently cause issues, like crash a system.
**External vs. internal**

External and internal scans simulate an attacker's approach.

_External scans_ test the perimeter layer outside of the internal network. They analyze outward facing systems, like websites and firewalls. These kinds of scans can uncover vulnerable things like vulnerable network ports or servers.

_Internal scans_ start from the opposite end by examining an organization's internal systems. For example, this type of scan might analyze application software for weaknesses in how it handles user input.

### **Authenticated vs. unauthenticated**

Authenticated and unauthenticated scans simulate whether or not a user has access to a system.

_Authenticated scans_ might test a system by logging in with a real user account or even with an admin account. These service accounts are used to check for vulnerabilities, like broken access controls.

_Unauthenticated scans_ simulate external threat actors that do not have access to your business resources. For example, a scan might analyze file shares within the organization that are used to house internal-only documents. Unauthenticated users should receive "access denied" results if they tried opening these files. However, a vulnerability would be identified if you were able to access a file.

### **Limited vs. comprehensive**

Limited and comprehensive scans focus on particular devices that are accessed by internal and external users.

_Limited scans_ analyze particular devices on a network, like searching for misconfigurations on a firewall.

_Comprehensive scans_ analyze all devices connected to a network. This includes operating systems, user databases, and more.

**Pro tip:** Discovery scanning should be done prior to limited or comprehensive scans. Discovery scanning is used to get an idea of the computers, devices, and open ports that are on a network.

## Patching gaps in security
A **patch update** is a software and operating system update that addresses security vulnerabilities within a program or product. Patches usually contain bug fixes that address common security vulnerabilities and exposures.
**Note:** Ideally, patches address common vulnerabilities and exposures before malicious hackers find them. However, patches are sometimes developed as a result of a **zero-day**, which is an exploit that was previously unknown.

## Common update strategies


- Manual updates
### **Manual updates**

A manual deployment strategy relies on IT departments or users obtaining updates from the developers. Home office or small business environments might require you to find, download, and install updates yourself. In enterprise settings, the process is usually handled with a configuration management tool. T

**Advantage:** An advantage of manual update deployment strategies is control. That can be useful if software updates are not thoroughly tested by developers, leading to instability issues.

**Disadvantage:** A drawback to manual update deployments is that critical updates can be forgotten or disregarded entirely.
- Automatic updates
### **Automatic updates**

An automatic deployment strategy takes the opposite approach. With this option, finding, downloading, and installing updates can be done by the system or application.
**Pro tip:** The Cybersecurity and Infrastructure Security Agency (CISA) recommends using automatic options whenever they’re available.

Certain permissions need to be enabled by users or IT groups before updates can be installed, or pushed, when they're available. It is up to the developers to adequately test their patches before release.

**Advantage:** An advantage to automatic updates is that the deployment process is simplified. It also keeps systems and software current with the latest, critical patches.

**Disadvantage:** A drawback to automatic updates is that instability issues can occur if the patches were not thoroughly tested by the vendor. This can result in performance problems and a poor user experience.

## End-of-life software

Sometimes updates are not available for a certain type of software known as end-of-life (EOL) software. All software has a lifecycle. It begins when it’s produced and ends when a newer version is released. At that point, developers must allocate resources to the newer versions, which leads to EOL software. While the older software is still useful, the manufacturer no longer supports it.