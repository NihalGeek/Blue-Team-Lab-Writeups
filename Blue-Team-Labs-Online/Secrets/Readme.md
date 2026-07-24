# 🔐 Secrets (HS256)

<h3 align="center">Blue Team Labs Online (BTLO) Write-up</h3>

<p align="center">
<img src="https://img.shields.io/badge/Platform-BTLO-blue?style=for-the-badge">
<img src="https://img.shields.io/badge/Category-Incident%20Response-success?style=for-the-badge">
<img src="https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge">
<img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
</p>

---

# 📖 Overview

The **Secrets** challenge focuses on investigating a suspicious **JSON Web Token (JWT)** secured using the **HS256 (HMAC-SHA256)** signing algorithm.

The objective was to analyze the provided token, recover its signing secret, and generate a **valid low-privilege JWT** by modifying the privilege level while maintaining a valid signature.

---

# 🎯 Objectives

- ✅ Decode and analyze the JWT
- ✅ Identify the token structure
- ✅ Understand HS256 authentication
- ✅ Recover the signing secret
- ✅ Generate a valid low-privilege JWT
- ✅ Successfully complete the BTLO challenge

---

# 📑 Table of Contents

- 📖 Overview
- 🎯 Objectives
- 🛠️ Tools Used
- 📚 Skills Practiced
- 🧠 Investigation Timeline
- 📷 Investigation Evidence
- 📊 Key Learnings
- 💡 Takeaways
- 🏁 Challenge Summary

---

# 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| 🔑 Hashcat | Recover JWT signing secret |
| 🔍 CyberChef | Decode JWT |
| 🌐 token.dev | Verify & sign JWT |
| 💻 PowerShell | Execute Hashcat commands |

---

# 📚 Skills Practiced

- JSON Web Tokens (JWT)
- Base64URL Encoding
- HS256 Authentication
- HMAC
- Password Cracking
- Hashcat
- Dictionary Attacks
- Mask Attacks
- JWT Manipulation
- Authentication Security

---

# 🧠 Investigation Timeline

## 🔍 Phase 1 – JWT Analysis

The supplied JWT was decoded to inspect its:

- Header
- Payload
- Signature

### Findings

- Algorithm: **HS256**
- Token Type: **JWT**
- Administrative privilege identified in the payload.

---

## 🔐 Phase 2 – Understanding JWT Authentication

The payload contained an administrator privilege.

Changing:

```json
"admin": true
```

to

```json
"admin": false
```

immediately invalidated the signature.

This demonstrated that HS256 signs the entire payload using a shared secret.

---

## ⚡ Phase 3 – Secret Recovery

### Attempt 1 — Dictionary Attack

Performed a standard dictionary attack using **RockYou**.

```bash
hashcat -m 16500 -a 0 jwt.txt rockyou.txt
```

### Result

❌ Exhausted

The signing secret was **not present** within the RockYou wordlist.

---

### Attempt 2 — Evidence-Based Hypothesis

The challenge contained the following clue:

```
BTL{_4_Eyes}
```

This suggested that the signing secret might consist of **four characters**.

A mask attack was performed.

```bash
hashcat -m 16500 -a 3 jwt.txt ?a?a?a?a
```

### Result

✅ Secret Successfully Recovered

```
BTL0
```

---

## 🔑 Phase 4 – JWT Re-Signing

After recovering the secret, the JWT payload was modified:

```json
"admin": true
```

↓

```json
"admin": false
```

Using the recovered signing secret, a new **verified HS256 JWT** was generated.

---

## ✅ Phase 5 – Validation

The newly generated JWT successfully passed signature verification and was accepted by the challenge platform.

Challenge completed successfully.

---
---

# 📊 Key Learnings

| Concept | Status |
|----------|:------:|
| JWT Structure | ✅ |
| Base64URL Encoding | ✅ |
| HS256 Authentication | ✅ |
| HMAC | ✅ |
| Hashcat | ✅ |
| Dictionary Attack | ✅ |
| Mask Attack | ✅ |
| JWT Re-signing | ✅ |

---

# 💡 Takeaways

- JWT payloads are **encoded**, not encrypted.
- HS256 security depends entirely on the strength of the signing secret.
- Dictionary attacks are not always sufficient for recovering secrets.
- Challenge clues can significantly reduce brute-force search space.
- Evidence-driven hypothesis testing is more effective than blindly increasing attack complexity.
- Understanding authentication mechanisms is more valuable than simply reproducing exploitation steps.

---

# 🏁 Challenge Summary

| Property | Value |
|----------|-------|
| Platform | Blue Team Labs Online |
| Challenge | Secrets |
| Category | Incident Response |
| Authentication | JWT (HS256) |
| Difficulty | Easy |
| Status | ✅ Completed |

---

<div align="center">

### ⭐ Thank you for reading!

This repository documents my hands-on cybersecurity learning journey through practical labs, investigations, and incident response challenges.

</div>
