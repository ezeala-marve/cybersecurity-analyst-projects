# Windows Logon and Task Audit Analysis

## Project Overview
This project demonstrates practical Windows Event Log analysis by auditing both authentication events (logons) and persistence mechanisms (scheduled tasks). The goal was to identify successful and failed access attempts, privilege use, and evidence of potential attacker persistence on a system. This analysis was conducted on a live Windows environment, highlighting real-world log nuances.

---

## Part 1: Logon and Account Management Analysis

### Objectives:
- Identify successful and failed user logons.
- Audit changes to user accounts.
- Detect highly privileged "Special Logons".
- Understand the context behind different logon types.

### Key Event IDs & Findings:

#### 1. Successful Logon (Event ID 4624)
This event is recorded when a user or process successfully logs on to a system.
- **Why it matters:** Establishes a baseline of normal activity. A successful logon from an unusual account or at an unusual time can indicate compromise.
- **Key Finding - Logon Type 5 (Service):** I discovered a **Logon Type 5** event.
  - **What it means:** This logon was performed by a **service** starting up. The "Subject" section shows which account was used to launch the service (e.g., `SYSTEM`, `LOCAL SERVICE`).
  - **Investigation Insight:** A crucial discovery was that **Logon Type 5 events often have no "Source Network Address"**. This is because the logon originates from the Windows Service Control Manager *within* the machine itself, not from the network. This absence of a source IP is normal and expected for this logon type.
  - **Attacker Abuse:** Attackers can install malicious software as a service to maintain persistence. Analysts should investigate service logons from unfamiliar user accounts.

![Screenshot of Logon Type 5 with no Source IP](
https://github.com/Major241/cyber-portfolio/blob/main/logon_type_5_no_ip.png.png?raw=true)
*Caption: A Logon Type 5 event. Note the absence of a "Source Network Address," which is expected behavior for a service starting locally.*

#### 2. Special Logon (Event ID 4672)
This event is recorded when a user with highly privileged rights (Administrator) logs on.
- **Why it matters:** Privileged account logons require extra scrutiny. Any unauthorized use of an admin account is a critical security incident.
- **What to look for:**
  - `Account Name`: Which privileged account was used?
  - `Account Domain`: The domain of the account.

#### 3. User Account Management (Event ID 5379)
This event is related to credential management, like a password being changed or reset.
- **Why it matters:** Attackers may change passwords to maintain access ("backdoor") or lock out legitimate users.
- **What to look for:**
  - `Subject Account Name`: Who performed the action?
  - `Target Account Name`: Whose credentials were changed?

---

## Part 2: Hunting for Persistence (Scheduled Tasks)

### Objectives:
- Identify the creation and execution of scheduled tasks.
- Analyze tasks for malicious characteristics like hidden execution.

### Key Event IDs & Findings:
*All events found in `Microsoft-Windows-TaskScheduler/Operational`.*

#### 1. Task Registered/Created (Event ID 106)
- **Why it matters:** This log shows the moment a task was created. A task created by a user (not system) needs investigation.
- **What to look for:**
  - `TaskName`: Is it disguised? (e.g., `GoogleUpdateTaskMachineUA` vs. `my-malicious-task`)
  - `UserId`: Who created it?

#### 2. Task Execution (Event ID 200)
- **Why it matters:** This confirms the task ran and shows the exact command executed. This is the most important event for finding malware.
- **Analysis of My Event:** The task executed PowerShell with the argument `-WindowStyle Hidden`.
  - **Why this is a key finding:** The `-WindowStyle Hidden` parameter is a common adversary tactic to hide the execution of a script from the user. While it can have legitimate uses, its presence in a newly created or unfamiliar scheduled task is a high-severity indicator of compromise that demands immediate investigation.
- **Context is Critical:** In this simulated example, the command was benign (writing a text file). However, in a real incident, this same command structure could easily be followed by a malicious script or an `-EncodedCommand` containing malware.
- **Other Red Flags to Hunt For:**
  - `-EncodedCommand` / `-enc`: Used to obfuscate PowerShell commands with Base64 encoding.
  - Execution from user directories: Paths containing `AppData`, `Temp`, or other non-standard locations for system utilities.

![Screenshot of Event ID 200](https://github.com/Major241/cyber-portfolio/blob/main/event_id_200_execution.png.png?raw=true)
*Caption: A scheduled task execution (200) showing a hidden PowerShell command, a key indicator of compromise.*

---

## Investigation Notes & Challenges

**The Learning Curve:**
- **The Challenge 1:** The most useful logs for task analysis (`TaskScheduler/Operational`) are not enabled by default. My initial investigation found an empty log.
- **The Solution 1:** I enabled the log manually via its Properties in Event Viewer. This highlights a critical real-world lesson: essential auditing features are often off, and proactive configuration is needed for effective monitoring.
- **The Challenge 2:** The standard guidance for logon events (4624) is to "look for the source IP." I found this field missing from Logon Type 5 events, which was initially confusing.
- **The Solution 2:** Through research, I learned that the absence of a source IP is a key data point itself, confirming the logon was local and service-related. This taught me to understand the context of events, not just blindly follow checklists.
- **The Win:** Discovering the less common Event ID 5379 for credential management within the Security log.
- **The Challenge 3:** The simulated task did not exhibit the classic "textbook" red flags like `-EncodedCommand`, providing an incomplete picture.
- **The Solution 3:** This reinforced a critical lesson: malware detection is often about piecing together **partial indicators**. While the full suite of red flags wasn't present, the single indicator of `-WindowStyle Hidden` was still a high-value finding. This mimics real-world investigations where evidence is rarely perfect.

## Conclusion
This exercise provided hands-on experience with core Windows security monitoring. Correlating events from different logs (Security vs. Application/Services logs) is key to building a complete story of what happened on an endpoint. Understanding the context of these events—why they are important, what to look for, and what their absence means—is the foundation of effective threat hunting and incident response.
