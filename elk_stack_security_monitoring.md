# ELK Stack Homelab: Building a Security Analyst's Playground

## TL;DR
I built a fully functional ELK Stack on Ubuntu to practice real SOC analyst workflows. From battling SSL errors to finally seeing security logs appear in Kibana, this project transformed theoretical knowledge into practical investigation skills. The result: a homelab where I can practice threat hunting, log analysis, and incident response using the same tools deployed in enterprise security operations.

---

## Introduction

### Purpose of the Project
This project documents my hands-on journey of building a fully functional ELK Stack from scratch on Ubuntu, specifically tailored for security monitoring and incident response practice. The goal was to create a practical environment where I could develop real SOC analyst skills beyond theoretical knowledge.

### Why ELK for Security Monitoring
The ELK Stack provides the foundation for modern security operations: Elasticsearch for powerful log storage and retrieval, Kibana for intuitive data visualization, and the ecosystem for comprehensive security monitoring. Unlike simulated environments, this setup uses actual tools deployed in enterprise SOCs, giving me practical experience that directly translates to real-world scenarios.

## Setting up the Environment

### Installing Ubuntu
The foundation began with a clean Ubuntu Server 22.04 LTS installation, configured with minimal resources to mirror constrained enterprise environments. The choice of Ubuntu Server rather than Desktop emphasized the reality that security tools typically run on headless systems.

### Preparing the System for ELK
System preparation involved optimizing the Ubuntu instance for the resource-intensive ELK Stack, including memory allocation adjustments, storage planning, and network configuration to ensure reliable service operation.

## Tools & Technologies Used

### Elasticsearch
The cornerstone of the stack, Elasticsearch serves as the powerful search and analytics engine that enables rapid querying of security events and logs.

### Kibana
The analyst's interface—where investigations come to life through intuitive visualizations, dashboards, and the Discover interface for real-time log exploration.

## Building the ELK Stack

### Installing and Configuring Elasticsearch
The installation process involved navigating dependency challenges and configuration nuances, particularly around security settings and network binding. Through trial and error, I established a stable Elasticsearch instance ready for security data ingestion.

### Configuring Kibana
Kibana configuration required careful attention to network settings and Elasticsearch connectivity. The breakthrough came when I successfully configured Kibana to accept external connections while maintaining a secure link to Elasticsearch.

## Elastic Investigative Flow

![Kibana Discover Interface Showing Security Events](https://github.com/ezeala-marve/cybersecurity-analyst-projects/blob/main/images/elastic.png.png?raw=true)

### Ingesting Security Logs
The moment of truth arrived when I successfully ingested sample security data into Elasticsearch, transforming raw logs into searchable, actionable security intelligence.

### Searching and Filtering Events
Using Kibana's Discover interface, I practiced essential security queries:
- Identifying failed authentication attempts
- Tracking user activity across systems
- Correlating events by time and source
- Filtering noise to focus on genuine threats

### Visualizing Data in Kibana
Beyond simple searches, I explored Kibana's visualization capabilities to create dashboards that provide at-a-glance security posture awareness—exactly what SOC analysts need during active incidents.

## Workflow of a SOC Analyst

### Daily Monitoring Routines
This homelab enabled me to practice essential SOC workflows: starting each session with a review of recent events, checking for anomalies, and following up on potential security indicators.

### Detecting and Investigating Alerts
Through practical exercises, I developed the investigative mindset required for effective alert triage—distinguishing between false positives and genuine threats, and understanding the story that security logs tell.

### Why This Workflow Matters
The repetitive nature of these exercises builds the muscle memory that separates novice analysts from experienced professionals. When real incidents occur, the tools should feel like an extension of the analyst's thought process.

## Challenges and Lessons Learned

The journey included significant challenges: SSL configuration complexities, service dependency management, and network troubleshooting. Each obstacle overcome provided deeper understanding of how these systems operate in production environments.

The most valuable lesson emerged clearly: tools don't replace analytical thinking—they amplify it. An analyst who understands both the technology and the security fundamentals can wield ELK as a powerful investigative instrument.

## Conclusion

### Key Takeaways
This project demonstrated that effective security monitoring combines technical proficiency with analytical mindset. The ELK Stack provides the platform, but the analyst provides the insight.

### Next Steps in the Journey
The foundation is now set for advanced security monitoring practice, including real log source integration, automated alerting, and complex investigation scenarios that mirror actual security operations center
