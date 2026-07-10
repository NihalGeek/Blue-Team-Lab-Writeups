# 🕵️ Network Analysis – Web Shell

## 📖 Overview

This investigation involved analyzing a packet capture (PCAP) to identify suspicious activity targeting a web application. By combining statistical analysis, packet inspection, HTTP stream reconstruction, and payload decoding, the complete attack chain was reconstructed—from reconnaissance to successful remote command execution through a web shell.

---

# 🎯 Investigation Objectives

| Objective | Description |
|-----------|-------------|
| 📊 Analyze Network Traffic | Identify abnormal communication patterns within the PCAP |
| 🔍 Investigate Suspicious Activity | Isolate malicious HTTP requests |
| 🌐 Reconstruct Sessions | Follow client-server communication |
| 🔓 Decode Payloads | Analyze URL-encoded attacker payloads |
| 🛡️ Understand the Attack | Reconstruct the attack lifecycle |

---

# 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Packet Capture Analysis |
| URL Decoder | Decode URL-encoded payloads |

---

# 🔍 Investigation Timeline

## Phase 1 — Initial Traffic Analysis

The investigation began with a high-level review of the captured network traffic using Wireshark's **Statistics** feature.

One particular source host immediately stood out because it generated a significantly higher number of packets than the rest of the captured traffic.

Further inspection showed that many of these packets also contained relatively large payloads, making the communication unusual enough to warrant deeper investigation.

### Initial Observations

| Observation | Analysis |
|------------|----------|
| High packet count | Possible automated activity |
| Large packet sizes | Suggested significant data transfer or payload delivery |
| Repeated communication | Required further investigation |

---

## Phase 2 — Narrowing the Scope

Instead of inspecting every captured packet, Wireshark display filters were used to isolate the relevant traffic.

This reduced background noise and allowed the investigation to focus only on communication involving the suspicious hosts.

Using packet filtering and stream reconstruction made it possible to understand the interaction in chronological order.

### Techniques Used

| Technique | Purpose |
|-----------|---------|
| Display Filters | Reduce unrelated traffic |
| HTTP Filtering | Focus on web application traffic |
| Follow HTTP Stream | Reconstruct complete conversations |

---

## Phase 3 — Session Reconstruction

Multiple HTTP sessions were reconstructed using **Follow HTTP Stream**.

These sessions provided valuable context that individual packets alone could not reveal.

During analysis, several important observations were made:

- Authentication requests containing transmitted credentials
- Server software and version information
- User-Agent strings identifying automated reconnaissance tools
- Structured HTTP requests targeting the web application

These findings indicated that the traffic represented a coordinated attack rather than normal user activity.

---

## Phase 4 — Attack Reconstruction

As additional HTTP requests were examined, a clear attack progression emerged.

The evidence suggested that the attacker first performed reconnaissance against the target before interacting with vulnerable application components.

Subsequent requests revealed the upload of a malicious PHP file, which was later accessed repeatedly to execute operating system commands through the web server.

Many request parameters were URL-encoded, requiring additional decoding before their true purpose became visible.

After decoding the payloads, the requests exposed multiple offensive techniques, including SQL injection attempts, command execution, and other malicious inputs directed at the compromised application.

---

# 📊 Investigation Summary

| Stage | Findings |
|--------|----------|
| Reconnaissance | Automated scanning and enumeration activity detected |
| Web Enumeration | Multiple HTTP requests targeting application resources |
| Credential Discovery | Login sessions observed during HTTP analysis |
| Server Fingerprinting | Server details and technologies identified |
| Web Shell Activity | Malicious PHP web shell uploaded and accessed |
| Command Execution | HTTP requests used to execute commands remotely |
| Payload Analysis | URL-decoded payloads revealed attack techniques |

---

# 🚩 Indicators of Suspicious Activity

| Indicator | Description |
|-----------|-------------|
| High Packet Volume | Unusual communication between hosts |
| Large HTTP Payloads | Suspicious request bodies |
| Automated User-Agent | Reconnaissance and exploitation tools identified |
| Encoded Payloads | URL-encoded malicious input |
| Repeated HTTP Requests | Consistent interaction with uploaded web shell |
| Remote Command Execution | Commands executed through HTTP parameters |

---

# 🧠 Skills Demonstrated

| Skill | Demonstrated |
|-------|:------------:|
| Wireshark Packet Analysis | ✅ |
| Network Traffic Analysis | ✅ |
| HTTP Traffic Inspection | ✅ |
| Packet Filtering | ✅ |
| Session Reconstruction | ✅ |
| URL Decoding | ✅ |
| IOC Identification | ✅ |
| Attack Chain Reconstruction | ✅ |
| Web Shell Analysis | ✅ |



# 🎯 Conclusion

This investigation demonstrated how seemingly ordinary HTTP traffic can conceal a complete attack lifecycle.

Starting with statistical anomalies, the investigation progressively narrowed the scope through packet filtering, reconstructed HTTP sessions, and decoded attacker payloads. These steps ultimately revealed the progression from reconnaissance and web application enumeration to web shell deployment and remote command execution.

The exercise reinforced the importance of correlating multiple sources of evidence rather than relying on a single indicator. By combining traffic statistics, protocol analysis, and payload inspection, it was possible to reconstruct the attack with confidence and gain a deeper understanding of the techniques used by the attacker.
