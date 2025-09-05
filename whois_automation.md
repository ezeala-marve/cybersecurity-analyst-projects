# **Building a WHOIS Automation Tool on Windows: A Cybersecurity Analyst's Journey**

## **Introduction**

In digital investigations, a domain name is often the first clue. The `whois` command is a fundamental tool for interrogating this clue, providing registration details that can reveal the age, owner, and origin of a potentially malicious site. This project documents the process of overcoming Windows' limitations to install this tool and engineer a PowerShell script that automates the extraction of critical intelligence, replicating a core workflow of a Security Operations Center (SOC) analyst.

---

## **Steps: The Installation & Automation Process**

### **Step 1: Establishing Basic Functionality**

*   **Goal:** To execute the basic `whois` command in a Windows command-line environment to retrieve raw domain registration data.
*   **The Struggle:** On a fresh Windows system, the `whois` command does not exist. Executing it returns a standard error:
    ```powershell
    whois : The term 'whois' is not recognized as the name of a cmdlet, function, script file, or operable program.
    ```
*   **The Solution:** The utility is available as part of the **Microsoft Sysinternals** suite.
    1.  **Download:** Source the official `whois.exe` from the [Microsoft Sysinternals website](https://learn.microsoft.com/en-us/sysinternals/downloads/whois).
    2.  **Installation:** Place the executable in `C:\Windows\`.
    3.  **Permission Handling:** Click **"Continue"** on the **"Destination Folder Access Denied"** admin prompt.
    4.  **Final Renaming:** Rename `whois64.exe` to **`whois.exe`**.
*   **Validation:** The command `whois google.com` now executes successfully.

### **Step 2: Filtering Data for Actionable Intelligence**

*   **Goal:** To transform raw WHOIS output into a concise report of key forensic indicators.
*   **The Struggle:** The raw output is a massive wall of text, making manual analysis inefficient.
*   **The Solution:** Use the **pipe operator (`|`)** to filter output with `Select-String`.
    ```powershell
    whois google.com | Select-String "Creation Date"
    ```

### **Step 3: Engineering an Automated Investigation Script**

*   **Goal:** To package the process into a reusable, user-friendly script.
*   **The Solution - Batch Script (`investigate.bat`):**
    ```batch
    @echo off
    set /p target="Enter a domain or IP: "
    echo.
    echo [REPORT FOR %target%]
    echo ===========================
    echo.
    whois %target% | find "Creation Date"
    whois %target% | find "Registrar"
    whois %target% | find "Name Server"
    whois %target% | find "Country"
    echo.
    pause
    ```
    **Command Breakdown:**
    *   `@echo off` : Hides the commands, showing only results.
    *   `set /p target=...` : Prompts user for input and stores it in a variable.
    *   `echo.` : Prints a blank line for readability.
    *   `echo [REPORT FOR %target%]` : Prints a title with the target.
    *   `whois %target% | find "Creation Date"` : The core automation.
        *   `whois %target%` : Runs the command.
        *   `|` : The **PIPE**. Feeds output to the next command.
        *   `find "Creation Date"` : Filters lines containing this phrase.
    *   `pause` : Waits for a key press before closing the window.

*   **The Evolution - PowerShell Script (`investigate.ps1`):**
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
*   **The Execution Hurdle:** PowerShell's `Restricted` execution policy blocks scripts.
*   **The Fix:** Relax the policy for the current user safely.
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

### **Step 4: Final Output**

*   **Goal:** To execute the final script and produce a clean, structured intelligence report.
*   **The Result:** The script transforms a raw data dump into a structured report in seconds.

![Final Output of the investigate.ps1 script](https://github.com/user-attachments/assets/a55c8e16-8434-4ab7-8d78-ae5ab6359358)
*The final output of the script.*

---

## **Lessons Learned & Application in a Real Analyst Workflow**

### **Lessons Learned:**
1.  **Persistence is Primary:** Errors are diagnostic tools, not failures.
2.  **The Power of the Pipe (`|`):** The fundamental concept for data analysis in tools like Splunk and SIEMs.
3.  **Automation is Force Multiplication:** The analyst's duty is to build systems that automate tasks.
4.  **Understand Security Controls:** Knowing *why* execution policy exists is key to managing it properly.

### **Application in a Real SOC Workflow:**
An analyst receives an alert for a potential phishing campaign with a link to `fake-login.example.com`.
1.  **Triage:** The analyst runs `.\investigate.ps1`.
2.  **Investigation:** The script returns:
    *   `Creation Date: 2024-09-01` (Major red flag).
    *   `Registrar: NameSilo, LLC` (Common for abusive domains).
    *   `Name Server: ns1.dodgyhosting.ru` (Bulletproof hosting).
3.  **Decision:** Based on this automated intelligence, the analyst confirms the **true positive** and initiates blocking procedures.

---

## **Tools**
*   **Microsoft Sysinternals WhoIs**
*   **Windows PowerShell**
*   **Notepad**

---

## **Conclusion**

This project demonstrates that foundational cybersecurity is about mastering core utilities and automating them effectively. The journey from a `"command not found"` error to an automated investigation script is the essence of practical, hands-on skill development.
