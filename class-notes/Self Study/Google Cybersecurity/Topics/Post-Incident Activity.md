---
course: Coursera Google Cybersecurity
topic: Incident Response
tags:
  - google
  - cybersecurity
---

# 📝 Post-Incident Activity

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-05|Related day: 2026-11-05]]

The final phase of the [[Incident Response Lifecycle]] — and the one that decides whether the same incident happens again.

---

## 📄 The Final Report

A comprehensive review covering:

- **The 5 W's** — who, what, where, why, when
- An event **timeline**
- **Incident details** and investigative findings
- **Recommendations** for future prevention

Structure typically includes an executive summary, chronological timeline, findings and recommendations.

> [!TIP] Know your audience
> The same report gets read by engineers and executives. Use appropriate language for non-technical stakeholders — and **tailor the depth** to the reader.

---

## 🗣️ Lessons Learned Meetings

Held **within about two weeks** of remediation, bringing together everyone involved.

**Preparation matters:** set an agenda in advance and designate roles — a **moderator** and a **scribe** — so cross-departmental collaboration actually happens.

**Discussion focuses on:**

- How the incident was discovered
- How it was contained
- How recovery went
- What could have been handled differently

> [!IMPORTANT] Blameless by design
> Reviews frequently uncover human error — a missed recovery step, someone clicking a phishing link. These are treated as **learning opportunities**, not grounds for individual blame. Blame makes people hide information, which makes the next incident worse.

---

## ✅ Actionable Recommendations

Outputs of the review:

- Gaps in procedures
- Errors made during response
- Ineffective security controls

...which become **prioritised updates**: revised [[Playbooks & Incident Response|playbooks]], new tooling, or new training.

---

## 📚 Documentation Discipline

Post-incident work is where good documentation pays off. Documentation must be:

- **Targeted** — depth and technical level matched to the audience
- **Concise** — purpose stated up front so findings are found fast
- **Maintained** — regularly reviewed and updated as threats and regulations change

> [!NOTE] Why documentation matters operationally
> Documenting rule activations, severity levels and false positives lets a security operation **scale beyond a few analysts**. Playbooks remove ambiguity so everyone knows their role during an incident. Records also serve as evidence for insurance claims, regulatory investigations and legal matters.

### Chain of custody

**Chain of custody** is the formal process of documenting evidence possession and control across an incident's lifecycle — a transparent paper trail of **who** handled evidence, and **why, when and where** it was transferred.

- Write-protect digital media and verify it with a cryptographic **hash** before handling, so later tampering is detectable.
- Document the evidence description (IPs, MAC addresses) plus a log of transfers, dates and personnel.
- A **broken chain** — inconsistent logging or missing entries — can render evidence inadmissible in legal proceedings.

---

## 🔗 Related

- [[Incident Response Lifecycle]]
- [[Playbooks & Incident Response]]
- [[Business Continuity]]
- [[Hashing & Password Storage]]
- [[CSIRT & SOC Teams]]
