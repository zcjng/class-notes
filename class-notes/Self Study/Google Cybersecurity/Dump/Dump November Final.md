# Course 6 Module 4

# Components of a detection signature

Action

- Defines what the IDS should do when rule conditions are met, such as alert, pass, or reject.
- Typically appears as the first element in a signature rule.

Header

- Specifies network traffic attributes, including protocols, source and destination IP addresses, ports, and traffic direction.
- Allows analysts to target external or unusual connections by defining the origin and destination of packets.

Rule Options

- Customizes signatures with extra parameters enclosed in parentheses and separated by semicolons, such as inspecting packet content for malicious payloads.
- Includes key fields like `msg` (the alert description), `sid` (a unique signature ID), and `rev` (the signature version number).

# Examine signatures with Suricata

Suricata Configuration and Navigation

- Suricata configuration files and pre-written rule templates are stored on the Linux system in the `/etc/suricata` directory, specifically within the `rules` folder.
- Security analysts can use commands like `cd`, `ls`, and `less` to locate and review rule files, where lines beginning with `#` indicate comments.

Signature Structure and Header

- The signature begins with an action, such as `alert`, which dictates the response triggered when conditions are satisfied.
- The rule header specifies traffic parameters, including the protocol (`http`), source (`HOME_NET`), destination (`EXTERNAL_NET`), ports (`ANY`), and direction of network flow.

Rule Options and Conditions

- Options enclosed in parentheses define specific criteria, such as the `msg` to display, traffic `flow` state (e.g., established connections), and specific packet `content` (e.g., the HTTP request "GET").
- Tailoring and testing these rules is essential for security analysts to effectively detect threats while minimizing false positives.
# Examine Suricata logs

Log Format Overview

- Suricata uses EVE JSON (Extensible Event Format, JavaScript Object Notation), which organizes data into key-value pairs for simplified searching and text extraction.
- Combining both alert logs and network telemetry logs enables security analysts to reconstruct the full sequence of events during an investigation.

Alert Logs

- These logs record security-relevant events triggered by signatures detecting suspicious network traffic, such as potential malware.
- Key fields in an alert log include the event type (`alert`), involved IP addresses, protocol, and signature details like the message and ID.

Network Telemetry Logs

- Telemetry logs record standard network flow activity, such as connections to specific ports, regardless of whether suspicious activity is present.
- An HTTP telemetry log example includes fields indicating the event type (`http`), accessed hostname, client user agent, and returned content type.

# Overview of Suricata
This reading provides an overview of Suricata, detailing its primary operational modes, signature rule components, and essential logging mechanisms for network threat detection.

Suricata Features and Capabilities

- Operates as a network- or host-based Intrusion Detection System (IDS), an Intrusion Prevention System (IPS) to block malicious traffic, or a Network Security Monitoring (NSM) tool to capture network logs.
- Allows security teams to analyze live traffic or packet captures for incident response, forensics, and signature testing.

Rule Structure and Customization

- Uses signatures composed of three main parts—an action (such as alert, pass, drop, or reject), a header (containing IP, port, and protocol details), and customizable rule options.
- Relies on custom-written rules tailored to an organization's specific network infrastructure to minimize false positives and enhance detection accuracy.

Configuration and Log Files

- Uses the `suricata.yaml` configuration file to define environment settings and rule evaluation order (defaulting to pass, drop, reject, then alert).
- Generates two primary log outputs: `eve.json` for detailed, metadata-rich event analysis and SIEM ingestion, and `fast.log` for basic, legacy alert records.
# Reexamine SIEM tools

The SIEM Data Pipeline

- SIEM tools collect large volumes of data from multiple sources and normalize it into a consistent format for easier analysis.
- Once normalized, the data is indexed so security analysts can quickly search and monitor network activity.

Common SIEM Platforms

- Splunk Enterprise Security provides tools to collect, index, search, analyze, and visualize security data across an environment.
- Chronicle, Google Cloud's SIEM platform, forwards, normalizes, and indexes security data so it can be queried through a search interface.

# Log sources and log ingestion

The SIEM Process

- The core process consists of collecting and aggregating data, normalizing it into a standard format, and analyzing it to identify unusual patterns.
- Log ingestion focuses on the initial collection and aggregation phase, providing the centralized data needed for analysts to prioritize security incidents.

Understanding Log Ingestion

- Ingestion involves copying and storing event data—such as authentication attempts and network activity—from generating sources directly into the SIEM tool.
- Creating a copy of the event data ensures that original source logs are preserved and not modified during analysis.

