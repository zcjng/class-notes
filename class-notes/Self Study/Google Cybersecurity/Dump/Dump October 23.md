## Question

What are the elements of security risk planning? Select three answers.

Threats

Correct

Security risk planning involves the analysis of three elements: assets, threats, and vulnerabilities. A threat is any circumstance or event that can negatively affect assets, such as a burglar stealing money from a cash register.
**Threats** are circumstances or events that can negatively impact assets. There are many different types of threats. However, they are commonly categorized as two types: intentional and unintentional.
For example, an _intentional_ threat might be a malicious hacker who gains access to sensitive information by targeting a misconfigured application. An _unintentional_ threat might be an employee who holds the door open for an unknown person and grants them access to a restricted area. Either one can cause an event that must be responded to.

Assets

Correct

Security risk planning involves the analysis of three elements: assets, threats, and vulnerabilities. An asset is an item perceived as having value to an organization, such as a cash register and the money inside it.

Vulnerabilities

Correct

Security risk planning involves the analysis of three elements: assets, threats, and vulnerabilities. A vulnerability is a weakness that can be exploited by a threat, such as an unlocked door to a restricted area.
**Vulnerabilities** are weaknesses that can be exploited by threats. There’s a wide range of vulnerabilities, but they can be grouped into two categories: technical and human.

For example, a _technical_ vulnerability can be misconfigured software that might give an unauthorized person access to important data. A _human_ vulnerability can be a forgetful employee who loses their access card in a parking lot. Either one can lead to risk.


a **risk** is anything that can impact the confidentiality, integrity, or availability of an asset. Since organizations have particular assets that they value, they tend to differ in how they interpret and approach risk.

One way to interpret risk is to consider the potential effects that negative events can have on a business. Another way to present this idea is with this calculation:

**Likelihood x Impact = Risk**

# Asset Management & Classification

> [!quote] Core Philosophy _"You can only protect what you know you have."_

## 1. Overview & Importance

**Asset management** is the systematic process of identifying, tracking, and managing the risks affecting organizational assets. To design an effective security perimeter and keep operational friction low, organizations must maintain an accurate inventory.

### Key Asset Types

|   |   |   |
|---|---|---|
|**Category**|**Description**|**Examples**|
|**Digital Assets**|Electronic data and records|Customer PII, financial ledgers, transactional logs|
|**Information Systems**|Processing environments & infrastructure|Networks, cloud infrastructure, enterprise software|
|**Physical Assets**|Tangible property and gear|Data centers, office facilities, laptops, servers|
|**Intangible Assets**|Non-physical organizational value|Intellectual property (IP), brand reputation, trade secrets|

## 2. Fundamentals of Asset Classification

Asset classification is the practice of labeling assets based on their **sensitivity** and **criticality** to the organization.

### The 4 Core Questions of Classification

An organization must answer four foundational questions for every asset:

1. **What do you have?** _(Identity & description)_
    
2. **Where is it?** _(Physical location, host, or cloud storage region)_
    
3. **Who owns it?** _(Accountability and governance)_
    
4. **How important is it?** _(Business impact analysis / criticality)_
    

> [!tip] Business Benefits
> 
> - **Risk Prioritization:** Focuses high-cost defensive resources on the most critical targets.
>     
> - **Cost Optimization:** Prevents over-spending on protecting low-risk assets.
>     
> - **Regulatory Compliance:** Satisfies audit and legal frameworks (e.g., ISO/IEC 27001, SOC 2, HIPAA).
>     

## 3. Standard Classification Schemes

```
▲ Restricted      (Need-to-know / Highest Sensitivity)
│ Confidential    (Significant business damage if leaked)
│ Internal-Only   (Employees & trusted business partners)
▼ Public          (Zero impact if disclosed)
```

|   |   |   |   |
|---|---|---|---|
|**Classification Level**|**Sensitivity**|**Access Scope**|**Impact of Breach / Disclosure**|
|**🔴 Restricted**|Highest|Strictly "Need-to-Know"|Catastrophic damage or extreme legal liability.|
|**🟠 Confidential**|High|Authorized personnel|Significant negative financial, reputational, or operational impact.|
|**🟡 Internal-Only**|Moderate|Employees & authorized partners|Minor operational disruption; not meant for public viewing.|
|**🟢 Public**|Lowest|General public|Negligible / No negative consequences.|

> [!note] Sector Variability Commercial naming schemes differ from government sectors. In government frameworks (e.g., US DoD), classifications follow schemes like _Confidential_, _Secret_, and _Top Secret_, where _Confidential_ is often the lowest classified tier rather than an intermediate one.

