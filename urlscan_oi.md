### **🕵️ Practical Mastery of urlscan.io for Threat Intelligence**

A hands-on guide to using `urlscan.io` for web security analysis, focusing on its core functionality, analytical workflow, and critical nuances for accurate investigations.

---

#### **🔍 The Summary Tab: The Analyst's Dashboard**

The first page provides the high-level overview and key pivot points.

*   **Overall Score & Verdict:** The immediate visual indicator. A low, red score is a strong initial signal. The community verdict is useful but **not 100% reliable**—treat it as a signal, not a final verdict.
*   **Key Pivot - Main IP Address:** This is the most critical data point. Copy the IP and search it on **VirusTotal** to see if it's associated with other malicious activity.
*   **HTTP Server & Redirect Chain:** Reveals the server technology and the path of any redirects.

[![urlscan.io Summary Tab](https://github.com/Major241/cyber-portfolio/blob/main/images/urlscan_oi.png.png?raw=true)](https://github.com/Major241/cyber-portfolio/blob/main/images/urlscan_oi.png.png?raw=true)
*The summary tab provides the critical first look, including the score, community verdict, and key pivots like the IP address.*

---

#### **⚡ The Most Important Tab: Redirects**

This tab maps every hop from the initial URL to the final destination. The discrepancy between the **Submitted URL** and the **Effective URL** is a major red flag.

*   **Submitted URL:** Often a shortened or disguised link (e.g., `https://mub.me/ce1`).
*   **Effective URL:** The real, often highly suspicious destination (e.g., `http://sqd465qs6d5s.odeatech.com/...`).
*   **The Takeaway:** The Submitted URL is the disguised doorway. The Effective URL is the dangerous room you walk into.

---

#### **🔎 The DOM & Behavior Tabs**

*   **DOM Tab:** Inspect the fully rendered HTML to find hidden elements (e.g., `display: none;`), obfuscated scripts, or malicious form actions.
*   **Behavior Tab:** Reviews the page load against rules to flag known threats like cryptojacking or phishing kits. Like the verdict, this is a useful signal but not definitive.

---

#### **✅ The Investigative Workflow**

1.  **Submit** a suspicious link to urlscan.io (use **Private** visibility for sensitive URLs).
2.  **Analyze the Summary:** Check the score and, most importantly, **pivot on the IP** via VirusTotal.
3.  **Inspect the Redirects:** Identify the true destination and any deception.
4.  **Search the DOM** for keywords like "login" or "password" and hidden elements.
5.  **Correlate:** Use urlscan.io's **search** (`ip:1.2.3.4`) to find other malicious sites on the same infrastructure.

---

#### **🛠️ Tools Used**
-   **urlscan.io** (Core Analysis)
-   **VirusTotal** (IP & Hash Reputation Checks)

**Source:** [urlscan.io](https://urlscan.io)

---
