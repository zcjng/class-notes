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