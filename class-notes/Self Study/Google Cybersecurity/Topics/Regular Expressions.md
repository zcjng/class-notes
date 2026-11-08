---
course: Coursera Google Cybersecurity
topic: Python
tags:
  - google
  - cybersecurity
---

# 🧬 Regular Expressions

[[Google Cybersecurity|← Back to Course Hub]] · [[2026-11-08|Related day: 2026-11-08]]

A **regular expression (regex)** is a sequence of characters that defines a **search pattern** — letting you find text by its *structure* when you don't know the exact string.

That's exactly the analyst's problem: you never know the IP, email or filename in advance, but you know what one looks like.

---

## 🐍 Python's `re` Module

```python
import re

re.findall(pattern, text)
```

- `import re` loads the module; patterns are ordinary Python strings.
- **`re.findall()`** takes the pattern and the target string, and returns a **list of every match** found.
- No match → empty list.

```python
import re

text = "Contact soc@company.com or abuse@company.org for details."
pattern = r"\w+@\w+\.\w+"

print(re.findall(pattern, text))
# ['soc@company.com', 'abuse@company.org']
```

---

## 🔤 Character Symbols

| Symbol | Matches |
| :--- | :--- |
| `\w` | Any alphanumeric character **or underscore** |
| `\d` | A single digit |
| `\s` | Whitespace (space, tab) |
| `\.` | A **literal** period |
| `.` | **Any** character, including symbols |

> [!WARNING] The period trap
> `.` matches anything. To match an actual dot — as in a domain name — you must escape it as `\.`.

---

## 🔢 Quantifiers

| Quantifier | Repetitions |
| :--- | :--- |
| `+` | **One or more** |
| `*` | **Zero or more** |
| `{2}` | Exactly 2 |
| `{1,3}` | Between 1 and 3 |

### Building up the email pattern

| Piece | Meaning |
| :--- | :--- |
| `\w+` | Username — one or more alphanumerics |
| `@` | Literal at-sign |
| `\w+` | Domain name |
| `\.` | Literal dot |
| `\w+` | Extension (`com`, `org`) |

**Composite example:** `\w+:\s\d+` matches a label, a colon, whitespace, then a number — useful for pulling values out of log lines like `port: 5023`.

---

## 🛡️ Where Analysts Use Regex

- **Log parsing** — pull IPs, usernames, ports and timestamps out of free text.
- **Raw log search** in Google SecOps — the only mode that accepts regex.
- **Wireshark `matches`** operator — filter packets by regex.
- **`grep`** on Linux — see [[Linux Filtering]].
- **Detection rules** — YARA-L and SIEM correlation logic rely on the same pattern thinking. See [[SIEM Queries]].

---

## 🔗 Related

- [[Linux, SQL & Python]]
- [[SIEM Queries]]
- [[Log Management & Formats]]
- [[Linux Filtering]]
- [[Wireshark]]
- [[Security Tools]]
