# SOC Incident Response Lab

A collection of hands-on, fictional SOC alert investigations. This lab simulates the end-to-end process of responding to security incidents following the NIST IR lifecycle, from initial alert to containment and eradication.

## 🚨 Lab Overview

This project demonstrates the analytical process of a Security Operations Center (SOC) Analyst by responding to realistic security alerts. For each incident, I act as the analyst, requesting data from simulated sources to triage, investigate, contain, and resolve the threat.

**Key Skills Practiced:**
- **Incident Triage & Analysis**
- **Digital Forensics & Evidence Collection**
- **Threat Intelligence Correlation**
- **Containment & Eradication Strategy**
- **Documentation & Ticketing**

## 🔍 Lab Structure

Each lab is contained in its own directory and includes:
- `alert.md`: The initial SIEM alert that triggered the investigation.
- `investigation.md`: The step-by-step investigation process, including my queries and the simulated data responses.
- `ticket.md`: The final incident ticket, documenting the timeline, findings, and resolution.

## 📂 Incident 1: Brute Force Attack on Privileged Service Account

**Alert:** `HV001: Suspicious Volume of Failed Logins - Potential Brute Force`
**Summary:** An alert triggered for 32 failed login attempts to a VPN service account (`svc_jenkins`) followed by one success from a known malicious IP.

### Investigation Highlights:
- **Triage:** Verified the user was a privileged service account with outdated credentials.
- **Threat Intel:** Confirmed the source IP was associated with bulletproof hosting and malicious activity.
- **Impact Analysis:** Discovered an outbound C2 connection and ~15MB of data exfiltration post-compromise.
- **Response:** Contained the threat by disabling the account and blocking the IP, then eradicated by enforcing password policies and implementing VPN MFA.

**Key Takeaway:** The critical importance of MFA for all remote access and strict password policies for service accounts.

## 📂 Incident 2: Web Server Compromise via LOLBins

**Alert:** `EXEC-ALERT-007: Execution of LOLBin followed by Network Call`
**Summary:** A critical alert detected the execution of `certutil.exe` by a web service account (`iis_service`) to download a file from a malicious domain.

### Investigation Highlights:
- **Triage:** Identified the account as a service account with web server permissions.
- **Threat Intel:** Confirmed the destination domain was a known malware distribution site.
- **Root Cause Analysis:** Correlated the event with IIS logs to find a successful WordPress plugin exploit.
- **Response:** Contained the host immediately, eradicated the downloaded malware, and initiated patching for the web application vulnerability.

**Key Takeaway:** The necessity of robust web application security and the ability to recognize Living Off the Land Binaries (LOLBins) in attacker workflows.

## 📂 Incident 3: Phishing & Credential Harvesting Attempt

**Alert:** `EMAIL-ALERT-101: Suspected Phishing Email Delivered to Mailbox`
**Summary:** A high-severity alert for a phishing email with a malicious link, sent to a high-value helpdesk account.

### Investigation Highlights:
- **Triage:** Analyzed sender domain and confirmed spoofing indicators (new domain, no DMARC).
- **Breach Check:** Discovered the target account had a weak, compromised password.
- **Response:** Purged the malicious email from all mailboxes and enforced a password reset for the vulnerable account.

**Key Takeaway:** Proactive credential hygiene is as important as detecting the initial phishing email.
## 📋 Example Incident Ticket: INC-2023-1027-001

**Title:** Suspicious VPN Login Activity for svc_jenkins
**Status:** Resolved
**Priority:** High
**Assigned Analyst:** Marvel

| Field | Details |
| :--- | :--- |
| **Description** | SIEM Alert HV001 triggered at 14:05 UTC. 32 failed login attempts from 203.0.113.55 to svc_jenkins account on VPN gateway, followed by a successful authentication. |
| **Investigation Timeline** | `14:07 UTC` - Alert acknowledged. Investigation begun. <br> `14:09 UTC` - AD query: `svc_jenkins` is a privileged service account. <br> `14:11 UTC` - Threat Intel: Source IP `203.0.113.55` is confirmed malicious. <br> `14:13 UTC` - Firewall query: Found outbound C2 connection from VPN IP `10.8.0.17` to known C2 infrastructure. <br> `14:15 UTC` - **CONTAINMENT: Disabled user `svc_jenkins`. Blocked IP `10.8.0.17` at the firewall.** <br> `14:18 UTC` - **ERADICATION: Reset password for `svc_jenkins` and enforced 90-day expiration policy.** |
| **Assets Involved** | **User:** `svc_jenkins` <br> **Source IP:** `203.0.113.55` <br> **Host:** `vpn.corporate[.]com` |
| **Findings & Analysis** | Privileged service account was compromised via a brute-force attack from a known malicious IP. The attacker established a C2 channel and exfiltrated approximately 15MB of data. |
| **Containment Actions** | 1. Disabled compromised service account `svc_jenkins`. <br> 2. Blocked malicious VPN-assigned IP `10.8.0.17` at the network perimeter. |
| **Eradication & Recovery** | 1. Reset password for the account to a new strong credential. <br> 2. Removed "PasswordNeverExpires" flag and set a 90-day expiration policy. |
| **Root Cause** | Successful brute-force attack against a privileged service account due to a static, never-expiring password. |
| **Resolution** | Contained active attack, eradicated the root cause by resetting credentials, and initiated a policy change to implement MFA for all VPN logins. |
## 🚀 How to Use This Lab

1.  Review the `alert.md` for a specific incident.
2.  Read the `investigation.md` and think about what your next query would be before reading my action.
3.  Compare your thought process with my investigation path.
4.  Review the completed ticket in `ticket.md` to see the full documentation.

## ✅ Purpose

This lab was created to practice and demonstrate the core analytical skills required in SOC incident response. The simulated environment allows for focus on the investigative process and decision-making without the need for a complex lab setup.

---

**📌 Hashtags:** `#SOC #IncidentResponse #DFIR #Cybersecurity #SOCAnalyst #TryHackMe #HackTheBox #BlueTeam #SIEM #EDR #ThreatHunting #CyberLab #NIST #LOLBins`

**Disclaimer:** This is a fictional lab using simulated data. All IP addresses, domains, and user details are either randomly generated or known malicious indicators used for educational purposes only.
