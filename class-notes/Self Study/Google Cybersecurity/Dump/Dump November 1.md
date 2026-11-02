# Course 5 Module 2

Public Key Infrastructure (PKI) is an encryption framework designed to secure online communication by managing encryption methods and establishing trust between parties.

Encryption Methods Used in PKI

- Asymmetric encryption utilizes a public key to encrypt data and a private key to decrypt it, offering high security but slower speeds.
- Symmetric encryption uses a single shared secret key for both encryption and decryption, providing faster communication at the expense of lower security.

How PKI Combines Both Types

- Applications often balance speed and security by starting connections with asymmetric encryption before switching to symmetric encryption.
- Both methods face the shared vulnerability of needing a reliable way to verify the identity of the person or system receiving the keys.

Establishing Trust with Digital Certificates

- PKI resolves key management and trust issues through digital certificates, which function as digital ID badges verifying the identity of a public key holder.
- A trusted Certificate Authority (CA) verifies an organization's details, signs the certificate with its own private key, and validates authenticity.

Previously, you learned these terms: 

- **Encryption**: the process of converting data from a readable format to an encoded format
    
- **Public key infrastructure** (PKI):  an encryption framework that secures the exchange of online information
    
- **Cipher**: an algorithm that encrypts information
    

All digital information deserves to be kept private, safe, and secure. Encryption is one key to doing that! It is useful for transforming information into a form that unintended recipients cannot understand. In this reading, you’ll compare symmetric and asymmetric encryption and learn about some well-known algorithms for each.

## Types of encryption

There are two main types of encryption:

- **Symmetric encryption** is the use of a single secret key to exchange information. Because it uses one key for encryption and decryption, the sender and receiver must know the secret key to lock or unlock the cipher.
    
- **Asymmetric encryption** is the use of a public and private key pair for encryption and decryption of data. It uses two separate keys: a public key and a private key. The public key is used to encrypt data, and the private key decrypts it. The private key is only given to users with authorized access.
    

## The importance of key length

Ciphers are vulnerable to **brute force attacks**, which use a trial and error process to discover private information. This tactic is the digital equivalent of trying every number in a combination lock trying to find the right one. In modern encryption, longer key lengths are considered to be more secure. Longer key lengths mean more possibilities that an attacker needs to try to unlock a cipher.

One drawback to having long encryption keys is slower processing times. Although short key lengths are generally less secure, they’re much faster to compute. Providing fast data communication online while keeping information safe is a delicate balancing act. 

## Approved algorithms

Many web applications use a combination of symmetric and asymmetric encryption. This is how they balance user experience with safeguarding information. As an analyst, you should be aware of the most widely-used algorithms.

### **Symmetric algorithms**

- _Triple DES (3DES)_ is known as a block cipher because of the way it converts plaintext into ciphertext in “blocks.” Its origins trace back to the Data Encryption Standard (DES), which was developed in the early 1970s. DES was one of the earliest symmetric encryption algorithms that generated 64-bit keys, although only 56 bits are used for encryption. A **bit** is the smallest unit of data measurement on a computer. As you might imagine, Triple DES generates keys that are three times as long. Triple DES applies the DES algorithm three times, using three different 56-bit keys. This results in an effective key length of 168 bits. Despite the longer keys, many organizations are moving away from using Triple DES due to limitations on the amount of data that can be encrypted. However, Triple DES is likely to remain in use for backwards compatibility purposes.   
    
- _Advanced Encryption Standard (AES)_ is one of the most secure symmetric algorithms today. AES generates keys that are 128, 192, or 256 bits. Cryptographic keys of this size are considered to be safe from brute force attacks. It’s estimated that brute forcing an AES 128-bit key could take a modern computer billions of years!
    

### **Asymmetric algorithms**

- _Rivest Shamir Adleman (RSA)_ is named after its three creators who developed it while at the Massachusetts Institute of Technology (MIT). RSA is one of the first asymmetric encryption algorithms that produces a public and private key pair. Asymmetric algorithms like RSA produce even longer key lengths. In part, this is due to the fact that these functions are creating two keys. RSA key sizes are 1,024, 2,048, or 4,096 bits. RSA is mainly used to protect highly sensitive data.
    
