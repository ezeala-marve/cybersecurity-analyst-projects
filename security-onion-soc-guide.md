# Security Onion Complete Guide

**TL;DR**

Security Onion isn't a single tool; it's a pre-built security workflow. You use four main tools in sequence:

1.  **Sguil:** Your real-time alarm system. It flashes red for critical alerts.
2.  **Squert:** Your correlation engine. It connects the dots between alerts.
3.  **Kibana:** Your time machine. It lets you investigate what happened weeks ago.
4.  **CyberChef:** Your decoder ring. It translates hidden or encoded data.

**The workflow is simple:** See an alert in **Sguil**, investigate its context in **Squert**, research its history in **Kibana**, and decode any hidden payloads with **CyberChef**.

## The Core Concept: A security Operations Workflow

Let me break down how Security Onion really works. It's not just a bunch of tools thrown together it's a complete security operations workflow that makes you feel like you're in a real SOC.

### The Four Tools That Work Together

First up: Sguil - Your 911 Dispatch Center
This is where the action happens first. Alerts pop up in real-time just like emergency calls. The colors on the left tell you the priority at a glance: red means critical, yellow is suspicious, green is just informational. You use this when you need to know what's happening right this second.

Then there's Squert - Your Shift Supervisor
This gives you a web-based view of everything Sguil is showing you, but with more context. It connects the dots between different alerts and shows you attack patterns over time. It even shows you where attacks are coming from on a map.

Next: Kibana - Your Detective
When you need to dig deeper, Kibana is your go-to. It lets you search through months of logs in seconds and spot trends you'd never see in real time. This is where you answer questions like "Has this happened before?" or "What's the full story behind this attack?"

And finally: CyberChef - Your Forensics Lab
This is where you decode the tricky stuff. That base64 encoded command? Paste it here. Weird hex dump? CyberChef makes it readable. It's like having a Swiss Army knife for data analysis.

## Real World Scenarios You'll Actually Face

### Catching Malware Beaconing

Here's what typically happens:
You're watching Sguil when you notice suspicious outbound connections happening like clockwork every five minutes. You click on the alert and see it's calling home to some random IP address. You jump over to Squert and confirm this isn't a one time thing it's a pattern. Then you check Kibana and discover this machine has been doing this for two weeks! Finally, you use CyberChef to decode the communication and understand what the malware is saying.

What you do: Isolate that infected computer and block the malicious IP.

### Stopping Brute Force Attacks

How this plays out:
Sguil starts lighting up with failed SSH login attempts. Squert shows you the attack is coming from multiple IP addresses working together. Kibana reveals this is part of a bigger campaign hitting all your servers. CyberChef helps you decode any sneaky payloads they're trying to send.

What you do: Block the entire attacker IP range and tighten up your SSH security.

### Spotting Data Theft

The sequence:
Sguil alerts you about huge data transfers happening at 3 AM. Squert connects this to database access logs. Kibana shows you exactly what data got accessed and when. CyberChef helps you understand what format the stolen data is in.

What you do: Contain the breach immediately and change all compromised passwords.

## Getting Started Fast

Here are the commands you'll use daily:

Check if everything is running properly
sudo so-status

Access the web tools from your main computer
https://localhost:5601    for Kibana
https://localhost/squert/ for Squert

On the Security Onion machine itself, just double-click the Sguil and CyberChef icons on the desktop

To generate some test traffic and see alerts in action
ping -c 5 8.8.8.8

## Pro Tips From Experience

Pay attention to those colors in Sguil they're your triage system. Handle the red alerts first, yellow ones next, green can wait.

Remember to adjust Kibana's time range if you're investigating something that happened last week, don't just look at the last 15 minutes.

CyberChef lets you chain operations together decode base64 first, then parse the JSON that appears.

Squert's timeline view is golden for understanding how an attack unfolded step by step.

## The Big Picture

The key to mastering Security Onion is using all the tools together. Don't get stuck just watching Sguil all day. When something interesting happens, follow this natural flow:

See an alert then Investigate it then Connect it to other events then Decode any hidden data then Take action

This guide comes straight from hands-on experience setting up and using Security Onion in real scenarios. No theory just what actually works when you're in the trenches.

If this helped you understand how Security Onion really works, please star this repo!
