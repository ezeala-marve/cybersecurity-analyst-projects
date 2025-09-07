# **Building a Splunk SOC from Scratch: A Log-by-Log Journey**

This project documents my hands-on journey from installing Splunk to building a functional security operations center (SOC) with dashboards and automated alerting. Every challenge and solution is detailed below.

---

## **Phase 1: Installation & Data Ingestion**

### **Downloading and Installing Splunk**
- **Process:** Downloaded Splunk Enterprise from the official website using the free license, which allows up to 500MB of data per day—ideal for learning and small-scale monitoring.
- **Challenge:** The download required account approval, which took 24 hours due to Splunk’s verification process.
- **Installation:** Ran the installer and completed the setup by accepting the license, setting admin credentials, and switching to the free license to avoid time-based limitations.

### **Ingesting Log Files**
Splunk does not use a traditional database. Instead, it:
- **Indexes data:** Breaks logs into individual events and builds a search-optimized index.
- **Uses source types:** Automatically detects log format (e.g., `log`, `csv`).

**Steps to Ingest Logs:**
1. Prepared a log file named `security_logs.log` with sample security event data.
2. Navigated to **Settings > Add Data > Upload**.
3. Selected the file and used default settings (Source Type: `log`, Index: `main`).
4. Repeated the process for a second, more complex file: `web_server_logs.log`.

**Sample log content:**
```
2024-05-21T10:17:45.678Z ERROR Login failed for user: bob from IP: 192.168.1.202. Reason: Invalid password.
2024-05-21T00:01:15.123Z INFO 192.168.1.105 GET /home.html 200 4321 User:mary
```

**⚠️ Critical Lesson: Time Range**
- Many searches returned **“No results found”** because the time picker was set to recent data.
- **Solution:** Adjusted the time range to **“All time”** to include all ingested historical logs.

---

## **Phase 2: The Analysis – Mastering SPL (Search Processing Language)**

### **Understanding the Pipe (`|`)**
The pipe is the core of SPL. It forwards results from one command to the next, enabling step-wise refinement of data.

**Example Search Flow:**
```spl
source="web_server_logs.log"    // Get all events from this source
| rex "\\s(?<status>\\d{3})\\s"  // Extract HTTP status codes using regex
| stats count by status         // Count events by status code
```

### **Key Commands Used:**
- `rex`: Extracts fields using regular expressions.
- `stats`: Per statistical operations (count, sum, avg) and groups results by a field.
- `top`: Displays the most frequent values in a field.
- `table`: Formats results into a clear, column-based view.

**Sample Use Case: Find Failed Logins**
```spl
source="security_logs.log" 
| search "failed"
| rex "user: (?<user>\\w+)"
| rex "IP: (?<ip>[\\d.]+)"
| table _time, user, ip
```

---

## **Phase 3: Building the Dashboard – “Web Traffic Monitor”**

### **Why Dashboards?**
Dashboards provide real-time, visual insights into system health and security events. They are built from multiple panels, each representing a saved search or visualization.

### **Creating the Dashboard**
1. Ran a search to analyze web traffic, e.g.:
   ```spl
   source="web_server_logs.log"
   | rex "\\s(?<status>\\d{3})\\s"
   | stats count by status
   ```
2. Clicked **Save As > Dashboard Panel**.
3. Created a new dashboard named **“Web Traffic Monitor”**.
4. Added multiple panels including:
   - A **pie chart** of HTTP status codes.
   - A **column chart** of the top 5 most active users.

### **Challenges & Solutions:**
- The dashboard dropdown sometimes failed to show existing dashboards.
- **Fix:** Typed the dashboard name manually instead of selecting from the list.

---

## **Phase 4: Automation – Proactive Alerting**

### **Creating an Alert**
**Goal:** Get notified of a rise in server errors (HTTP 500).

**Steps:**
1. Built and tested the search:
   ```spl
   source="web_server_logs.log" status=500
   | stats count
   ```
2. Clicked **Save As > Alert**.
3. Configured the alert:
   - **Title:** “High Server Errors”
   - **Condition:** Trigger if number of results is greater than 1.
   - **Schedule:** Run every hour.
   - **Action:** Send email to the analyst.

### **Why Scheduled and Not Real-Time?**
- Real-time alerts are resource-intensive.
- For most operational purposes (e.g., error rates, login attempts), checking every hour or day is sufficient.

---

## **The SOC Analyst Workflow**

A typical day using this setup involves:

1. **Dashboard Review:**  
   - Open the **Web Traffic Monitor** dashboard to check application health and user activity.

2. **Alert Triage:**  
   - Investigate any alerts received via email.
   - Drill into details using SPL queries.

3. **Proactive Hunting:**  
   - Run custom searches to look for anomalies, e.g.:
     ```spl
     source="security_logs.log" failed 
     | stats count by ip 
     | where count > 3
     ```

4. **Reporting and Maintenance:**  
   - Fine-tune alerts and dashboards based on findings.
   - Document incidents and responses.

---

## **Screenshots**

### 📊 Dashboard View
![Dashboard](https://your-link-here/dashboard.png)

### ⚠️ Alert Configuration
![Alert](https://your-link-here/alert.png)

---

## **Tools Used**
- Splunk Enterprise (Free License)
- Windows for hosting
- Notepad++ for log creation

---

## **Conclusion**
This project took me from zero to capable of:
- Ingestion and parsing of log data
- Writing effective SPL queries
- Building insightful dashboards
- Configuring actionable alerts

These skills form the foundation of security monitoring and are directly transferable to real-world SOC environments. The free tier of Splunk is more than enough to continue practicing and refining these abilities.
