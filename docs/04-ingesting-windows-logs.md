# SIEM Log Ingestion: Windows Endpoint with Sysmon

This project documents the process of forwarding logs from a Windows 11 endpoint to a Splunk SIEM. The key focus is on integrating **Sysmon (System Monitor)**, a powerful tool that provides deep visibility into process creation, network connections, and other granular system activities far beyond standard Windows Event Logs. This write-up details the full process from setup and configuration to a multi-stage troubleshooting investigation and final success.

---
## 1. Endpoint Preparation & Sysmon Deployment

The first phase was to prepare the Windows 11 VM and deploy Sysmon. A prerequisite was identifying the IP address of the Linux host running Splunk, which would serve as the destination for all logs.

<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/11fe783f-618f-4d85-9fad-dc344c4aa29c" />

Sysmon and a standard industry configuration file from SwiftOnSecurity were downloaded to a `C:\Tools` directory. Using a configuration file is a critical best practice to filter out noise and focus on high-fidelity security events.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/78c033f0-03f6-4890-917c-c4ce6e4d652b" />

Sysmon was then installed via an administrative PowerShell, applying the rules from the configuration file.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/514a4bf6-9f3e-4843-9c4e-6184c8974f2b" />

---
## 2. Splunk Universal Forwarder Installation

With the log source prepared, the Splunk Universal Forwarder was installed on the Windows VM. The installer was configured with the IP of the Splunk server for both the **Deployment Server** (port 8089) and the **Receiving Indexer** (port 9997).

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/c098e73f-a37f-401f-a336-fbfab6628ff9" />
<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/5e9887a4-8606-4c51-bcb2-9f1ddd0a5f50" />
<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/ff269af7-14b2-4d47-ab29-a194aa24f534" />

---
## 3. Forwarder Configuration & Initial Verification

The forwarder was configured by creating an `inputs.conf` file to monitor the core Windows Event Logs (Security, Application, System) and the Sysmon operational log.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/518110d0-faf6-404a-acac-41224465c9bb" />

An outbound Windows Firewall rule was created for the `splunkd.exe` process on TCP port 9997 to ensure logs could be sent out.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/046b7ad3-f71d-43a1-bbd3-4c559763671d" />

An initial search in Splunk for the VM's hostname confirmed that the standard Windows logs were being successfully ingested.

<img width="1919" height="894" alt="image" src="https://github.com/user-attachments/assets/3c092cbf-cbaa-4b09-ae02-62ae8aa2a39b" />

---
## 4. Advanced Troubleshooting: Isolating the Sysmon Issue

Despite the successful ingestion of standard logs, a search for `sourcetype="Sysmon"` returned no results. This triggered a systematic investigation to find the root cause.

### 4.1 - Initial Diagnosis
A check of the forwarder's internal logs (`splunkd.log`) revealed a critical error: **`errorCode=5 (Access is denied)`** when trying to subscribe to the Sysmon event channel. This immediately pointed to a permissions issue on the Windows VM.

<img width="969" height="649" alt="image" src="https://github.com/user-attachments/assets/278a71fa-df88-465f-80f2-803c7017d119" />

### 4.2 - Investigating the Root Cause
The investigation revealed that even the administrator account was getting an "Access is denied" error when trying to view the Sysmon logs in Event Viewer. The underlying cause was an incorrect installation path for Sysmon.

<img width="976" height="687" alt="image" src="https://github.com/user-attachments/assets/aeded886-e0e5-4108-8514-2ed7901c01b2" />

The issue was resolved by **reinstalling Sysmon from a system-wide directory (`C:\Tools`)** instead of a user-profile folder, which corrected the local permissions. After the reinstallation, the logs were visible in Event Viewer.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/79e1b2c8-05ff-4725-810b-f6662e02ef2c" />

### 4.3 - The Final Fix
Even with local access restored, the forwarder service still failed to send the logs. The final fix was to reset the service's security context. This was done by going into the `SplunkForwarder` service properties, toggling the "Log on as" account, and ultimately setting it back to the correct default: the **`Local System account`**. Restarting the service after this change forced a full refresh of the now-correct file and channel permissions.

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/5d0db3a0-1680-4689-8e39-417f249cf1db" />

---
## 5. Verification and Success

Following the extensive troubleshooting, a search for `sourcetype="Sysmon"` in Splunk was finally successful. The events began streaming in as expected, confirming the pipeline was fully functional. This project highlights a realistic, multi-step diagnostic process required to solve complex integration issues.

<img width="1919" height="896" alt="image" src="https://github.com/user-attachments/assets/3e20c185-e390-4601-b6a6-29766d380f8a" />
