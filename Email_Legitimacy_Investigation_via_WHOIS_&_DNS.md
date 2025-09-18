# 🔍 SOC Analyst Project: Email Legitimacy Investigation via WHOIS & DNS

## Overview
This project simulates a real Security Operations Center (SOC) investigation to determine the legitimacy of an email sender. By analyzing email headers and performing open-source intelligence (OSINT) gathering techniques—including WHOIS lookup, DNS record analysis (via both Dig and PowerShell), and threat intelligence reputation checks—I methodically verified the authenticity of a domain and its associated infrastructure. This workflow is fundamental for identifying phishing attempts and malicious email campaigns.

## 🛠️ Tools Used
- **Email Header Analysis:** Manual inspection
- **WHOIS Lookup:** [`whois`](https://linux.die.net/man/1/whois) command-line utility
- **DNS Lookup:** 
  - [`dig`](https://linux.die.net/man/1/dig) command-line utility (Linux/Mac/WSL)
  - **PowerShell:** [`Resolve-DnsName`](https://docs.microsoft.com/en-us/powershell/module/dnsclient/resolve-dnsname) cmdlet (Windows)
- **Threat Intelligence Platforms:**
  - [VirusTotal](http://virustotal.com/)
  - [AbuseIPDB](http://abuseipdb.com/)

## 📋 Key Investigation Details
- **Target Domain:** `quora.com`
- **Sending IP:** `18.214.110.142`
- **DKIM Signature:** `d=quora.com` | `s=pigeon`
- **Analysis Date:** `(1-9-2025)`

## 🧪 Methodology & Workflow

I followed a structured, five-step analytical process to assess the email's legitimacy:

### 1. Extract Key Data from Email Headers
The first step was parsing the raw email headers to find the crucial investigative clues:
- **`From` Domain:** The visible sender address (`quora.com`).
- **`Received` Headers:** The originating mail server's IP address (`18.214.110.142`).
- **`DKIM-Signature`:** The signing domain (`d=quora.com`) and selector (`s=pigeon`).

> **📸 Screenshot Suggestion:** ![Email Headers](https://github.com/Major241/cyber-portfolio/blob/main/images/mail_header.png.png?raw=true)

### 2. Perform WHOIS Lookup
**Goal:** Establish domain registration history and ownership credibility.
```bash
whois quora.com
```
**Key Findings:**
- **Registration Date:** 1999
- **Registrar:** 
CSC Corporate Domains, Inc. (a reputable enterprise-grade registrar)
- **Registrant:** Legitimate corporate details
- **Conclusion:** An old, established domain with transparent registration is a strong initial indicator of legitimacy.

> **📸 Screenshot Suggestion:** ![Whois quora](https://github.com/Major241/cyber-portfolio/blob/main/images/quora_whois.png.png?raw=true)

### 3. Execute DNS Record Analysis
**Goal:** Verify the domain's email security policies and infrastructure.

I performed DNS lookups using **two different methods** to cross-verify results and demonstrate tool flexibility.

**Method A: Using `dig` (Linux/Mac/WSL)**
```bash
# Check Mail Servers (MX)
dig quora.com MX +short

# Check Sender Policy Framework (SPF)
dig quora.com TXT +short | grep spf

# Retrieve DKIM Public Key (using the selector from the header)
dig pigeon._domainkey.quora.com TXT +short

# Check DMARC Policy
dig _dmarc.quora.com TXT +short
```

**Method B: Using PowerShell (Windows)**
```powershell
# Check Mail Servers (MX)
Resolve-DnsName quora.com -Type MX

# Check Sender Policy Framework (SPF) and other TXT records
Resolve-DnsName quora.com -Type TXT

# Check DMARC Policy
Resolve-DnsName _dmarc.quora.com -Type TXT

# Retrieve DKIM Public Key (using the selector from the header)
Resolve-DnsName pigeon._domainkey.quora.com -Type TXT
```

**Key Findings:**
- **SPF:** Present, listing authorized mail servers.
- **DKIM:** A valid public key was found for the selector `pigeon`.
- **DMARC:** Policy published, advising receivers on how to handle failures.
- **Conclusion:** The domain has a robust email security configuration, and the sending IP matched its authorized SPF records.

> **📸 Screenshot:** ![Powershell](https://github.com/Major241/cyber-portfolio/blob/main/images/PS_quora.png.png?raw=true)
### 3.5 Infrastructure Mapping with DNSDumpster
**Goal:** Discover related subdomains and "IP neighbors" to identify potential malicious infrastructure clusters.

**Process:** I submitted the target domain to `dnsdumpster.com` to visualize its hosting environment and discover other domains sharing the same IP space.

**Key Findings:**
- The domain `quora.com` resolves to a large cloud provider (AWS/Cloudflare), which is expected for a major service.
- The IP neighbors were primarily other legitimate Quora subdomains and services (e.g., `o50.pigeon.quora.com`), with no obvious suspicious patterns. This is consistent with a legitimate enterprise.

**Screenshot Suggestion:** ![Dnsdumpster](https://github.com/Major241/cyber-portfolio/blob/main/images/dns_dpster.png.png?raw=true)

### 4. Conduct Reputation Checks
**Goal:** Cross-reference findings with global threat intelligence.
- Searched the domain `quora.com` and IP `18.214.110.142` on **VirusTotal** and **AbuseIPDB**.
- **Finding:** No known malicious history or detections were found across multiple security vendors.

> **📸 Screenshot Suggestion:** ![VirusTotal](https://github.com/Major241/cyber-portfolio/blob/main/images/VT-quora-IP.png.png?raw=true)

### 5. Formulate a Conclusion
The collective evidence was evaluated against common indicators of compromise (IOCs):

| Indicator | Finding for Quora | Implication |
| :--- | :--- | :--- |
| **Domain Age** | Registered since 1999 | ✅ Strong Legitimacy Indicator |
| **Email Auth (SPF/DKIM/DMARC)** | Fully Implemented | ✅ Strong Legitimacy Indicator |
| **IP/Domain Reputation** | No detections | ✅ Strong Legitimacy Indicator |

## 🧠 Analyst Conclusion
Based on the comprehensive analysis, the domain `quora.com` and the email originating from IP `18.214.110.142` are **confirmed to be legitimate**. The domain has a long history, implements all necessary email authentication protocols correctly, and has a clean reputation across threat intelligence platforms. This case provides a strong baseline for identifying trustworthy senders.

## 📚 Lessons Learned
This exercise solidified the standard SOC workflow for email investigation:
1.  **Headers are the starting point:** They contain the essential fingerprints for your investigation.
2.  **Triangulate evidence:** No single tool provides all the answers. WHOIS, DNS, and reputation checks must be used together for a high-confidence assessment.
3.  **Understand Email Auth:** SPF, DKIM, and DMARC are not just acronyms; they are critical, verifiable signals of a domain's health and security posture.
4.  **Tool Flexibility:** The same core DNS principles can be applied using different tools (`dig`, `nslookup`, `Resolve-DnsName`), which is crucial for operating in diverse environments.
5.  **Baseline the "Good":** Knowing what a legitimate domain looks like makes identifying the suspicious ones—those with new registration, missing policies, and poor reputation—much faster and easier.

---

## 🔗 References & Further Reading
- [SANS Internet Storm Center: Diary of a Malicious Domain Investigation](https://isc.sans.edu/)
- [CISA: Avoiding Social Engineering and Phishing Attacks](https://www.cisa.gov/uscert/ncas/tips/ST04-014)
- [RFC 7489: Domain-based Message Authentication, Reporting, and Conformance (DMARC)](https://tools.ietf.org/html/rfc7489)
```

---
