# Passive DNS Pivoting Investigation: From Phishing URL to Malicious Infrastructure

## 📖 Overview

This document details a real-world cybersecurity investigation using **Passive DNS (pDNS) pivoting** to analyze a phishing domain. Starting from a single suspicious URL, we used **VirusTotal** and **SecurityTrails** to uncover a large-scale phishing operation hosted on a malicious server, identified the attacker's core infrastructure, and outlined mitigation steps.

## 🔍 The Initial Indicator

The investigation began with a potentially malicious domain attempting to impersonate a legitimate streaming service:
**`canada-neflxt[.]com`**

## 🕵️‍♂️ Investigation Steps

### Step 1: Analysis on VirusTotal

The first step was to confirm the malicious nature of the domain using VirusTotal.

**Finding:** The domain was flagged by 2/95 security vendors, confirming it was part of a phishing campaign.

![VirusTotal Domain Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/cana_vt.png.png?raw=true)
*Screenshot 1: VirusTotal analysis of `canada-neflxt[.]com` showing malicious detections.*

### Step 2: The Critical Pivot - Domain to IP

Within the VirusTotal "Relationships" graph, we identified the IP address the domain resolved to: **`159.89.228.225`**.

### Step 3: The Core Pivot - Reverse DNS Lookup

We pivoted on the IP address within VirusTotal to perform a reverse DNS lookup, viewing the "Passive DNS Replication" data.

**Finding:** The IP was a malicious hub, hosting **hundreds of other phishing domains** (e.g., `degreedescribeown.shop`, `drogfarabls.shop`). This pattern of randomly generated `.shop` domains is a strong indicator of a large-scale phishing operation.

![VirusTotal Passive DNS](https://github.com/Major241/cyber-portfolio/blob/main/images/vt_ip.png.png?raw=true)
*Screenshot 2: VirusTotal's Passive DNS results for the IP, showing a list of malicious sibling domains.*

### Step 4: Corroboration with SecurityTrails

To validate our findings with a second source, we performed a reverse IP lookup on SecurityTrails.

**Finding:** SecurityTrails confirmed that the IP **`159.89.228.225`** was currently hosting two domains: **`jwalker.pro`** and **`www.jwalker.pro`**. This identified the attacker's more permanent infrastructure among the many temporary phishing domains.

![SecurityTrails Reverse IP](https://github.com/Major241/cyber-portfolio/blob/main/images/securitytrail.png.png?raw=true)
*Screenshot 3: SecurityTrails reverse IP lookup results showing the associated domains.*

### Step 5: Attribution via WHOIS Lookup

We investigated the registration details of the discovered domain `jwalker.pro` to understand the attacker's infrastructure.

**Findings:**
*   **Registrar:** NameCheap, Inc.
*   **Registration Date:** 2022-09-28 (Indicating a persistent operation)
*   **Hosting Provider:** DigitalOcean, LLC (Based on name servers)
*   **Registrant Info:** The registrant name,organization and address were publicly visible in the WHOIS record. 

![WHOIS Lookup](https://github.com/Major241/cyber-portfolio/blob/main/images/whois_jw.png.png?raw=true)
*Screenshot 4: WHOIS results for `jwalker.pro` showing registrar and registration details.*

## 🧠 Key Lessons Learned

1.  **The Power of the Pivot:** A single malicious indicator (IOC) is never just one indicator. It is a gateway to discovering a wider network of malicious activity.
2.  **IP Addresses Are Key:** Domains can change quickly, but IP addresses have a history. Pivoting from a domain to its IP and then to all other domains on that IP is the most powerful step in uncovering attack scale.
3.  **Correlation is Crucial:** Using multiple tools (VirusTotal, SecurityTrails) provides a more complete picture and validates your findings.
4.  **Attacker TTPs:** This actor's tactics, techniques, and procedures (TTPs) included:
    *   Using DigitalOcean for cheap, scalable hosting.
    *   Registering domains with NameCheap under privacy protection.
    *   Deploying hundreds of random, generated `.shop` domains for phishing campaigns.
    *   Maintaining a longer-lasting `.pro` domain as a core part of their infrastructure.

## ✅ Recommended Mitigation & Actions

Gathering intelligence is only valuable if acted upon. Here are the crucial next steps:

1.  **Immediate Blocking:** Add all discovered domains and the IP address **`159.89.228.225`** to blocklists on firewalls, web filters, and email gateways.
2.  **Reporting for Takedown:**
    *   **Report the IP** to the **DigitalOcean Abuse Team** via their [official abuse report form](https://www.digitalocean.com/trust/report-abuse). Provide them with the evidence gathered here.
    *   **Report the domain** `jwalker.pro` to the **NameCheap Abuse Team** at `abuse@namecheap.com`.
3.  **Threat Intelligence Sharing:** Share the IOCs (domains, IPs) with your internal security team and relevant industry threat-sharing groups (ISACs).
4.  **Hunting:** Proactively search for other clusters of randomly-named domains on DigitalOcean IPs, as they are likely related to this same threat actor.

## 📋 Indicator of Compromise (IOC) List

| Type | Indicator | Description |
| :--- | :--- | :--- |
| Domain | `canada-neflxt[.]com` | Initial Phishing Domain |
| IPv4 | `159.89.228.225` | Malicious Hub Server |
| Domain | `jwalker[.]pro` | Attacker's Core Infrastructure |
| Domain | `degreedescribeown[.]shop` | Example of Phishing Sibling |
| Domain | `drogfarabls[.]shop` | Example of Phishing Sibling |

## 🛠️ Tools Used

*   [**VirusTotal**](https://www.virustotal.com) - Core analysis and pDNS pivoting
*   [**SecurityTrails**](https://www.securitytrails.com) - Reverse IP lookup and data validation
*   [**WHOIS**](https://www.whois.com) - Domain registration attribution

---

**Disclaimer:** This investigation was conducted for educational purposes. All IOCs are from a known malicious campaign and are used here to demonstrate forensic techniques.
