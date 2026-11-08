---
course: Coursera Google Cybersecurity
topic: Threats
tags:
  - google
  - cybersecurity
---

# 🎣 Phishing & Social Engineering

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-14|2026-10-14]] · [[2026-11-01|Related day: 2026-11-01]]

**Social engineering** is psychological manipulation that exploits human error — no sophisticated programming required. The goal is to trick a target into breaking normal security procedures and surrendering information, credentials, access, or valuables.

Attack timeframes vary wildly: from seconds (a phone call posing as tech support asking for a password) to months (watching a temporary worker's social media until you find an exploitable gap in their training).

---

## 🎭 The Four-Stage Social Engineering Lifecycle

1. **Prepare** — research the target, gather intelligence, find exploitable angles.
2. **Establish trust (pretexting)** — open communication using a disguised identity and a fabricated scenario that lowers defences.
3. **Persuade & manipulate** — use corporate terminology and psychological pressure to get the victim to volunteer information.
4. **Disconnect** — break communication quickly after getting the data, covering tracks.

> [!TIP] The best defence
> Attackers weaponise curiosity and the urge to be helpful. The most effective countermeasure is **education** — teaching people the behavioural warning signs. Managerial controls (policies, standards, procedures) are the formal layer underneath.

---

## 🕸️ The Social Engineering Tactics

| Tactic | Mechanism |
| :--- | :--- |
| **Baiting** | Tempts people into compromising security — e.g. a USB drive left in a car park, plugged in out of curiosity |
| **Phishing** | Digital communications (usually email) that trick people into revealing data or deploying malware |
| **Quid pro quo** | A form of baiting offering a reward for access, info or money — e.g. a fake loan officer offering a lower interest rate |
| **Tailgating** (piggybacking) | An unauthorised person follows an authorised person into a restricted area |
| **Watering hole** | The attacker compromises a site a specific group frequently visits and infects it — e.g. the **Holy Water** attack of 2020 on religious, charity and volunteer sites |

---

## 📌 Phishing

**Phishing** is the use of digital communications to trick people into revealing sensitive data or deploying malicious software.

A single compromised credential can hand an attacker full entry to the corporate network — and with it client lists or product secrets.

### 🧰 Inside a phishing kit

Attackers with minimal skill buy pre-packaged **phishing kits** built from three tools:

1. **Malicious attachments** — files carrying malware that infects internal infrastructure when opened.
2. **Fake data-collection forms** — disguised surveys that prompt for atypical, sensitive data.
3. **Fraudulent web links** — spoofed sites mirroring trusted brands to harvest credentials.

### 📡 Delivery channels

| Type | Channel |
| :--- | :--- |
| **Standard phishing** | Mass-distributed email |
| **Smishing** | **SMS** — spoofed local area codes or impersonated friends/family |
| **Vishing** | **Voice calls** — fake tech support, bank staff claiming an account issue |

### 🎯 Targeted variants

- **Spear phishing** — a malicious email targeting a **specific person or group**, appearing to come from a trusted source (e.g. the accountants at a small firm).
- **Whaling** — spear phishing aimed at **high-ranking executives**.
- **Business Email Compromise (BEC)** — a message that appears to come from a known source making a legitimate-sounding request, usually for financial gain.
- **Angler phishing** — impersonating customer service on social media. Attackers intercept complaints posted publicly, then contact the angry customer from a lookalike account and "helpfully" request sensitive details.

---

## 🕰️ Where It Started

One of the earliest mass phishing campaigns targeted **AOL Instant Messenger** users. Emails asked them to "verify" accounts or supply billing details, using official logos, colours and fonts. Stolen information was used to create fraudulent AOL accounts for further crimes. AOL responded by adding in-platform warnings — an early example of **mass phishing**, where volume increases the odds someone takes the bait.

---

## 🛡️ Defences

**Technical controls**

- **Blocklists** — ban known-malicious senders/addresses.
- **Allowlists** — permit only verified IPs to deliver internal mail.
- **Intrusion prevention systems (IPS)** — scrutinise email traffic patterns, quarantine anomalies, generate event logs.
- **Email filters and anti-phishing policies.**

**Human controls**

- Continuous security awareness training on red flags.
- Clear, simple reporting procedures — make reporting one click.
- Event-driven alerts (e.g. "a new phishing campaign is targeting us right now").

> [!NOTE] Layering is the point
> Filters catch known bad senders; training catches what slips through; reporting catches what training missed. See [[Defense in Depth]].

---

## 🔗 Related

- [[Threat Actors]]
- [[Malware Types & Behaviors]]
- [[IDS & IPS]]
- [[Security Controls]]
- [[Incident Response Lifecycle]]
- [[Malware History]]
