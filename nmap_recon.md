# Nmap Recon: scanme.nmap.org

**TL;DR:** Performed an Nmap reconnaissance scan (including version detection, OS detection, and NSE scripts) against the authorized target `scanme.nmap.org`. The scan successfully identified running services, operating system details, and a potential vulnerability flagged by Nmap's `--script vuln` mapping to **CVE-2007-6750** (Slowloris DoS).

> **Legal / Scope:** Scans were performed only against `scanme.nmap.org`, the Nmap project's official test host. Never scan targets you do not own or have explicit permission to test.

---

## 🎯 Goal
Identify open ports, running services and versions, OS fingerprint, and run Nmap vulnerability scripts to highlight potential CVEs for follow-up validation.

## 🧰 Environment
- **Target:** `scanme.nmap.org` (Nmap test host)
- **Scan machine:** Windows 11 (PowerShell)
- **Nmap version:** 7.95
- **Files included in this repo:**
  - `/screenshots/nmap_scan_results.png` — screenshot of terminal output (see below)

---

## 🔧 Commands used
The core commands executed and validated in this session were:

**```powershell**
# 1. Initial Vulnerability Scan
nmap -Pn --script vuln scanme.nmap.org

# 2. Aggressive Scan with output to a file
nmap -A --script vuln -oN scanme_aggressive_scan.txt scanme.nmap.org

# 3. Version Detection and OS Detection Scan (the main combined scan)
nmap -sS -sV -O scanme.nmap.org

# 4. Fast scan to verify basic connectivity and open ports
nmap -Pn -F scanme.nmap.org


**Flags we explicitly used and verified:**
- `-A`: Aggressive scan (enables OS detection, version detection, script scanning, and traceroute).
- `-oN`: Outputs the scan results in normal (human-readable) format to the specified text file.
- `-Pn`: Skips host discovery, assumes host is up. (Used because the target blocks pings).
- `--script vuln`: Runs Nmap's vulnerability script category.
- `-sS`: TCP SYN Stealth Scan for port discovery.
- `-sV`: Service Version Detection.
- `-O`: OS Detection.
- `-F`: Fast scan (100 most common ports).

---

**Notes on flags used:**
- `-sS`: TCP SYN Stealth Scan. A fast, less intrusive method to discover open ports.
- `-sV`: Service/version detection. Probes open ports to identify the exact application and version.
- `-O`: OS detection. Attempts to determine the host's operating system from its network stack behavior.
- `--script vuln`: Runs Nmap Scripting Engine (NSE) scripts that check for known vulnerabilities against the detected services.
- `-A`: Aggressive scan (enables OS detection, version detection, script scanning, and traceroute). A powerful combination for detailed reconnaissance.

---

## 📊 Results summary

**Key findings:**
- **Open ports & services:**
  - `22/tcp` open — `OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13`
  - `80/tcp` open — `Apache httpd 2.4.7 ((Ubuntu))`
- **OS fingerprint:** Linux 3.2 - 4.9
- **Vulnerability script output:** Nmap's `--script vuln` identified a potential vulnerability: **CVE-2007-6750** (Slowloris Denial-of-Service attack). The script reported the target as `LIKELY VULNERABLE`.

**Screenshot of the PowerShell output:**
*(The screenshot below shows the results of the `nmap -sS -sV -O scanme.nmap.org` command, which provided the service and OS details used in this report.)*

![Nmap Scan Results](screenshots/nmap_scan_results.png)

**Important Disclaimer:** Nmap's vuln scripts map service/version identifiers to known CVEs and emit *potential* matches. This indicates the presence of a version associated with a known vulnerability, not confirmed exploitability in the specific target environment. **This finding was on a dedicated test server.** Always conduct follow-up validation in a controlled, authorized environment before declaring a confirmed vulnerability.

---

### **Attacker's Perspective:**
This intelligence provides a clear attack vector. The specific software versions (`Apache 2.4.7`, `OpenSSH 6.6.1p1`) are prime for querying exploit databases. The confirmed OS (Linux) dictates the use of Linux-compatible payloads.

### **Defender's Perspective:**
This report is a actionable hardening guide. The immediate actions would be to:
1.  **Patch:** Upgrade the identified software to the latest versions.
2.  **Harden:** Configure the web server to mitigate the Slowloris DoS vulnerability.
3.  **Reduce Surface:** Justify every open port. Should SSH be publicly accessible?
4.  **Monitor:** Alert on Nmap scan patterns (`-sS`, `-sV`) as potential reconnaissance activity.

This exercise highlights how the same information is used to both launch attacks and build defenses.
