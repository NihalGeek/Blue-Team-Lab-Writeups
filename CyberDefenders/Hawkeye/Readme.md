# 🦅 HawkEye Malware Investigation

## 📖 Scenario

A packet capture (PCAP) containing network activity from a Windows workstation was provided for investigation.

The objective was to determine whether the system had been compromised, identify the malware involved, reconstruct the attack timeline, and understand how sensitive information was stolen and exfiltrated.

---

# 📌 Lab Summary

| Property | Value |
|----------|-------|
| Platform | CyberDefenders |
| Challenge | HawkEye |
| Category | Malware Analysis / Network Forensics |
| Difficulty | Easy |
| Tools Used | Wireshark, VirusTotal, PowerShell, Base64 Decoder, WHOIS |
| Status | ✅ Completed |

---

# 🕒 Timeline

| Event | Time (UTC) |
|------|-------------|
| First Packet Captured | 2019-04-11 02:07:07 |
| Last Packet Captured | 2019-04-11 03:10:48 |
| Total Capture Duration | 1 Hour 3 Minutes 41 Seconds |

---

# 🔍 Phase 1 — Initial Traffic Analysis

The investigation started by reviewing the overall network activity using **Statistics → Conversations** within Wireshark.

One external IP address immediately attracted attention due to the unusually high amount of traffic exchanged with the victim workstation.

```
Victim
10.4.10.132

↓

External Host

217.182.138.150
```

This conversation accounted for approximately **2 MB of transferred data**, significantly more than any other external communication captured within the PCAP.

Based on the traffic volume, this communication became the primary focus of the investigation.

---

# 📊 Initial Findings

| Observation | Analysis |
|------------|----------|
| Large amount of traffic | Suspicious communication |
| Repeated TCP conversations | Possible malware communication |
| Multiple external connections | Required further investigation |

---

# 🔍 Phase 2 — Malware Delivery

Following the HTTP conversations revealed the download of a Windows executable from an external web server.

```
tkraw_Protected99.exe
```

The executable was extracted directly from the PCAP using **Export HTTP Objects**.

---

## Malware Verification

After exporting the executable, a SHA-256 hash was generated using PowerShell.

The hash was then submitted to VirusTotal.

VirusTotal reported that the executable was detected as malicious by numerous security vendors, confirming that **tkraw_Protected99.exe** was the malware responsible for the compromise.

---

# 📦 Malware Details

| Property | Value |
|----------|-------|
| Filename | tkraw_Protected99.exe |
| Type | Windows Executable |
| Size | ~1.93 MB |
| Verification | VirusTotal |
| Detection | Malicious |

---

# 🔍 Phase 3 — SMTP Investigation

While examining additional TCP conversations, SMTP traffic was identified.

Following the SMTP stream revealed an authenticated email session.

The malware successfully authenticated using the following credentials:

| Field | Value |
|------|-------|
| Username | sales.del@macwinlogistics.in |
| Password | Sales@23 |

The authentication completed successfully before the malware began transmitting encoded data.

---

# 🔓 Phase 4 — Payload Decoding

The SMTP email body was Base64 encoded.

After decoding the payload, the email contents revealed that the malware was **HawkEye Keylogger Reborn v9**.

The decoded report contained sensitive information harvested from the compromised system, including:

- Browser credentials
- Email credentials
- Outlook credentials
- Saved passwords
- Victim system information

The victim machine was identified as:

```
BEIJING-5CD1-PC
```

The logged-in user was:

```
Roman McGuire
```

---

# 🚨 Credential Theft

The decoded report contained credentials harvested from multiple applications.

Examples included:

| Source | Information Collected |
|---------|----------------------|
| AOL | Username & Password |
| Bank of America | Username & Password |
| Google Chrome | Saved Credentials |
| Microsoft Outlook | Email Credentials |
| POP3 Configuration | Account Details |
| SMTP Configuration | Mail Server Details |

These credentials had been harvested directly from the victim workstation before being packaged into the email.

---

# 📧 Data Exfiltration

The SMTP session showed the malware creating an email containing the stolen credentials.

Interestingly, both the sender and recipient addresses were identical.

This indicates that the attacker used the compromised mailbox as a storage location for stolen credentials, allowing them to retrieve the information later by accessing the mailbox.

Rather than communicating with a traditional Command & Control (C2) server, the malware exfiltrated stolen data through a legitimate SMTP service.

---

# 🌍 Infrastructure Analysis

Further investigation was performed on the external infrastructure involved during the attack.

Public WHOIS records identified the external server as belonging to an OVH network allocation located in France.

The MAC address vendor associated with the captured traffic was identified as Hewlett Packard.

These findings provided additional context regarding the systems involved during the investigation.

---

# 📈 Attack Timeline

```text
Victim receives phishing email
            │
            ▼
Victim downloads tkraw_Protected99.exe
            │
            ▼
Executable launched
            │
            ▼
HawkEye Keylogger installed
            │
            ▼
Browser & Email credentials harvested
            │
            ▼
SMTP Authentication
            │
            ▼
Credentials encoded using Base64
            │
            ▼
Email containing stolen credentials created
            │
            ▼
Credentials successfully exfiltrated
```

---

# 🚩 Indicators of Compromise (IoCs)

| Category | Indicator |
|----------|-----------|
| Malware | HawkEye Keylogger Reborn v9 |
| Malware File | tkraw_Protected99.exe |
| Internal Host | 10.4.10.132 |
| External Host | 217.182.138.150 |
| Protocol | HTTP |
| Protocol | SMTP |
| Encoding | Base64 |
| Activity | Credential Theft |
| Activity | Data Exfiltration |

---

# 🧠 Skills Demonstrated

- ✅ Wireshark Packet Analysis
- ✅ Network Traffic Analysis
- ✅ TCP Conversation Analysis
- ✅ HTTP Object Extraction
- ✅ SHA-256 Hashing
- ✅ Malware Verification
- ✅ VirusTotal Analysis
- ✅ SMTP Investigation
- ✅ Base64 Decoding
- ✅ Threat Intelligence
- ✅ WHOIS Analysis
- ✅ IOC Identification
- ✅ Attack Chain Reconstruction

---

# 🎯 Conclusion

This investigation demonstrated how a complete malware infection can be reconstructed using only network traffic.

Beginning with statistical traffic analysis, the investigation identified suspicious communications, recovered the malicious executable, confirmed its malicious nature using VirusTotal, analyzed SMTP-based credential exfiltration, decoded harvested credentials, and reconstructed the full attack lifecycle.

The challenge highlighted the importance of correlating evidence from multiple sources rather than relying on a single indicator. By combining packet analysis, malware verification, protocol analysis, and threat intelligence, it was possible to understand the entire compromise—from initial malware delivery to successful credential theft and exfiltration.

---
# Lessons Learned

- High-volume conversations can quickly identify suspicious hosts.
- Exporting HTTP objects enables recovery of malware directly from PCAPs.
- SMTP traffic should never be ignored, as it can reveal credential exfiltration.
- Base64 encoding does not provide security—it only obscures data.
- Threat intelligence platforms such as VirusTotal help validate malware samples.
- Correlating multiple pieces of evidence is essential to accurately reconstruct an attack.
