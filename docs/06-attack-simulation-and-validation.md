# Attack Simulation & Detection Validation

This document details the final phase of the SOC lab project: validating the previously built detections using simulated attacks from a Kali Linux virtual machine. A detection is only useful if it is proven to work against a realistic attack. This process demonstrates the full lifecycle of security monitoring, from data collection to proactive defense validation.

The lab is configured with a Kali Linux VM acting as the attacker, and the Linux host and Windows 11 VM as the targets.

---
## 1. Use Case: Validating the SSH Brute-Force Alert

The first simulation will test the "Failed SSH Logins" detection created in the previous phase.

* **The Attacker:** Kali Linux VM
* **The Target:** Linux Mint Host 
* **The Tool:** Hydra (a popular network logon cracker)
* **The Goal:** To see if a high-volume SSH brute-force attack from Kali triggers the corresponding panel and alert in Splunk.

### 1.1 - The Attack


### 1.2 - The Detection


### 1.3 - The Alert


### 1.4 - Conclusion
