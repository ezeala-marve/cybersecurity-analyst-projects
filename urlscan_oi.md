# Practical Mastery of urlscan.io for Threat Intelligence

A hands-on, comprehensive guide to using `urlscan.io` for web security analysis, covering its powerful functionality, practical workflow, and important analytical nuances.

## Overview

`urlscan.io` is an indispensable tool in a security analyst's toolkit. It acts as a remote, isolated browser, fetching website content safely and providing a treasure trove of data for investigation. This guide is based on my practical experience using the tool to analyze suspicious links, focusing on how to leverage its features and understand its signals.

## Key Functionality & Practical Analysis

### 1. The Summary Tab: The Analyst's Dashboard

The first page you see is your high-level overview and primary pivot point.

*   **Screenshot & Overall Score:** Your immediate visual and quantitative assessment. A clean, well-known site will have a high green score. A malicious site often has a low red score. This is your first indicator.
*   **Community Verdict:** A powerful crowd-sourced feature.
    *   **How I use it:** A high number of "malicious" votes is a massive red flag and a strong signal from the community.
    *   **The Critical Nuance:** This verdict is **not 100% reliable**. It can suffer from false positives (e.g., a strict security scanner being flagged) and false negatives (a new phishing site that hasn't been voted on yet). **Always use it as a strong signal, not a sole verdict.**
*   **Main IP Address & ASN:** This is my most-used pivot point.
    *   **Practical Pivot:** Copy the IP and search it on **VirusTotal**. If other vendors have flagged files or URLs associated with this IP, it's a major red flag. The Autonomous System Number (ASN) reveals the hosting provider, which can often be a known bulletproof host.
*   **HTTP Server & Redirect Chain:** Shows the web server software (e.g., nginx, Apache) and the full path of any redirects.

![Summary Page of a urlscan.io result](https://github.com/Major241/cyber-portfolio/blob/main/images/urlscan_oi.png.png?raw=true)
*The summary tab provides the critical first look, including the score, verdict, and key pivots like the IP address.*

### 2. The DOM Tab: The Hidden Structure

The Document Object Model tab shows the HTML, scripts, and resources after the page has fully loaded in the browser.

*   **Finding Hidden Elements:** I look for hidden divs, obfuscated scripts, or comments that shouldn't be there. Malicious actors often hide phishing forms or links using CSS (`display: none;`).
*   **Identifying Key Resources:** It's a quick way to see all the external JavaScript libraries and fonts being loaded. A request to a known malicious domain here is a clear sign of compromise.

### 3. The Redirects Tab: Following the Breadcrumbs

This tab maps out every single hop from the initial URL to the final destination.

*   **Practical Use:** Explanation of Submitted URL vs. Effective URL

The discrepancy between the **Submitted URL** and the **Effective URL** is one of the most common and telling signs of a malicious or deceptive website.

*   **Submitted URL (`https://mub.me/ce1`):** This is the link that was provided to the victim. It's a shortened URL (using the `mub.me` domain, which is a known URL shortener). Attackers use shorteners to:
    1.  Hide the long, suspicious-looking real URL.
    2.  Make the link seem more benign or trustworthy.
    3.  Sometimes, to track how many people click the link.

*   **Effective URL (`http://sqd465qs6d5s.odeatech.com/...`):** This is the **real destination** after the URL shortener and any other redirects have been processed. This is the actual webpage that loads in your browser. This URL is highly suspicious:
    *   It uses **HTTP** (not secure HTTPS), meaning any data you enter is sent in plain text.
    *   The subdomain (`sqd465qs6d5s`) is a long, random string, a common technique to avoid blacklisting and create unique URLs for each victim.
    *   The path (`/RXJaSVZ3NVBhVFhsei9tOTFTcnd4R1RLZmdBVINmYW05`) is also long and random, likely containing unique identifiers for the campaign or target.
    *   The domain (`odeatech.com`) is being abused. It may have been a legitimate domain that expired and was purchased by attackers, or it may have been compromised.

**In simple terms:** The Submitted URL is the disguised doorway. The Effective URL is the dangerous room you actually walk into. The **Redirects tab** in urlscan.io shows you the exact path you took from the doorway to the room.

---

### 4. The Behavior Tab: The Action Report

This tab processes the page load and identifies specific, noteworthy actions using built-in and community rules.

*   **Tracking Detection:** Identifies known advertising and tracking networks.
*   **Malicious Flags:** Can detect scripts known for cryptojacking, phishing kits, and other threats.
*   **Note:** Like the verdict, this is a fantastic signal, but its effectiveness is limited by its rulesets. Novel threats may not be detected immediately.

### 5. Beyond the Scan: Search & API

urlscan.io is more than just a scanner; it's a searchable intelligence database.

*   **Search Syntax:** You can search for:
    *   IP addresses: `ip:1.2.3.4`
    *   ASNs: `asn:AS15169` (That's Google!)
    *   Hostnames: `domain:github.com`
    *   Hashes: `hash:md5_hash_here` (to find scans of downloaded files)
    *   This allows you to see what else is hosted on a suspicious IP, connecting isolated incidents to a broader campaign.
*   **API:** Allows for automation and integration into your own tools or workflows for scalable analysis.

## My Typical Investigative Workflow

1.  **Find a Suspicious Link:** In a phishing email, a social media post, or a security alert.
2.  **Submit to urlscan.io:** Paste the URL. Set "Visibility" to **Private** if the link is sensitive.
3.  **Analyze the Summary:**
    *   Check the **score** and **community verdict** (understanding its limitations).
    *   **Pivot the IP** to VirusTotal for reputation checks.
    *   Note the final domain and server info.
4.  **Investigate the Redirects:** See if the link was hiding its true destination.
5.  **Dig into the DOM:** Search for keywords like "login", "password", or look for hidden iframes and scripts.
6.  **Review Behavior:** Check for any automatically detected malicious behaviors.
7.  **Search for Related Activity:** Use the search bar to query the IP or domain to uncover the scope of the malicious infrastructure.

## Example Investigation

**Scenario:** A suspicious link: `http://netflix-support-login[.]verify[.]com`

1.  **Submission:** Scan the URL on urlscan.io.
2.  **Finding (Summary Tab):** The final URL is actually `http://45.76.123.222/nginxlp/netflix/`. The IP has a low reputation score on VirusTotal. The ASN is a known bulletproof host. The community verdict is 5/10 malicious.
3.  **Finding (Redirects Tab):** Shows one redirect from the original submitted URL to the IP address, confirming the deception.
4.  **Finding (DOM Tab):** Reveals a perfect replica of the Netflix login page, with the form action pointed to a `.php` file on the attacker's server.
5.  **Finding (Search):** Searching the IP `ip:45.76.123.222` reveals 15 other scans for different branded login pages (PayPal, Steam, Facebook).

**Conclusion:** Confirmed large-scale phishing operation. The technical pivots (IP, DOM, Redirects) provided the concrete evidence, while the community verdict and score provided supporting signals.

## Conclusion

urlscan.io is a powerful hub for initial threat investigation. Its genius is in providing the data to understand the *story* of a webpage—where it lives, what it's built with, where it goes, and what it does.

**The key to mastery is this:** Use its automated features (scores, verdicts, behavior detections) as powerful **signals**, but always base your final conclusion on your **manual analysis** of the technical evidence it provides (IPs, DOM, Redirects). Correlate your findings with other tools like VirusTotal for a robust analysis.

I highly recommend every aspiring security analyst get hands-on with this tool.
