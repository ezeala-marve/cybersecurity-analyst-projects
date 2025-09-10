### **🕵️ Passive DNS Pivoting: From Phishing URL to Malicious Infrastructure**

A real-world investigation tracing a single phishing domain to a massive malicious hub using passive DNS analysis, revealing the attacker's core infrastructure and tactics.

---

#### **🔍 The Initial Indicator**

Analysis began with a suspicious domain:
**`canada-neflxt[.]com`**

---

#### **⚡ The Investigation: Step-by-Step Pivots**

1.  **VirusTotal Analysis:** Confirmed the domain was malicious (2/95 detections).
2.  **First Pivot (Domain → IP):** Found the resolving IP: **`159.89.228.225`**.
3.  **Core Pivot (Reverse DNS):** VirusTotal's passive DNS revealed the IP hosted **hundreds of phishing domains** (e.g., `degreedescribeown.shop`), indicating a large-scale operation.
4.  **Validation & Attribution:** SecurityTrails reverse IP lookup identified the attacker's core infrastructure: 
#### **⚡ Corroborating with other Tools**

A reverse IP lookup on **SecurityTrails** validated the findings and identified the attacker's core, persistent infrastructure: **`jwalker.pro`**.
**`jwalker.pro`**. A WHOIS lookup revealed NameCheap as the registrar and DigitalOcean as the host.

---

#### **📊 Visual Evidence**

[![VirusTotal Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/cana_vt.png.png?raw=true)](https://github.com/Major241/cyber-portfolio/blob/main/images/cana_vt.png.png?raw=true)
*VirusTotal analysis of the initial phishing domain.*

[![Passive DNS Pivot](https://github.com/Major241/cyber-portfolio/blob/main/images/vt_ip.png.png?raw=true)](https://github.com/Major241/cyber-portfolio/blob/main/images/vt_ip.png.png?raw=true)
*The critical pivot: Passive DNS reveals hundreds of malicious sibling domains on the same IP.*

[![SecurityTrails Reverse IP](https://github.com/Major241/cyber-portfolio/blob/main/images/securitytrail.png.png?raw=true)](https://github.com/Major241/cyber-portfolio/blob/main/images/securitytrail.png.png?raw=true)
*SecurityTrails confirms the malicious IP and reveals the attacker's core domain.*

---

#### **🧠 Key TTPs Uncovered**

The threat actor's **Tactics, Techniques, and Procedures (TTPs)** included:
*   **Resource Development (T1583):** Using DigitalOcean for scalable, cheap hosting.
*   **Acquire Infrastructure (T1583.001):** Registering domains with NameCheap.
*   **Phishing (T1566):** Deploying hundreds of randomly-generated `.shop` domains for campaigns.
*   **Establish Accounts (T1585.001):** Maintaining a persistent `.pro` domain for core infrastructure.

---

#### **✅ Actionable IOCs & Mitigation**

**Indicators of Compromise (IOCs):**
| Type | Indicator | Description |
| :--- | :--- | :--- |
| Domain | `canada-neflxt[.]com` | Initial Phishing Domain |
| IPv4 | `159.89.228.225` | Malicious Hub Server |
| Domain | `jwalker[.]pro` | Attacker's Core Infrastructure |

**Recommended Actions:**
1.  **Block** all IOCs at network perimeter.
2.  **Report** the IP to **DigitalOcean Abuse** and the domain to **NameCheap Abuse**.
3.  **Hunt** for other clusters of random domains on DigitalOcean IPs.

---

#### **🛠️ Tools Used**
-   **VirusTotal** (Core Analysis & pDNS)
-   **SecurityTrails** (Reverse IP Lookup)
-   **WHOIS** (Attribution)

---