## 4. Key Classification Challenges

> [!warning] Friction Points in Practice

### A. Ambiguous Asset Ownership

- Ownership of physical infrastructure (like real estate or main servers) is clear, but personal computing devices blur lines.
    
- **Example:** A corporate laptop assigned to a remote worker containing both proprietary company code and the employee's personal documents/photos.
    

### B. Mixed Classification Data (Data Aggregation)

- Single documents or payloads often combine varying levels of sensitivity.
    
- **Example:** A postal delivery or billing statement:
    
    - _Recipient Name:_ Public / low sensitivity.
        
    - _Mailing Address / Account Balance:_ Confidential / restricted.



# Cloud Computing & Cloud Security

> [!quote] Definition: Cloud Computing (UK NCSC) _"An on-demand, massively scalable service, hosted on shared infrastructure, accessible via the internet."_

## 1. The Cloud Computing Shift

Historically, launching digital products required building and maintaining expensive, on-premises back-end infrastructure.

### Core Drivers of Cloud Adoption

- **Scalability:** Scale workloads rapidly up or down based on traffic.
    
- **Agility:** Accelerate development life cycles and deploy instantly.
    
- **Cost Efficiency:** Shift from upfront capital expenditure (CapEx) on physical servers to flexible operational expenditure (OpEx).
    
- **The Trade-off:** While simplifying operations, cloud environments introduce complex shared-tenancy attack surfaces and distributed governance risks.
    

## 2. Cloud Service Models

Cloud solutions generally fall into three service tiers depending on the level of customer control vs. provider management:

```
▲ User Control
│
│  [ IaaS ]  Infrastructure (Virtual machines, networking, raw storage)
│  [ PaaS ]  Platform (Runtime environments, developer build tools)
│  [ SaaS ]  Software (Turnkey applications delivered via browser)
▼
```

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|**Model**|**Full Name**|**Primary Focus**|**Customer Responsibility**|**Provider Responsibility**|**Representative Examples**|
|**SaaS**|Software as a Service|End-user applications|Data access policies, user credentials, end-point devices|App uptime, patching, runtime, infrastructure|Gmail, Slack, Zoom|
|**PaaS**|Platform as a Service|Application development & hosting|Application code, data, app-level configurations|Hardware, OS, runtime environment, network security|Google App Engine, Heroku, VMware Cloud Foundry|
|**IaaS**|Infrastructure as a Service|Foundational infrastructure|Operating systems, runtime, firewall configs, storage, apps|Physical hardware, physical data center security, virtualization hypervisors|AWS EC2, Google Cloud Platform (Compute), Azure VMs|

## 3. The Shared Responsibility Model

Moving to the cloud replaces traditional perimeter defense with a partnership model between the **Cloud Service Provider (CSP)** and the **Customer**.

```
Traditional (On-Prem)           Cloud (Shared Model)
┌───────────────────────┐       ┌───────────────────────┐
│ Everything managed by │  ───▶ │ Customer: Data / IAM  │
│ customer's internal   │       ├───────────────────────┤
│ security team         │       │ CSP: Infrastructure   │
└───────────────────────┘       └───────────────────────┘
```

### Customer Scope (Direct Control)

- **Identity & Access Management (IAM):** Who gets access to which resource and under what conditions.
    
- **Resource Configuration:** Hardening network security groups, object storage buckets, and access ports.
    
- **Data Security & Handling:** Encryption at rest, encryption in transit, data retention, and classification.
    

> [!tip] Rule of Thumb The amount of security responsibility delegated to the CSP increases as you move from **IaaS** $\rightarrow$ **PaaS** $\rightarrow$ **SaaS**. However, **data classification, identity management, and credential protection always remain the customer's responsibility.**

## 4. Key Cloud Security Challenges

> [!warning] Critical Vulnerabilities & Risks

1. **Misconfiguration (Primary Threat Vector):**
    
    - Relying on default "out-of-the-box" settings that leave public write/read permissions enabled (e.g., exposed storage buckets).
        
2. **Cloud-Native Breaches:**
    
    - Exploits targeting misconfigured IAM roles, API gateways, and cloud service metadata endpoints.
        
3. **Visibility & Access Monitoring:**
    
    - Distributed workloads make it challenging to maintain unified log ingestion and behavioral auditing across hybrid/multi-cloud setups.
        
4. **Regulatory & Compliance Demands:**
    
    - Auditing cross-border data residency and privacy mandates (e.g., **GDPR**, **HIPAA**, **PCI DSS**) in distributed multi-tenant environments.
        

