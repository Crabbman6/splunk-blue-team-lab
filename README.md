# SOC Analyst Portfolio: Splunk SIEM Home Lab

![Splunk](https://img.shields.io/badge/Splunk-000000?style=for-the-badge&logo=splunk)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Windows](https://img.shields.io/badge/Windows_11-0078D4?style=for-the-badge&logo=windows-11&logoColor=white)
![Sysmon](https://img.shields.io/badge/Sysmon-0078D4?style=for-the-badge&logo=windows-terminal&logoColor=white)

---

## 🎯 Project Overview

This repository serves as a portfolio of my hands-on experience in building a fully functional Security Operations Center (SOC) home lab. The primary goal is to simulate a real-world security environment using **Splunk Enterprise** to ingest, parse, and analyze logs from both Linux and Windows endpoints. This project demonstrates my practical skills in SIEM administration, threat detection engineering, and incident analysis, all of which are crucial for an entry-level SOC Analyst role.

---

## 🛠️ Lab Architecture

The lab is designed to be a lightweight yet effective environment for security monitoring. All components are virtualized or run locally, simulating a typical small business network.

| Component         | Technology / OS          | Purpose                                        |
| ----------------- | ------------------------ | ---------------------------------------------- |
| **SIEM Host** | Linux Mint 21            | Hosts the Splunk Enterprise instance.          |
| **Linux Endpoint**| Ubuntu Server (VM)       | Generates system and authentication logs.      |
| **Windows Endpoint**| Windows 11 (VM)        | Generates Windows Event Logs & Sysmon data.    |
| **Log Forwarders**| Filebeat / Splunk UF     | Collects and forwards logs to Splunk.          |

![Lab Diagram](screenshots/lab-diagram.png)

---

## 🚀 Core Projects & Detections

This section details the key security use cases implemented in the lab. Each project includes log source configuration, Splunk parsing, and the development of detection rules using **Splunk Processing Language (SPL)**.

### 1. [Linux Endpoint Monitoring & Threat Detection](./01-Linux-Log-Ingestion.md)
* **Objective:** Ingest critical Linux logs to monitor for common attack vectors.
* **Log Source:** `/var/log/auth.log` and `/var/log/syslog` forwarded via Filebeat.
* **Detections Implemented:**
    * Brute-Force SSH attempt detection.
    * Alert for successful SSH login from a new IP address.
    * Monitoring of `sudo` command usage for privilege escalation.

### 2. [Windows Endpoint Security with Sysmon](./02-Windows-Sysmon-Ingestion.md)
* **Objective:** Gain deep visibility into Windows host activity using Sysmon.
* **Log Source:** Windows Event Logs (Security, System, Application) and Sysmon logs forwarded via the Splunk Universal Forwarder.
* **Detections Implemented:**
    * Detection of suspicious PowerShell commands (e.g., encoded commands, download cradles).
    * Identification of Mimikatz-like process access (`lsass.exe`).
    * Monitoring for persistence mechanisms (e.g., new scheduled tasks, registry run keys).

### 3. [Dashboards & Alerting](./03-Dashboards-and-Alerting.md)
* **Objective:** Visualize security events and create automated alerts for high-fidelity threats.
* **Technologies:** Splunk Dashboards, SPL, and Splunk Alerting framework.
* **Features:**
    * **SOC Overview Dashboard:** A single pane of glass showing failed logins, notable Sysmon events, and network traffic highlights.
    * **Real-time Alerts:** Configured email alerts for critical detections, such as multiple failed logins followed by a success from the same IP.

---

## 💡 Skills Demonstrated

This project showcases a range of technical competencies essential for a Blue Team or SOC role:

* **SIEM Administration:** Deployed and configured Splunk Enterprise, including index management, data inputs (HEC), and forwarder management.
* **Log Analysis:** Parsed and normalized varied log formats from both Linux and Windows operating systems.
* **Threat Detection Engineering:** Wrote custom detection rules in SPL to identify indicators of compromise (IOCs) and tactics, techniques, and procedures (TTPs).
* **Endpoint Security:** Configured and monitored Windows endpoints with Sysmon for advanced threat hunting capabilities.
* **Data Visualization:** Built intuitive dashboards in Splunk to provide actionable security insights for analysts.
* **Incident Response:** Developed alerting logic to flag high-priority security events for immediate investigation.
