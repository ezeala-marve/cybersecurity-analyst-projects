# VirusTotal Analysis Guide: From Basics to Threat Intelligence

## Project Overview
This guide demonstrates practical threat intelligence analysis using VirusTotal, moving beyond basic detection scores to interpret contextual clues and turn raw data into actionable intelligence. We analyze malicious and benign indicators across IP addresses, file hashes, and URLs, highlighting key patterns and limitations of automated scanning tools.

## Table of Contents
1. [Analyst's Framework](#analysts-framework)
2. [IP Address Analysis](#ip-address-analysis)
3. [File Hash Analysis](#file-hash-analysis)
4. [URL Analysis](#url-analysis)
5. [Key Findings & Patterns](#key-findings--patterns)
6. [Limitations of VirusTotal](#limitations-of-virustotal)
7. [Conclusion](#conclusion)

## Analyst's Framework

### For Files (Hashes)
1. **Detection Ratio:** Number of engines flagging the file. High ratio (50+/70) indicates malice; low ratio requires deeper investigation.
2. **File Details:**
   - **Names:** Generic names (e.g., `f.exe`) are suspicious
   - **Type:** Executables (`PE32`, `PE64`) are higher risk
   - **Signing:** Legitimate software is often signed by trusted companies
   - **Magic:** Confirms file type and helps verify disguises
3. **Behavior & Relations:**
   - **Contacted Domains/IPs:** Calls to unknown domains indicate C2 activity
   - **Dropped Files:** Shows what other files the malware creates/downloads
4. **Community:** Comments from analysts provide crucial context (e.g., "Ransomware")

### For URLs
1. **Detection Ratio:** Clean score (0/90+) doesn't guarantee safety
2. **Details Tab:**
   - **Title:** Often mimics legitimate services (e.g., "Microsoft Login")
   - **Redirects:** Final destination often reveals true malicious site
3. **Relations Tab:**
   - **Downloaded Files:** Clean URL serving malicious file is major red flag
   - **Contacted URLs:** Shows other potentially malicious sites
4. **Community:** Tags like `phishing`, `malware`, `scam` are critical indicators

### For IP Addresses
1. **Reputation Score:** Number of vendors flagging it (even 1/90 can be significant)
2. **Relations Tab:**
   - **Communicating Files:** Many malicious files talking to one IP = likely C2 server
   - **Historical DNS:** Shows domains resolved to this IP
   - **URLs:** Malicious URLs hosted on this IP
3. **Geo-Location & ASN:** Often in privacy-friendly countries or obscure hosting providers

## IP Address Analysis

### Malicious IP: 185.220.101.134
![Malicious IP Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/malicious_ip.png.png?raw=true)

**Findings:**
- **Reputation Score:** 11/94 vendors flagged as malicious
- **Network:** AS 60729 (Germany)
- **Communicating Files:** 58+ malicious files
- **Threat Type:** Ransomware (Win32/Filecoder)

**Verdict: 🚨 MALICIOUS - Command & Control Server**

**Analysis:** This IP exhibits classic C2 server characteristics with multiple confirmed malicious connections. The high number of communicating malware files (58+) and specific ransomware associations indicate active threat infrastructure.

### Benign IP: 8.8.8.8 (Google DNS)
![Benign IP Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/benign_ip.png.png?raw=true)

**Findings:**
- **Reputation Score:** 0/94 vendors flagged as malicious
- **Network:** AS 15169 (Google LLC, USA)
- **Communicating Files:** 1,000,000+ legitimate files
- **Purpose:** Google Public DNS server

**Verdict: ✅ CLEAN - Essential Internet Infrastructure**

**Analysis:** This IP demonstrates legitimate global infrastructure patterns. The massive scale of communications represents normal DNS query traffic rather than malicious activity.

## File Hash Analysis

### Malicious File: Mimikatz Trojan
**SHA-256:** `fb55414848281f804858ce188c3dc659d129e283bd62d58d34f6e6f568feab37`
![File Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/malicious_file.png.png?raw=true)

**Findings:**
- **Detection Ratio:** 63/72 vendors flagged as malicious
- **File Type:** Win32 EXE
- **File Name:** mimikatz.exe
- **Signing Status:** Not signed
- **Network Activity:** 9 contacted domains, 100+ IPs
- **Community Tags:** #malware #mimikatz

**Verdict: 🚨 MALICIOUS - Credential Access Tool**

**Analysis:** This file represents a credential-theft tool with overwhelming vendor consensus about its malicious nature. The lack of digital signature and extensive network communication patterns indicate advanced credential harvesting capabilities.

### EICAR Test File: Benign but Detected
**MD5:** `44d88612fea8a8f36de82e1278abb02f`
![EICAR Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/EICAR.png.png?raw=true)

| What to Look For | Finding | Analysis & Lesson |
| :--- | :--- | :--- |
| **Detection Ratio** | 62/68 | **Finding:** Extremely High Detection Rate. <br> **Lesson:** High score doesn't automatically mean malware - this file is designed to be detected. |
| **File Details** | Name: `eicar.com`<br>Type: `text/plain` | **Finding:** Simple text file, not executable. <br> **Lesson:** The "Details" tab provides crucial context. |
| **Behavior** | **No network connections.**<br>**No files dropped.** | **Finding:** File is inert. <br> **Lesson:** Absence of behavioral indicators shows it's not a traditional threat. |
| **Community** | Tags: `test`, `eicar` | **Finding:** Community identifies as test file. <br> **Lesson:** Crowd-sourced context is invaluable. |

**Verdict: 🟡 BENIGN TEST FILE**
This is the EICAR Anti-Virus Test File, a harmless text string used globally to verify antivirus functionality.

## URL Analysis
**URL:** http[:]//x4z9arb[.]cn/4712/
![URL Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/malicious_url.png.png?raw=true)

**Findings:**
- **Detection Ratio:** 4/97 vendors flagged as malicious
- **Final URL:** No redirects (same as initial)
- **Relations:** No downloaded files
- **Vendor Classification:** Malicious (alphaMountain.ai)

**Verdict: 🚨 SUSPICIOUS - Likely Phishing Landing Page**

**Analysis:** This URL shows limited but significant detection coverage. The lack of redirects indicates a direct malicious landing page, likely designed for credential phishing.

## Key Findings & Patterns

### Malicious Pattern Indicators:
1. **High-but-Not-Unanimous Detection:** Many threats show 50-70% detection rates
2. **Suspicious Network Patterns:** Callouts to unknown domains/IPs
3. **File Characteristics:** Unsigned executables with generic names
4. **Community Consensus:** Security vendor and community tags align

### Benign Pattern Indicators:
1. **Zero Detection Scores:** Essential infrastructure shows 0/94 ratings
2. **Massive Legitimate Scale:** Millions of clean communications
3. **Verified Ownership:** Clear corporate ownership
4. **Expected Behavior:** Normal network patterns for claimed purpose

## Limitations of VirusTotal

1. **Reactive Nature:** 
   - Only analyzes previously submitted samples
   - Cannot detect truly novel (zero-day) threats
   - Requires someone to first encounter and submit the threat

2. **Context Blindness:**
   - Cannot understand organizational context
   - May flag legitimate tools used maliciously
   - Cannot assess internal network relevance

3. **Evasion Techniques:**
   - Advanced threats use polymorphism to avoid hash-based detection
   - Time-based delays can avoid initial detection
   - Environmental awareness can bypass automated analysis

4. **False Sense of Security:**
   - Clean scans don't guarantee safety
   - Low detection rates may indicate novel threats
   - Requires expert interpretation of results

5. **Privacy Considerations:**
   - Submitting internal files exposes organizational data
   - Requires careful handling of sensitive information

---

> ⚠️ **Note:** VirusTotal analyses are subject to change over time as security vendors update their detection engines and threat intelligence feeds. Always re-scan if you need the most up-to-date results.

## Conclusion

VirusTotal serves as a valuable initial assessment tool but should not be relied upon as a complete security solution. Effective threat intelligence requires:

1. **Contextual Analysis:** Understanding what's normal for your environment
2. **Multi-Layered Defense:** Combining automated tools with human expertise
3. **Continuous Monitoring:** Recognizing that threat intelligence has a shelf life
4. **Correlation:** Combining multiple indicators for confident assessment

The most effective security posture uses VirusTotal as one component of a comprehensive threat intelligence strategy, complemented by network monitoring, endpoint detection, and human analysis.

**Date of Analysis:** August 29, 2025  
**Tools Used:** VirusTotal, Command-line utilities, Threat intelligence frameworks

---
