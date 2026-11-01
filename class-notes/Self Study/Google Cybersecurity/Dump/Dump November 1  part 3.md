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