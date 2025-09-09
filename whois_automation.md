### **🛠️ Building a WHOIS Automation Tool on Windows**

A practical project to overcome Windows limitations by installing the `whois` command and engineering a PowerShell script to automate domain intelligence gathering, replicating a core SOC analyst workflow.

---

#### **🔧 The Problem & Solution**

*   **The Problem:** Windows lacks a native `whois` command, a crucial tool for domain investigation.
*   **The Solution:** Source `whois.exe` from **Microsoft Sysinternals**, install it in `C:\Windows\`, and rename it for system-wide access.

---

#### **⚡ From Manual to Automated Analysis**

**Manual Command:**
```powershell
whois google.com | Select-String "Creation Date"
```

**Automated PowerShell Script (`investigate.ps1`):**
```powershell
$target = Read-Host "Enter the domain or IP address"
$results = whois $target

echo "`n[REPORT FOR $target]"
echo "===================================================`n"

echo "[+] Creation Date:"
$results | Select-String "Creation Date" | Get-Unique

echo "`n[+] Registrar:"
$results | Select-String "Registrar" | Get-Unique

echo "`n[+] Name Servers:"
$results | Select-String "Name Server" | Get-Unique

echo "`n[+] Country:"
$results | Select-String "Country" | Get-Unique

echo "`n"
pause
```

---

#### **🚀 Key Features & Output**

The script automates the extraction of key forensic indicators from a raw WHOIS data dump:
-   **Creation Date**
-   **Registrar**
-   **Name Servers**
-   **Country**

[![Final Output of the WHOIS Automation Script](https://github.com/Major241/cyber-portfolio/blob/main/images/auto_whois.png.png?raw=true)](https://github.com/Major241/cyber-portfolio/blob/main/images/auto_whois.png.png?raw=true)
*The final, structured output of the automated investigation script.*

---

#### **🔒 Execution Policy Fix**

To run the script, you must first allow PowerShell to execute local scripts:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

#### **✅ Practical SOC Application**

This tool replicates a real analyst's workflow for triaging phishing domains. A newly registered domain with a dodgy registrar and bulletproof hosting, as revealed by the script, is a major red flag for confirming malicious activity.

---

#### **🛠️ Tools Used**
-   Microsoft Sysinternals WhoIs
-   Windows PowerShell
-   Notepad++ / VS Code

**Source:** [Microsoft Sysinternals WhoIs Download](https://learn.microsoft.com/en-us/sysinternals/downloads/whois)
