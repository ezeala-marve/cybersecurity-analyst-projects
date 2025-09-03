**A Security-Focused Analysis of HTTP and DNS with Wireshark**

**Introduction:** Move beyond theory. This post documents my hands-on journey dissecting HTTP and DNS packets to understand the fundamental security implications of these protocols. We'll cover everything from plaintext password sniffing to identifying malicious DNS patterns.

---

### **Part 1: HTTP - The Protocol That Shows Why Encryption Isn't Optional**

#### **The Theory: HTTP vs. HTTPS**
*   **HTTP:** The postcard. Every piece of data is transmitted in plain, human-readable text. Anyone who intercepts it can read it.
*   **HTTPS:** The sealed letter. HTTP is wrapped in a layer of encryption called **TLS/SSL**. The data is encrypted between your browser and the web server, making it gibberish to any interceptor.

#### **The Practical Proof: Sniffing a Plaintext Login**
*(https://github.com/Major241/cyber-portfolio/blob/main/images/wireshark_http.png.png?raw=true)*
My Wireshark capture proves the theory. The filter `http.request.method == "POST"` instantly located the form submission packet. Expanding the `HTML Form URL Encoded` section revealed all form fields in plaintext.

**The Security Takeaway:** On an open Wi-Fi network, an attacker doesn't need to "hack" anything. They simply run Wireshark, apply the same filter, and harvest credentials from anyone logging in over HTTP. This is a low-skill, high-impact attack.

#### **The Defender's Tool: Wireshark Filters for Threat Hunting**
This isn't just about proving a point; it's about building a detective's toolkit.
*   `http.request.method == "POST"`: The top filter for finding data exfiltration or login attempts.
*   `http.host contains "login"`: Discovers authentication traffic to various subdomains.
*   `http contains "password"`: A broad but effective search for the keyword in any HTTP packet.

**Why TLS is Non-Negotiable:** TLS encryption prevents this entire class of attack. In my capture, if the form had been served over HTTPS, the `Application Data` section of the TLS protocol would have been encrypted, showing only ciphertext. This is the single most critical control for protecting data in transit.

---

### **Part 2: DNS - The Internet's Phonebook and a Hacker's Covert Channel**

#### **The Theory: How DNS Resolution Works**
It's a simple question-and-answer protocol:
1.  **Client Query:** "What is the IP address for `google.com`?" (Type `A` or `AAAA`).
2.  **Server Response:** "The IP address is `142.251.42.206`."

#### **The Practical Proof: Dissecting the Conversation**
*(Use your DNS screenshot here)*
Using `ping -c 1 google.com` on a minimal Linux system triggered the DNS lookup. The Wireshark capture, filtered with `port 53`, shows the entire conversation:
*   **The Query (Question):** A request for the `A` record of `google.com`.
*   **The Response (Answer):** The answer containing the IP address in the `Answers` section.

#### **From Normal to Malicious: Analyzing DNS Traffic**
This is where we go from analysis to threat hunting.

**Normal DNS Traffic:**
*   Occasional, sporadic queries to legitimate domains (google.com, github.com).
*   Primarily `A` and `AAAA` record types.

**Suspicious DNS Traffic - What SOC Analysts Hunt For:**
*   **Weird, Long Subdomains:** The hallmark of **DNS tunneling/exfiltration**. Data is encoded into subdomains.
    *   *Normal:* `drive.google.com`
    *   *Malicious:* `a1B2c3D4e5F6g7H8i9J0kL1mN2oP3qR4sT5uV6wX7yZ8.stolen-data.evil[.]com`
*   **Repeated TXT Record Queries:** `TXT` records can hold arbitrary text. Attackers use them to send commands to malware through DNS responses, often bypassing firewalls.
*   **High Query Volume:** A machine making thousands of requests to random, non-existent domains (NXDOMAIN responses) might be malware searching for its dormant command-and-control (C2) server.

**Why SOCs Monitor DNS:** Before malware can steal data or receive commands, it must "phone home" to its C2 server. The first step of that call is almost always a DNS lookup. Catching this lookup is a critical early warning signal to isolate a machine before a major breach occurs.

#### **The Investigator's Toolkit: CLI Tools for DNS**
While Wireshark is for deep inspection, CLI tools are for rapid response:
*   **`dig @8.8.8.8 google.com A`:** The precision tool for manual DNS lookup.
*   **`dig google.com TXT`:** Specifically checks for TXT records, crucial for investigating SPF policies or malicious commands.
*   **`nslookup google.com`:** The classic interactive tool for quick checks.
*   **`sudo tcpdump -i eth0 -w dns_capture.pcap port 53`:** The go-to for lightweight, continuous packet capture on a server, which can be analyzed later in Wireshark.

---

### **Conclusion: Connecting the Dots**

This analysis moves from the basic functionality of protocols to their critical role in security. Understanding that HTTP exposes data and that DNS can be abused for covert communication is foundational for any security practitioner. These skills are not academic; they are the first steps in detecting and preventing real-world attacks, from credential theft to full-scale data exfiltration.

---