Using Log Forwarders

- Manual log collection is inefficient across large networks, making automated software like log forwarders the preferred approach.
- Log forwarders can be native to an operating system, third-party software, or proprietary tools integrated with the SIEM to automate sending logs for normalization and search.

# Query for events with Splunk

The Importance of Specific SIEM Queries

- SIEM databases store massive volumes of historical data, meaning broad searches can slow down response times significantly.
- Adding specific parameters, such as event IDs or narrow date and time ranges, speeds up queries and quickly pinpoints relevant security events.

Splunk Query Syntax and Search Operators

- Splunk uses Search Processing Language (SPL) to create targeted queries using specific indexes and keywords.
- Operators like the Boolean `OR` allow searching multiple terms simultaneously, while wildcards like `fail*` capture variations such as "failed" or "failures."

Analyzing Search Results and Refining Logs

- The Splunk interface presents search findings through a visual timeline to identify activity peaks, alongside an events viewer displaying raw log records and timestamps.
- Analysts can inspect event metadata—such as host name, source, and source type—and apply exclusion filters like `host!=www1` to refine investigation results.

# Query for events with Google SecOps

Search Types and Query Rules

- Chronicle uses the YARA-L language to define rules for detection, allowing analysts to search through ingested log data for activities like data exfiltration.
- While raw log searches are available to find unparsed data or troubleshoot ingestion, the default method is UDM search, which queries normalized and formatted data.

Constructing a UDM Query

- A structured query builder allows searches using common UDM fields, such as searching for `metadata.event_type = "USER_LOGIN"` combined with logical operators.
- Adding conditions like `security_result.action = "BLOCK"` narrows the search to specific outcomes, such as failed or blocked user authentication attempts.

Interpreting Search Results

- The results view displays a timeline bar graph to reveal activity patterns over time, along with a timestamped list of specific events and associated device assets.
- Analysts can click individual events to inspect the underlying raw logs for deeper investigation or use Quick Filters (such as target IP addresses) to narrow the data.


# Search methods with SIEM tools


Searching in Splunk

- Splunk uses Search Processing Language (SPL) to query indexed data, using commands and arguments to filter or format results (e.g., `index=main fail`).
- Analysts can refine queries using pipes (`|`) to pass the output of one command into another, wildcards (`*`) to match similar terms, or quotation marks for exact phrase matching.

Google SecOps (Chronicle) UDM Search

- Unified Data Model (UDM) Search is the default method that queries ingested, parsed, and normalized data for faster results.
- UDM events include standard fields such as Entities, Event metadata, Network metadata, and Security results to quickly locate specific information like user logins.

Google SecOps (Chronicle) Raw Log Search

- Raw Log Search queries unparsed, raw logs when needed data cannot be located through normalized UDM searches.
- This search method takes longer to run but allows querying by values like hashes or filenames and supports regular expressions for pattern matching.

# Course 7 Module 3

# Regular expressions in Python
Introduction to Regular Expressions

- A regular expression (regex) is a sequence of characters defining a search pattern, allowing analysts to locate text matching specific structures without knowing the exact strings beforehand.
- Common regex symbols include `+` to match one or more occurrences of a character and `\w` to match any alphanumeric character.

Constructing a Pattern for Email Addresses

- Combining symbols like `\w+` matches alphanumeric strings of varying lengths, representing the username, domain name, and extension segments of an email.
- Literal characters like `@` are included directly, while characters with special meanings like the period must be escaped with a backslash (`\.`) to be treated as literal punctuation.

Extracting Matches in Python

- Python uses the `re` module to handle regular expressions, requiring an import statement before use.
- The `re.findall()` function takes the regex pattern and target string as arguments, returning a list of all matching instances found in the text.

# More about regular expressions


The `re` Module and `re.findall()`

- Import the module using `import re` to work with regular expressions stored as Python strings.
- Use `re.findall(pattern, text)` with two parameters—the search pattern and the target string—to return all matching results in a list.

Character Type Symbols

- Match general character categories using `\w` for alphanumeric characters and underscores, or `\d` for single digits.
- Target whitespace, periods, or any character using `\s` for spaces, `\.` for literal periods, and `.` for any character including symbols.

Quantifiers and Pattern Construction

- Control repetitions with `+` for one or more occurrences, `*` for zero or more, or curly brackets like `\d{2}` or `\d{1,3}` for exact numbers or ranges.
- Combine components into composite patterns (such as `\w+:\s\d+`) to isolate target data from surrounding text.

