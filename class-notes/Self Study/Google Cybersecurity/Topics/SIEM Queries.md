---
course: Coursera Google Cybersecurity
topic: Tools
tags:
  - google
  - cybersecurity
---

# 🔎 SIEM Queries

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-08|Related day: 2026-11-08]]

Once logs are collected, normalized and **indexed**, the analyst's job becomes writing a good query. The two platforms in this course are **Splunk** (SPL) and **Google SecOps / Chronicle** (UDM search).

---

## ⚡ Why Specificity Matters

SIEM databases hold enormous volumes of historical data. A broad search can **slow response times dramatically**.

Narrow the query with:

- **Event IDs** — hunt a specific log event rather than "everything".
- **Tight date and time ranges** — incidents have a window; use it.
- **Indexes and sources** — search the right data store, not all of them.

---

## 🟢 Splunk — Search Processing Language (SPL)

```spl
index=main fail
index=main fail* | stats count by host
index=web status=500 host!=www1
```

| Element | Behaviour |
| :--- | :--- |
| `index=main` | Restricts the search to a specific index |
| `fail*` | **Wildcard** — matches `failed`, `failure`, `failures` |
| `"exact phrase"` | Quotation marks force exact phrase matching |
| `OR` | Boolean operator — search multiple terms at once |
| `\|` | **Pipe** — passes output of one command into the next |

### Reading results

- **Timeline** visualisation — spots activity peaks instantly.
- **Events viewer** — raw log records with timestamps.
- **Metadata panel** — host, source, sourcetype.
- **Exclusion filters** — `host!=www1` removes a noisy host from the investigation.

**Splunk Enterprise Security** offers collection, indexing, search, analysis and visualisation across the whole environment.

---

## 🔵 Google SecOps (Chronicle)

### Two search modes

| Mode | Data | Speed | When to use |
| :--- | :--- | :--- | :--- |
| **UDM Search** (default) | Ingested, parsed and **normalized** data | Fast | Normal investigation |
| **Raw Log Search** | Unparsed raw logs | Slower | When normalized data is missing — also for troubleshooting ingestion. Supports hashes, filenames and **regular expressions** |

**UDM** = Unified Data Model. Events carry standard field groups: **Entities**, **Event metadata**, **Network metadata**, and **Security results**.

```text
metadata.event_type = "USER_LOGIN" AND security_result.action = "BLOCK"
```

That query finds blocked or failed authentication attempts. Human-readable equivalents are built with a structured query builder.

**YARA-L** is the rule language used for Chronicle **detection** rules — e.g. spotting data exfiltration patterns in ingested logs.

### Reading results

- Timeline **bar graph** for activity patterns over time.
- Timestamped event list with associated device assets.
- Click an event to inspect the **underlying raw log**.
- **Quick filters** (e.g. target IP) to narrow the dataset.

---

## 🆚 Side by Side

| | **Splunk** | **Google SecOps** |
| :--- | :--- | :--- |
| **Query language** | SPL | UDM search; **YARA-L** for rules |
| **Data model** | Indexes + sourcetypes | Unified Data Model (UDM) |
| **Raw log access** | Yes | Yes, via Raw Log Search |
| **Best for** | Deep custom correlation across indexes | Fast normalized search in cloud-native environments |

---

## 🔗 Related

- [[Logs & SIEM]]
- [[Log Management & Formats]]
- [[Suricata]] — `eve.json` is designed to feed these tools
- [[Regular Expressions]] — used by raw-log search and `matches` filters
- [[Incident Triage]] — the queries that feed prioritization
