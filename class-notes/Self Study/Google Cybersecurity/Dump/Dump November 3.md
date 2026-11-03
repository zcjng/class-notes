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