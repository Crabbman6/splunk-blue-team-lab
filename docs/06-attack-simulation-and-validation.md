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
Immediately after the attack began, the **"SOC Overview" dashboard** in Splunk provided immediate visibility into the attack. The "Failed SSH Logins" panel populated with thousands of events originating from the Kali VM's IP address, confirming the SPL query was correctly identifying the brute-force activity in near real-time.

<img width="1890" height="421" alt="image" src="https://github.com/user-attachments/assets/72257e5a-11af-41d8-8092-90d05b00df79" />

### 1.3 - The Alert
To verify that the automated alert was triggered, the alert's **Job History** was inspected in Splunk.

The Job History shows that the search ran on its 5-minute schedule. Crucially, the "Events" column confirms that the search found results multiple times after the attack began, which proves that the alert's trigger condition (`Number of results > 0`) was successfully met.

<img width="1895" height="733" alt="image" src="https://github.com/user-attachments/assets/bae43352-97a3-41a5-a2a9-55e510c51f86" />

### 1.4 - Conclusion
The simulation was a success. The Hydra attack was correctly identified by the SPL query, visualized on the dashboard, and the scheduled alert's trigger conditions were met. This validates the end-to-end effectiveness of this detection rule.