- _Digital Signature Algorithm (DSA)_ is a standard asymmetric algorithm that was introduced by NIST in the early 1990s. DSA also generates key lengths of 2,048 bits. This algorithm is widely used today as a complement to RSA in public key infrastructure.
    

### **Generating keys**

These algorithms must be implemented when an organization chooses one to protect their data. One way this is done is using OpenSSL, which is an open-source command line tool that can be used to generate public and private keys. OpenSSL is commonly used by computers to verify digital certificates that are exchanged as part of public key infrastructure.

**Note:** OpenSSL is just one option. There are various others available that can generate keys with any of these common algorithms. 

In early 2014, OpenSSL disclosed a vulnerability, known as the [Heartbleed bug](https://en.wikipedia.org/wiki/Heartbleed), that exposed sensitive data in the memory of websites and applications. Although unpatched versions of OpenSSL are still available, the Heartbleed bug was patched later that year (2014). Many businesses today use the secure versions of OpenSSL to generate public and private keys, demonstrating the importance of using up-to-date software.

## Obscurity is not security

In the world of cryptography, a cipher must be proven to be unbreakable before claiming that it is secure. According to [Kerckhoff’s principle](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle), cryptography should be designed in such a way that all the details of an algorithm—except for the private key—should be knowable without sacrificing its security. For example, you can access all the details about how AES encryption works online and yet it is still unbreakable.

Occasionally, organizations implement their own, custom encryption algorithms. There have been instances where those secret cryptographic systems have been quickly cracked after being made public.

**Pro tip:** A cryptographic system _should not_ be considered secure if it requires secrecy around how it works.

### Encryption is everywhere

Companies use both symmetric and asymmetric encryption. They often work as a team, balancing security with user experience.

For example, websites tend to use asymmetric encryption to secure small blocks of data that are important. Usernames and passwords are often secured with asymmetric encryption while processing login requests. Once a user gains access, the rest of their web session often switches to using symmetric encryption for its speed.

Using data encryption like this is increasingly required by law. Regulations like the Federal Information Processing Standards (FIPS 140-3) and the General Data Protection Regulation (GDPR) outline how data should be collected, used, and handled. Achieving compliance with either regulation is critical to demonstrating to business partners and governments that customer data is handled responsibly.

## Key takeaways

Knowing the basics of encryption is important for all security professionals. Symmetric encryption relies on a single secret key to protect data. On the other hand, asymmetric uses a public and private key pair. Their encryption algorithms create different key sizes. Both types of encryption are used to meet compliance regulations and protect data online.




Hash Functions and Unique Values

- Unlike symmetric or asymmetric encryption, a hash function is a one-way process that produces a fixed, unique identifier known as a hash value or digest instead of generating decryption keys.
- Even the slightest alteration to an original file—such as changing a single line of code—will produce an entirely different hash value, exposing unauthorized modifications.

Data Integrity and Non-Repudiation

- Hashes are primarily used to confirm data integrity by verifying the accuracy and consistency of files and applications.
- They establish non-repudiation by ensuring that the authenticity of information cannot be denied once validated against its original hash.

Practical Application in Security

- Analysts can generate hash values using common algorithms (like MD5 or SHA-256) via command-line tools to check files on their systems.
- Generated hashes can be cross-referenced against databases of known malicious software, such as VirusTotal, to detect threats.



One of the earliest hash functions is Message Digest 5, more commonly known as MD5. Professor Ronald Rivest of the Massachusetts Institute of Technology (MIT) developed MD5 in the early 1990s as a way to verify that a file sent over a network matched its source file.
MD5 works by converting data into a 128-bit value.


### **Hash collisions**

One of the flaws in MD5 happens to be a characteristic of all hash functions. Hash algorithms map any input, regardless of its length, into a fixed-size value of letters and numbers.
MD5 values are limited to 32 characters in length. Due to the limited output size, the algorithm is considered to be vulnerable to **hash collision**, an instance when different inputs produce the same hash value. Because hashes are used for authentication, a hash collision is similar to copying someone’s identity. Attackers can carry out collision attacks to fraudulently impersonate authentic data.MD5's shortcomings gave way to a new group of functions known as the Secure Hashing Algorithms, or SHAs.

Numbers besides each SHA function indicate the size of its hash value in bits. Except for SHA-1, which produces a 160-bit digest, (NIST) approves each of these algorithms.

**Five functions make up the SHA family of algorithms:**
- SHA-1
- SHA-224
- SHA-256
- SHA-384
- SHA-512

## Secure password storage

Passwords are typically stored in a database where they are mapped to a username. The server receives a request for authentication that contains the credentials supplied by the user. It then looks up the username in the database and compares it with the password that was provided and verifies that it matches before granting them access.

This is a safe system unless an attacker gains access to the user database. If passwords are stored in plaintext, then an attacker can steal that information and use it to access company resources. Hashing adds an additional layer of security. Because hash values can't be reversed, an attacker would not be able to steal someone's login credentials if they managed to gain access to the database.

A **rainbow table** is a file of pre-generated hash values and their associated plaintext. They’re like dictionaries of weak passwords. Attackers capable of obtaining an organization’s password database can use a rainbow table to compare them against all possible values.

**Salting** is an additional safeguard that's used to strengthen hash functions. A _salt_ is a random string of characters that's added to data before it's hashed. The additional characters produce a more unique hash value, making salted data resilient to rainbow table attacks.

## Access Controls
Maintain CIA 
system are broken down to AAA

# 1 Authentication, 


Authentication Factors

- **Knowledge**, or something the user knows
    
- **Ownership**, or something the user possesses
    
- **Characteristic**, or something the user is

### Single Sign-On (SSO)

- SSO streamlines user access by consolidating multiple logins into a single identity verification step.
- Relying on SSO alone creates significant security risks if it depends only on a single authentication factor.
**Single sign-on** (SSO) is a technology that combines several different logins into one. More companies are turning to SSO as a solution to their authentication needs for three reasons:

1. **SSO improves the user experience** by eliminating the number of usernames and passwords people have to remember.
    
2. **Companies can lower costs** by streamlining how they manage connected services.
    
3. **SSO improves overall security** by reducing the number of access points attackers can target.
## How SSO works

SSO works by automating how trust is established between a user and a service provider. Rather than placing the responsibility on an employee or customer, SSO solutions use trusted third-parties to prove that a user is who they claim to be. This is done through the exchange of encrypted access tokens between the identity provider and the service provider.

Similar to other kinds of digital information, these access tokens are exchanged using specific protocols. SSO implementations commonly rely on two different authentication protocols: LDAP and SAML. LDAP, which stands for Lightweight Directory Access Protocol, is mostly used to transmit information on-premises; SAML, which stands for Security Assertion Markup Language, is mostly used to transmit information off-premises, like in the cloud.

**Note:** LDAP and SAML protocols are often used together.

### MFA builds on the benefits of SSO.
It works by having users prove that they are who they claim to be. The user must provide two factors (2FA) or three factors (3FA) to authenticate their identification. The MFA process asks users to provide these proofs, such as:

- **Something a user knows:** most commonly a username and password
    
- **Something a user has:** normally received from a service provider, like a one-time passcode (OTP) sent via SMS
    
- **Something a user is:** refers to physical characteristics of a user, like their fingerprints or facial scans



# 2 authorization 
Core Authorization Principles

- The principle of least privilege ensures that access to information and resources is granted only to the extent and duration needed.
- Separation of duties distributes critical responsibilities across multiple individuals or systems to reduce the risk of misuse and errors.
In other words, least privilege _limits_ _the access_ that an individual receives, while separation of duties _divides responsibilities_ among multiple people to prevent any one person from having too much control.




HTTP Basic Auth and HTTPS

- HTTP Basic Auth transmits usernames and passwords as plain text across the network, leaving credentials vulnerable to interception.
- HTTPS protects sensitive data by encrypting transmissions over the network to prevent unauthorized access.

OAuth and API Tokens

- OAuth is an open-standard authorization protocol that allows applications to share designated access without exchanging actual user passwords.
- API tokens are encrypted blocks of code containing identity and permission details that verify access securely between devices and servers.

And if you want to continue exploring this topic, try one of these follow-up questions:


# 3 Accounting framework

Accounting monitors access logs to track user sessions, uncover security incidents, and protect systems from unauthorized activity.

The Role of Accounting and Access Logs

- Accounting is the practice of tracking access logs, which record who accessed a system, when, and what resources were used.
- Security analysts use these logs as a primary tool to identify trends, investigate security breaches, and detect unauthorized access.

Sessions, IDs, and Cookies

- A session begins when a user accesses a system, generating a unique session ID that identifies the user and device until logout or timeout.
- Session cookies are exchanged between the device and server to validate the session and securely manage data without repeatedly sending sensitive credentials.

Session Hijacking Risks

- Attackers can commit session hijacking by stealing a user's session ID or cookie to impersonate them.
- Monitoring access logs helps analysts identify the unusual activity caused by stolen session tokens and protect sensitive organizational assets.


## Identity and access management (IAM)

**Identity and access management** (IAM) is a collection of processes and technologies that helps organizations manage digital identities in their environment. Both AAA and IAM systems are designed to authenticate users, determine their access privileges, and track their activities within a system.

Either model used by your organization is more than a single, clearly defined system. They each consist of a collection of security controls that ensure the _right user_ is granted access to the _right resources_ at the _right time_ and for the _right reasons_. Each of those four factors is determined by your organization's policies and processes.

**Note:** A user can either be a person, a device, or software.


### **User provisioning**

Back-end systems need to be able to verify whether the information provided by a user is accurate. To accomplish this, users must be properly provisioned. **User provisioning** is the process of creating and maintaining a user's digital identity.

**Pro tip:** Another role analysts have in IAM is to deprovision users. This is an important practice that removes a user's access rights when they should no longer have them.


If the right user has been authenticated, the network should ensure the right resources are made available. There are three common frameworks that organizations use to handle this step of IAM:

- Mandatory access control (MAC)
    ### **Mandatory Access Control (MAC)**

MAC is the strictest of the three frameworks. Authorization in this model is based on a strict need-to-know basis. Access to information must be granted manually by a central authority or system administrator. For example, MAC is commonly applied in law enforcement, military, and other government agencies where users must request access through a chain of command. MAC is also known as non-discretionary control because access isn’t given at the discretion of the data owner.


- Discretionary access control (DAC)
- ### **Discretionary Access Control (DAC)**

DAC is typically applied when a data owner decides appropriate levels of access. One example of DAC is when the owner of a Google Drive folder shares editor, viewer, or commentor access with someone else.
    
- Role-based access control (RBAC)
- ### **Role-Based Access Control (RBAC)**

RBAC is used when authorization is determined by a user's role within an organization. For example, a user in the marketing department may have access to user analytics but not network administration.

[IDPro](https://idpro.org/)© is a professional organization dedicated to sharing essential IAM industry knowledge.


# Course 5 Module 3

Security teams protect organizational assets by identifying weaknesses, anticipating how they can be exploited, and continuously maintaining defenses against both known and unexpected threats.

Vulnerabilities and Exploits

- A vulnerability is an inherent weakness in a system, whereas an exploit is the specific method or tool used to take advantage of that weakness.
- Effective security planning relies on anticipating potential exploits in advance so proper safeguards can be implemented before an attack occurs.

The Vulnerability Management Cycle

- Vulnerability management is an ongoing four-step cycle: identify vulnerabilities, analyze potential exploits, implement defensive controls, and evaluate defense effectiveness.
- Leveraging diverse perspectives across security teams improves the identification of obscure flaws and strengthens defensive planning.

#### **Zero-Day Exploits**

- A zero-day refers to an unknown exploit that is discovered while actively occurring, giving organizations zero days of advance notice to develop a patch.
- These unmapped threats present elevated risk because traditional defenses and mitigations have not yet been designed to counter them. 