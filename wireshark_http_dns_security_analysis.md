### **🛡️ HTTP & DNS Security Analysis with Wireshark**

A hands-on Wireshark analysis demonstrating the critical security risks of unencrypted web traffic and how DNS can be abused for malicious purposes.

---

#### **🔍 Key Findings & Visual Proof**

**1. HTTP: Plaintext Credential Sniffing**
*   **Theory:** HTTP transmits all data in plaintext, including passwords.
*   **Proof:** Using the filter `http.request.method == "POST"`, I captured a login request and extracted the credentials directly from the packet.
*   **Takeaway:** Without HTTPS (TLS), credentials are easily stolen on open networks.

![HTTP Plaintext Capture](https://github.com/Major241/cyber-portfolio/blob/main/images/wireshark_http.png.png?raw=true)

**2. DNS: The Covert Threat Channel**
*   **Theory:** Attackers abuse DNS for data exfiltration and command-and-control (C2).
*   **Proof:** Analyzed DNS queries (`port 53`) to understand normal vs. malicious patterns.
*   **Takeaway:** Monitoring DNS is crucial for early threat detection.

![DNS Analysis](https://github.com/Major241/cyber-portfolio/blob/main/images/wireshark_dns.png.jpeg?raw=true)

---

#### **⚡ Practical Threat Hunting Filters**

Use these Wireshark filters to detect malicious activity:
*   **HTTP Exfiltration:** `http.request.method == "POST"`
*   **Suspicious Logins:** `http.host contains "login"`
*   **Password Keywords:** `http contains "password"`
*   **DNS Tunneling:** Look for abnormally long subdomains and `TXT` record queries.

---

#### **🛠️ CLI Tools for Rapid Response**

```bash
# Manual DNS lookup for investigation
dig @8.8.8.8 google.com A
dig google.com TXT
nslookup google.com

# Lightweight packet capture for servers
sudo tcpdump -i eth0 -w dns_capture.pcap port 53
```

---

#### **✅ Conclusion**

This practical exercise underscores why encryption (HTTPS) is non-negotiable and why DNS monitoring is a critical layer of defense for detecting covert cyber attacks.
