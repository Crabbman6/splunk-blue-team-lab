# Attack Simulation & Detection Validation

This document details the final phase of the SOC lab project: validating the previously built detections using simulated attacks from a Kali Linux virtual machine. A detection is only useful if it is proven to work against a realistic attack. This process demonstrates the full lifecycle of security monitoring, from data collection to proactive defense validation.

The lab is configured with a Kali Linux VM acting as the attacker, and the Linux host and Windows 11 VM as the targets.

---
## 1. Use Case: Validating the SSH Brute-Force Alert

The first simulation tests the "Failed SSH Logins" detection and alert created in the previous phase.

* **The Attacker:** Kali Linux VM
* **The Target:** Linux Mint Host (`192.168.0.194`)
* **The Tool:** Hydra (a popular network logon cracker)
* **The Goal:** To see if a high-volume SSH brute-force attack from Kali triggers the corresponding panel and alert in Splunk.

### 1.1 - The Attack
To perform the attack, a small custom wordlist (`passwords.txt`) was created on the Kali VM. This list contained several common passwords as well as the one correct password for the target user, ensuring the attack could test for both failed and successful attempts.

<img width="1392" height="854" alt="image" src="https://github.com/user-attachments/assets/c69b49f5-ed33-4c5b-ad49-3a956cc29e04" />

The Hydra tool was then launched from the Kali terminal with the following command, targeting the Linux host's SSH service.

```bash
hydra -l jack -P passwords.txt 192.168.0.194 ssh
```
The attack ran successfully, rapidly trying each password and eventually finding the correct one.

<img width="1395" height="855" alt="image" src="https://github.com/user-attachments/assets/9082469f-1f7a-4879-a0d8-372efc884e6a" />

### 1.2 - The Detection
Immediately after the attack began, the **"SOC Overview" dashboard** in Splunk lit up with activity. The "Failed SSH Logins" panel populated with thousands of events originating from the Kali VM's IP address, confirming the SPL query was identifying the brute-force attack in near real-time.

[Add a screenshot of your Splunk dashboard panel showing the attack results here.]

### 1.3 - The Alert
Within 5 minutes of the attack starting, the scheduled alert was triggered. A search of Splunk's internal logs confirmed that the alert fired correctly, logging the custom event text containing the attacker's details. This proves the automated notification system is fully operational.

[Add a screenshot of the triggered alert found with the search `index=_internal sourcetype=splunkd_alert_actions` here.]

### 1.4 - Conclusion
The simulation was a success. The Hydra attack was correctly identified by the SPL query, visualized on the dashboard, and triggered a timely alert. This validates the end-to-end effectiveness of this detection rule.
