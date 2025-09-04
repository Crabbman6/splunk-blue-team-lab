-- template built with AI

# SOC Analyst Home Lab – Splunk

This repository documents my personal SOC analyst home lab, where I built and configured a Splunk SIEM environment to ingest, analyze, and detect suspicious activity across Linux and Windows systems.

## Goals
- Develop practical SOC analyst skills with industry tools
- Demonstrate hands-on SIEM, log analysis, and threat detection
- Showcase portfolio work for future employers

## Lab Environment
- Host: Linux Mint (Splunk Enterprise Free Edition – 500MB/day)
- Windows 11: Sysmon + Splunk Universal Forwarder over LAN
- Logs: Linux system logs (/var/log), Windows Event Logs, Sysmon
- Tools: Splunk, Sysmon, SPL queries, dashboards

![Lab Diagram](screenshots/lab-diagram.png)

## Key Projects
1. **Linux Log Ingestion** – monitoring SSH login attempts
2. **Windows Sysmon Integration** – collecting process, network, and registry events
3. **Threat Detection Queries** – brute-force login detection, suspicious PowerShell executions
4. **Dashboards** – visualizing failed logins, top processes, and Sysmon network connections
5. **Alerts** – configured Splunk to trigger alerts on brute-force attempts

## Portfolio Value
This project demonstrates:
- SIEM configuration & log ingestion
- Threat detection engineering
- Incident monitoring & reporting
- Hands-on blue team skills
