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