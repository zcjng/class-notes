---
course: Coursera Google Cybersecurity
topic: Incident Response
tags:
  - google
  - cybersecurity
---

# 👥 CSIRT & SOC Teams

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-03|Related day: 2026-11-03]]

Two different organizational units, often confused: a **CSIRT** responds to incidents, a **SOC** monitors for them continuously.

---

## 🧑‍🚒 CSIRT — Computer Security Incident Response Team

A specialized group of security professionals trained in incident management and response.

**Goals:** manage incidents effectively and efficiently, provide response and recovery services, and prevent future incidents. Security is a shared responsibility, so CSIRTs work cross-functionally with other departments.

Also called an **Incident Handling Team (IHT)** or **Security Incident Response Team (SIRT)** depending on the organization.

### Command, Control & Communication

Effective response requires all three:

| Element | Meaning |
| :--- | :--- |
| **Command** | Appropriate leadership and direction overseeing the response |
| **Control** | Managing the technical aspects — coordinating resources, assigning tasks |
| **Communication** | Keeping stakeholders informed |

### The three core roles

| Role | Responsibility |
| :--- | :--- |
| **Security analyst** | Continuously monitors the environment; analyses and triages alerts, performs root-cause investigation, escalates or resolves. Escalates critical threats to the technical lead |
| **Technical lead** | Manages all technical aspects — determines root cause, then creates and implements strategies for containment, eradication and recovery. Aligns IR priorities with business priorities (e.g. minimizing customer disruption) |
| **Incident coordinator** | Coordinates with relevant non-security departments during an incident |

**Non-security members** are regularly consulted: HR, public relations, management, IT, legal.

> [!NOTE] Structure varies
> A CSIRT can be a dedicated standing team or a task force that convenes as needed.

---

## 🖥️ SOC — Security Operations Center

An organizational unit dedicated to **monitoring networks, systems and devices** for threats or attacks. A SOC performs blue-team work: network monitoring, analysis and incident response.

### The tier pyramid

| Tier | Role | Responsibilities |
| :--- | :--- | :--- |
| **L1** | Least experienced analyst | Monitor, review and prioritise alerts by criticality; create and close tickets; escalate to L2/L3 |
| **L2** | Experienced analyst | Take escalated tickets and investigate deeper; configure and refine security tools; report to the SOC lead |
| **L3** | SOC lead | Manage team operations; perform advanced detection (malware and forensics analysis); report to the SOC manager |
| **Top** | SOC manager | Hire, train and evaluate staff; create performance metrics; produce incident/compliance/audit reports; communicate with executive stakeholders |

### Specialized roles

- **Forensic investigators** — usually L2/L3; collect, preserve and analyse digital evidence to determine what happened.
- **Threat hunters** — usually L3; detect, analyse and defend against new and advanced threats using threat intelligence.

---

## 🆚 CSIRT vs. SOC vs. NOC

| Unit | Focus |
| :--- | :--- |
| **CSIRT** | Responding to and resolving declared incidents |
| **SOC** | Continuous security monitoring and blue-team detection |
| **NOC** | Network **stability**, performance and availability — uptime, latency, packet loss |

A SOC is adversarial in mindset; a NOC is operational/reliability-minded. See [[Network Traffic Analysis]].

---

## 🔗 Related

- [[Incident Response Lifecycle]]
- [[Incident Triage]]
- [[IDS & IPS]]
- [[Logs & SIEM]]
- [[Network Traffic Analysis]]
- [[Playbooks & Incident Response]]
