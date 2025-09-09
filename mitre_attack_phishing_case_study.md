🎯 **Case Study: Deconstructing a Phishing Attack with MITRE ATT&CK**

🔹 **Objective**

Simulate a phishing email attack, collect forensic evidence, and map the adversary behaviors to the MITRE ATT&CK framework using real-world techniques.

---

🔹 **Investigation Steps**

	1.	Setup & Simulation

	•	Generated a trackable phishing URL with Canarytokens.

	•	Crafted a phishing email and executed a controlled click to trigger an alert.

	•	![canarytokens](Canarytokens interface showing the generated URL)

	2.	Evidence Collection

	•	Captured timestamp, IP address, and browser details from the triggered beacon.

	•	Validated adversary behavior using forensic logs.

	•	📸 Screenshot: Alert output (IP, timestamp, browser).

	3.	ATT&CK Mapping

	•	Resource Development → T1583 (Acquire Infrastructure)

	•	Initial Access → T1566.002 (Phishing: Spearphishing Link)

	•	Execution → T1204.001 (User Execution: Malicious Link)

	•	Command & Control → T1102 (Web Service)

	•	Collection → T1114 (Email Collection)

	•	Impact → T1498 (Network Denial of Service)

	•	📸 Screenshot: ATT&CK Navigator showing highlighted techniques.

	4.	Visualization

	•	Created a flow diagram of the phishing lifecycle.

	•	Email → Malicious Link → User Click → C2 Beacon → Data Collection

🔹 **Outcome**

This project transformed a theoretical framework into a hands-on investigative workflow. By simulating a phishing lifecycle and mapping each step to MITRE ATT&CK, I demonstrated how analysts can:

	•	Translate raw evidence into standardized threat language.

	•	Visualize adversary behavior across the kill chain.

	•	Use ATT&CK to guide hunting and strengthen SOC defenses.
