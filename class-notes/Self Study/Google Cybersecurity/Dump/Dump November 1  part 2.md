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