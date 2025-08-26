### **Project: Forensic Analysis of a Phishing Email**
**Date: February 26, 2024 | Level: Beginner | Category: Digital Forensics & Incident Response (DFIR)**

## Objective
To perform a hands-on forensic analysis of a phishing email by examining its headers to identify key Indicators of Compromise (IOCs) and understand the attack vector.

## Executive Summary
Analysis of a suspicious email claiming to be from Banco Bradesco confirmed it was a phishing attempt. The email originated from a suspicious cloud server (`137.184.34.4`) and failed all critical email authentication checks (SPF, DKIM, DMARC). The body was obfuscated with Base64 encoding to hide the final malicious link.

## Tools Used
- **Text Editor** (for manual header analysis)
- **VirusTotal** (for IOC reputation checking)
- **Evidence File:** Withheld. The original `.eml` file contains an active malicious URL and has been withheld to prevent accidental exposure. Analysis was performed on a private copy.

## Indicators of Compromise (IOCs)
| Indicator Type | Value | Notes |
| :--- | :--- | :--- |
| **Originating IP** | `137.184.34.4` | The source IP address of the attacker's server. |
| **Sending Hostname** | `ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06` | Indicates a disposable cloud server, likely from DigitalOcean. |
| **Spoofed From Domain** | `atendimento.com.br` | Used to impersonate the legitimate `bradesco.com.br` domain. |

## Practical Analysis Steps

### 1. **Header Analysis: Tracing the Origin**
I read the `Received:` headers from the bottom up to find the original source.
- **Key Finding:** The oldest header was: `Received: from ubuntu-s-1vcpu-1gb-35gb-intel-sfo3-06 (137.184.34.4) by BN8NAM11FT066.mail.protection.outlook.com...`
- **Conclusion:** The email did not come from Banco Bradesco's infrastructure. It was sent from a low-cost, likely fraudulent, cloud server.

### 2. **Email Authentication: A Definitive Fail**
I located and interpreted the `Authentication-Results:` header.
```
Authentication-Results: spf=temperror; dkim=none; dmarc=temperror; compauth=fail
```
- **SPF:** Failed with a temporary error.
- **DKIM:** The email lacked a digital signature entirely.
- **DMARC:** Failed with a temporary error.
- **Conclusion:** The email failed all authentication methods. A legitimate email would pass these checks.

### 3. **Social Engineering Lure**
- **Subject Line:** `CLIENTE PRIME - BRADESCO LIVELO: Seu cartão tem 92.990 pontos LIVELO expirando hoje!` (Translation: *Your card has 92,990 points expiring today!*)
- **Tactic:** Uses urgency and a high-reward offer to pressure the victim into clicking.

### 4. **VirusTotal IOC Check (Practical Step)**
I took the IOC I discovered and proactively checked its reputation.
1.  I navigated to [VirusTotal.com](https://www.virustotal.com).
2.  I entered the IP address **`137.184.34.4`** into the search bar.
3.  **Result:** As of February 2024, this IP has been flagged by multiple security vendors for malicious activity. [View the full report on VirusTotal](https://www.virustotal.com/gui/ip-address/137.184.34.4).
- **Conclusion:** The originating IP has a known bad reputation, confirming the malicious nature of this email.

### 5. **Body & Payload**
- The email body was encoded in **Base64**, a common technique to hide malicious links from basic filters.
- **Next Practical Step:** To safely extract the hidden link, I would submit the `.eml` file to a sandbox service like **Any.Run** or **Hybrid-Analysis**. These tools automatically decode and analyze the content in a safe environment, revealing the final phishing URL.

## Conclusion
This email is a confirmed phishing attempt. The evidence is conclusive: it was sent from a known malicious IP, failed all email authentication protocols, and used a social engineering lure. The final payload was hidden through encoding.

## Mitigation & Response
In a real-world scenario, the response would include:
1.  **Blocking the IOC:** Adding the IP `137.184.34.4` to network and email gateway blocklists.
2.  **User Awareness:** Notifying users of this specific phishing campaign targeting the brand.
3.  **Enhancing Detection:** Configuring security tools to flag emails that fail DMARC or come from similar cloud hosting IP ranges.

## References
- [VirusTotal Analysis for 137.184.34.4](https://www.virustotal.com/gui/ip-address/137.184.34.4)
- [Any.Run Interactive Sandbox](https://any.run/)
- [CISA: Security Tips (ST04-014)](https://www.cisa.gov/news-events/news/avoiding-social-engineering-and-phishing-attacks)
