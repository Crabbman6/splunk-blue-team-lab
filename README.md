# SOC Analyst Portfolio: Splunk SIEM Home Lab

![Splunk](https://img.shields.io/badge/Splunk-9.2-000000?style=for-the-badge&logo=splunk)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-5B7180?style=for-the-badge&logo=kali-linux&logoColor=white)
![Windows 11](https://img.shields.io/badge/Windows_11-0078D4?style=for-the-badge&logo=windows-11&logoColor=white)
![Linux Mint](https://img.shields.io/badge/Linux_Mint-87C375?style=for-the-badge&logo=linux-mint&logoColor=white)
![Sysmon](https://img.shields.io/badge/Sysmon-0078D4?style=for-the-badge&logo=windows-terminal&logoColor=white)

---

## 🎯 Project Overview

This repository documents a comprehensive Security Operations Center (SOC) home lab built from the ground up. The project simulates a real-world security monitoring environment using **Splunk Enterprise** to ingest, analyze, and detect threats across both Linux and Windows systems.

The project is broken down into distinct phases, from initial lab setup and data ingestion to detection engineering and the validation of those detections through simulated attacks. It serves as a practical demonstration of the core competencies required for a SOC Analyst role.

---

## 🛠️ Lab Architecture

This lab operates on a single host machine, which serves as both the SIEM server and a monitored Linux endpoint. Virtualization is used to create additional endpoints for monitoring and attack simulation.

| Component | Technology / OS | Purpose |
| :--- | :--- | :--- |
| **Host / SIEM / Linux Endpoint** | Linux Mint 21 | Runs Splunk, hosts VMs, and is monitored as the primary Linux endpoint. |
| **Windows Endpoint**| Windows 11 (VM) | A virtualized endpoint used to generate and forward Windows-specific logs. |
| **Attacker Machine**| Kali Linux (VM) | A virtualized endpoint used to simulate attacks and validate detections. |

For a detailed breakdown of the virtualization and network configuration, please see the full setup documentation:
* **[01-Lab-Setup-and-Architecture.md](./docs/01-lab-setup-and-architecture.md)**

---

## 🚀 Project Phases & Documentation

This project was completed in several phases, each with its own detailed write-up documenting the process, challenges, and outcomes.

| Phase | Summary |
| :--- | :--- |
| **1. SIEM Initialization** | Deployed Splunk Enterprise on the Linux host and performed initial health checks to ensure the platform was operational before ingesting data. **([Read the full write-up](./docs/02-starting-splunk.md))** |
| **2. Linux Log Ingestion** | Established a logging pipeline from the Linux host to Splunk using the Splunk Universal Forwarder. This section includes a detailed case study of troubleshooting a third-party log shipper (Filebeat). **([Read the full write-up](./docs/03-ingesting-linux-logs.md))** |
| **3. Windows Log Ingestion** | Integrated a Windows 11 endpoint, deploying Sysmon for deep visibility and the Splunk Universal Forwarder. This document details an advanced, multi-stage troubleshooting narrative to resolve complex permissions and configuration issues. **([Read the full write-up](./docs/04-ingesting-windows-logs.md))** |
| **4. Detection Engineering** | Moved from data collection to analysis. Developed custom SPL queries to detect specific threats (SSH Brute-Force on Linux, Suspicious PowerShell on Windows), visualized them on a SOC dashboard, and configured automated alerts. **([Read the full write-up](./docs/05-detection-engineering-and-dashboards.md))** |
| **5. Attack Simulation** | Validated the Linux SSH brute-force detection by launching a simulated attack from a Kali Linux VM using Hydra, proving the end-to-end effectiveness of the detection and alerting pipeline. **([Read the full write-up](./docs/06-attack-simulation-and-validation.md))** |

---

## 💡 Skills Demonstrated

This project showcases a range of technical competencies essential for a modern SOC role:

* **SIEM Administration:** Deployment, configuration, and health monitoring of Splunk Enterprise.
* **Log Management:** Configuration of log forwarders (Splunk UF, Filebeat) for both Linux (`syslog`, `auth.log`) and Windows (Event Logs, Sysmon) data sources.
* **Systematic Troubleshooting:** A core focus of this project, demonstrating a methodical approach to diagnosing and resolving complex issues related to software, networking, and permissions.
* **Detection Engineering:** Development of custom detection rules using Splunk Processing Language (SPL) to identify specific attacker techniques.
* **Security Monitoring:** Creation of dashboards for at-a-glance visualization and automated alerts for real-time threat notification.
* **Attack Simulation:** Validation of defensive measures by simulating a common attack vector (SSH Brute-Force) with an offensive security tool (Hydra).
* **Virtualization & Networking:** Setup and configuration of a multi-machine virtual lab environment using VirtualBox and bridged networking.
