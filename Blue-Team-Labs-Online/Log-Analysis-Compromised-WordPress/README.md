# 🕵️ Log Analysis – Compromised WordPress

## 📖 Scenario

A compromised WordPress web server generated an Apache access log containing evidence of attacker activity. The objective was to analyze the log file, identify indicators of compromise, reconstruct the attack timeline, determine the exploited vulnerability, and trace the attacker's actions from initial reconnaissance to web shell execution.

---

# 📌 Lab Summary

| Property | Value |
|----------|-------|
| Platform | Blue Team Labs Online |
| Challenge | Log Analysis – Compromised WordPress |
| Category | Log Analysis / Web Security / Incident Response |
| Difficulty | Medium |
| Tools Used | Linux CLI, grep, egrep, cut, sort, uniq, Apache Access Logs, VirusTotal, AbuseIPDB, CVE Database |
| Status | ✅ Completed |

---

# 🎯 Investigation Objectives

- Analyze Apache access logs.
- Identify suspicious IP addresses.
- Determine the vulnerable WordPress plugins.
- Identify the exploited CVE.
- Locate the uploaded PHP web shell.
- Reconstruct the attack timeline.
- Validate indicators using threat intelligence.

---

# 🔍 Investigation Process

### 1. Identified Most Active IP Addresses

Using Linux command-line tools, the access log was parsed to determine which IP addresses generated the highest number of requests.

**Command Used**

```bash
cat access.log | cut -d ' ' -f1 | sort | uniq -c | sort -nr
```

**Finding**

- Attacker IP identified:
  - **119.241.22.121**

---

### 2. Identified User Agents

Extracted User-Agent strings to determine tools and browsers used during the attack.

**Command**

```bash
cat access.log | cut -d '"' -f6 | sort | uniq -c | sort -nr
```

**Interesting Findings**

- Firefox
- WPScan
- sqlmap
- python-requests

These indicate automated reconnaissance and exploitation activity.

---

### 3. Identified Vulnerable Plugins

Searched the log for installed plugins.

**Command**

```bash
egrep 'simple-file-list|contact-form-7' access.log
```

**Finding**

Two plugins were identified:

- Contact Form 7
- Simple File List

---

### 4. Identified Exploited Vulnerability

Research confirmed that **Contact Form 7 (<5.3.2)** was vulnerable to unrestricted file upload.

**CVE**

- CVE-2020-35489

---

### 5. Located Admin Login Endpoint

Recovered the hidden admin login URI from the logs.

**Finding**

```
/wp-login.php?itsec-hb-token=adminlogin
```

---

### 6. Identified Uploaded Web Shell

Analysis of POST requests revealed repeated access to an uploaded PHP web shell.

**Web Shell**

```
fr34k.php
```

---

### 7. Attack Timeline

1. Reconnaissance
2. Plugin Enumeration
3. Discovery of vulnerable Contact Form 7
4. Authentication through hidden login endpoint
5. File Upload Exploitation
6. PHP Web Shell Upload
7. Remote Command Execution

---

### 8. Threat Intelligence

The attacker IP was validated using:

- VirusTotal
- AbuseIPDB

No public detections were present at the time of analysis.

---

# 🚩 Indicators of Compromise

| Type | Value |
|------|-------|
| Attacker IP | 119.241.22.121 |
| Web Shell | fr34k.php |
| Vulnerable Plugin | Contact Form 7 |
| Additional Plugin | Simple File List |
| CVE | CVE-2020-35489 |
| Hidden Login URI | /wp-login.php?itsec-hb-token=adminlogin |

---

# 🧠 Skills Demonstrated

- Apache Log Analysis
- Linux Command Line
- Threat Hunting
- Incident Response
- IOC Identification
- Web Application Security
- WordPress Security
- CVE Research
- Threat Intelligence Validation
- Attack Timeline Reconstruction

---

# 📸 Evidence

Screenshots supporting each investigation step are available in the **images/** directory.

---

# ✅ Key Takeaways

This investigation demonstrated how Apache access logs can be used to reconstruct a complete attack chain. By combining Linux command-line analysis, web security knowledge, and threat intelligence resources, it was possible to identify the attacker, determine the exploited vulnerability, locate the uploaded web shell, and validate indicators of compromise.
