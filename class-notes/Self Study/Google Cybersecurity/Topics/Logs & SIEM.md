---
course: Coursera Google Cybersecurity
topic: Tools
tags:
  - google
  - cybersecurity
---

# 📟 Logs & SIEM

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-10-15|Related day: 2026-10-15]]

---

## 📜 Understanding Logs

A **log** is a record of events that occur within an organization's systems and networks — including outbound requests to the internet from within the network. Logs are crucial for identifying vulnerabilities and potential security breaches.

| Log Type | Records |
| :--- | :--- |
| **Firewall log** | Attempted or established connections for incoming traffic from the internet. |
| **Network log** | All computers and devices that enter and leave the network, plus connections between devices and services on the network. |
| **Server log** | Events related to services such as websites, emails, or file shares — e.g. login, password, and username requests. |

---

## 🖥️ SIEM Tools

A **Security Information and Event Management (SIEM)** tool is an application that **collects and analyzes log data** to monitor critical activities in an organization and identify potential breaches as they happen.

- **Dashboards** — SIEM tools provide dashboards that visually organize data into categories; different tools have different dashboard types.
- **Metrics** — key technical attributes such as **response time, availability, and failure rate**, used to assess the performance of a software application; provided to stakeholders via dashboards.
- **Alerting** — SIEM tools **reduce the amount of data an analyst must review** by providing alerts for specific types of threats, risks, and vulnerabilities.
- **SOAR** — **Security orchestration, automation, and response** is a collection of applications, tools, and workflows that uses **automation to respond to security events**.

> [!TIP] TLDR
> SIEM tools are being further enhanced to improve automation and availability, which means they are mostly cloud-based.

### Hosting Options

- **Self-hosted** — the organization installs, operates, and maintains the tool on its own physical infrastructure; ideal for organizations needing physical control over confidential data.
- **Cloud-hosted** — maintained and managed by SIEM providers and accessed via the internet; suited for organizations that prefer not to invest in their own infrastructure (easier to set up, use, and maintain — a less experienced security team may choose this).
- **Hybrid** — combines self-hosted and cloud-hosted tools: benefits of the cloud while keeping physical control over confidential data.
- **Cloud-native** — fully maintained and managed by the vendor, designed to take full advantage of cloud capabilities (availability, flexibility, scalability).

> [!NOTE] IoT
> The growth of the **Internet of Things (IoT)** — interconnected devices with internet access — enlarges both the **attack surface** and the amount of data threat actors can exploit, driving the shift to cloud SIEM.

### Common SIEM Tools

| Tool | Type |
| :--- | :--- |
| **Splunk Enterprise** | Self-hosted data analysis platform for retaining, analyzing, and searching log data in real time. |
| **Splunk Cloud** | Cloud-hosted; collecting, searching, and monitoring log data — useful for hybrid or cloud-only environments. |
| **Chronicle** | Google's cloud-native tool for retaining, analyzing, and searching data: log monitoring, data analysis, and data collection. |

---

## 🆓 Open-Source vs. Proprietary Tools

- **Open-source tools** — often free and user-friendly; built collaboratively by the public, which can make the software more secure; highly customizable, producing a variety of services from the same package.
    - **Suricata:** open-source network analysis and threat detection software that inspects network traffic to identify suspicious behavior and generate logs.
- **Proprietary tools** — developed and owned by a person or company; users typically pay for usage and training, and only the owners can access and modify the source code (e.g. Splunk®, Google SecOps/Chronicle).

---

## 🔗 Related

- [[Security Tools]]
- [[Playbooks & Incident Response]]
- [[Linux, SQL & Python]]
- [[Common Vulnerabilities]]
