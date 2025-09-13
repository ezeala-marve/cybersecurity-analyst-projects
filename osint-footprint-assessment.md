# OSINT & Digital Footprint Analysis: A Self-Assessment

**TL;DR**
**Ethical OSINT Self-Assessment:** Executed a structured Open-Source Intelligence (OSINT) workflow to evaluate my personal digital footprint. The investigation involved automated social media discovery with **Sherlock**, multi-engine search aggregation (**Google, DuckDuckGo, Bing**), and critical breach database validation via **Have I Been Pwned**.

**Key Finding & Action:** While my active public footprint is minimal, the scan confirmed exposure of old credentials in historical third-party breaches. This finding underscores the non-negotiable importance of **password hygiene** (using a password manager) and universal **2-Factor Authentication (2FA)** enforcement—principles I actively practice and advocate for in security best practices.

---

## Objective
To perform a systematic, ethical self-assessment of my digital footprint using OSINT methodologies. The goal was to identify publicly available information, assess exposure from data breaches, and understand the associated personal security risks.

## ⚠️ Legal & Ethical Disclaimer
> **This investigation was conducted solely on myself for educational and self-hardening purposes.** All techniques were applied ethically to my own information. Unauthorized use of OSINT against others is illegal and unethical.

## 🔧 Tools & Techniques Used
- **Search Engines:** Google, DuckDuckGo, Bing
- **Breach Database:** Have I Been Pwned (HIBP)
- **Username Hunting:** Sherlock (Python tool)
- **Methodology:** Multi-phase workflow (detailed below)

## 🗺️ The OSINT Workflow

### Phase 1: Search Engine Reconnaissance
**Goal:** Discover publicly indexed information across major search platforms.
- **Action:** Queried `"My Name"` and variants with identifiers (location, profession) on Google, DuckDuckGo, and Bing.
- **Finding:** Minimal public footprint. No definitive social media profiles or personal details were discovered in search results, indicating strong privacy settings and/or minimal public sharing.

### Phase 2: Breach Database Validation
**Goal:** Audit historical exposure of my credentials in known data breaches.
- **Action:** Submitted my primary email to `https://haveibeenpwned.com`.
- **Finding:** The email was **found in historical breaches**. Old passwords (now retired) and account details were exposed in third-party incidents. This is a critical reminder of the importance of unique passwords and 2FA.

### Phase 3: Username Enumeration with Sherlock
**Goal Automate the discovery of accounts associated with a known username across hundreds of sites.
- **Action:** Installed and ran the Sherlock tool via the command `sherlock <my_username>`.
- **Finding:** The tool efficiently returned a short list of platforms where the username was registered. Manual review confirmed these were old or inactive accounts, aligning with the finding of a minimal *active* footprint.

## 📊 Risk Assessment & Synthesis
The synthesized findings from all phases present a low-to-moderate risk profile:
- **Low Attack Surface:** A minimal active public presence provides little material for targeted social engineering.
- **Moderate Credential Risk:** Historical breach exposure highlights the danger of password reuse. This risk is mitigated by the use of a password manager to generate and store unique, strong passwords for every service, and the enforcement of 2FA wherever possible.

## ✅ Conclusion & Recommendations
This self-assessment proved the value of systematic OSINT in personal security. The key takeaway is not the presence of a breach, but the necessary response to it.

**Proactive Recommendations for Everyone:**
1.  **Conduct Regular Self-Checks:** Use this workflow annually to audit your digital footprint.
2.  **Use a Password Manager:** This is the single most effective defense against credential stuffing attacks arising from breaches.
3.  **Enforce 2FA Universally:** Always enable multi-factor authentication on every account that supports it.
4.  **Review Privacy Settings:** Regularly audit the privacy settings on social media platforms.

#OSINT #CyberSecurity #DigitalFootprint #Privacy #InfoSec #SelfAudit #HIBP #Sherlock #PasswordManager #2FA

---
