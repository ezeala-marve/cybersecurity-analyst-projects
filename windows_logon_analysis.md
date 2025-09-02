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

![Screenshot of Logon Type 5 with no Source IP](https://github.com/Major241/cyber-portfolio/blob/main/images/logon_type_5_no_ip.png.png?raw=true)
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

#### 1. Task Execution (Event ID 200)
- **Why it matters:** This event confirms the task ran. This is a critical first step in the investigation.
- **The Finding:** The log confirmed that the task `MicrosoftsecurityHealth` was executed by the SYSTEM account.
- **The Limitation:** On this system, the log event did not contain the detailed `Actions` field showing the command-line arguments. This is a common real-world logging issue.

![Screenshot of Event ID 200](https://github.com/Major241/cyber-portfolio/blob/main/images/event_id_200_execution.png.png?raw=true)
*Caption: A scheduled task execution (200) was logged, but the critical command arguments were not captured in this view.*

#### 2. Correlating with Task Scheduler
- **The Pivot:** Due to the missing data in the event log, the investigation pivoted to the source: the **Task Scheduler application** itself.
- **The Discovery:** By inspecting the task's properties, the full command was revealed in the **Actions** tab:
  - **Program/script:** `powershell.exe`
  - **Add arguments:** `-WindowStyle Hidden -Command "echo 'This could be malicious!' > C:\Windows\Temp\secret_log.txt"`

- **Analysis of the Command:**
  - **`-WindowStyle Hidden`**: This is a key red flag. It is a common adversary tactic to hide the execution of a script from the user.
  - **The Overall Purpose:** The task was configured to run a hidden PowerShell command every time a user logs on, which is a classic technique for establishing persistence.

![Screenshot of the Task Scheduler Actions Tab](https://github.com/Major241/cyber-portfolio/blob/main/images/task_scheduler_action_lab.png.png?raw=true) 
*Caption: The Task Scheduler application provides the missing evidence, showing the hidden PowerShell command.*

---

## Investigation Notes & Challenges

**The Learning Curve:**
- **The Challenge 1:** The most useful logs for task analysis (`TaskScheduler/Operational`) are not enabled by default. My initial investigation found an empty log.
- **The Solution 1:** I enabled the log manually via its Properties in Event Viewer. This highlights a critical real-world lesson: essential auditing features are often off, and proactive configuration is needed for effective monitoring.
- **The Challenge 2:** The standard guidance for logon events (4624) is to "look for the source IP." I found this field missing from Logon Type 5 events, which was initially confusing.
- **The Solution 2:** Through research, I learned that the absence of a source IP is a key data point itself, confirming the logon was local and service-related. This taught me to understand the context of events, not just blindly follow checklists.
- **The Challenge 3:** The `TaskScheduler/Operational` log did not show the command arguments for Event ID 200, which is a known logging behavior on some systems.
- **The Solution 3:** I pivoted my investigation to the Task Scheduler application itself to find the configured command. This demonstrated the importance of correlating multiple data sources and not giving up when the primary source is incomplete.

## Conclusion
This exercise provided hands-on experience with core Windows security monitoring. It highlighted that real-world analysis is often messy. Logs can be incomplete, and evidence must be pieced together from various sources. The ability to adapt, pivot investigation strategies, and understand the "why" behind the data is what separates a proficient analyst from a beginner. Correlating events from different logs and system configurations is key to building a complete story of what happened on an endpoint.
