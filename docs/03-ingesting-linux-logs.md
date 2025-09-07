# SIEM Log Ingestion: Linux Endpoint

This project documents the end-to-end process of ingesting logs from a Linux endpoint into a Splunk SIEM. Centralizing logs is a core function of a **Security Operations Center (SOC)**, enabling analysts to monitor for threats, investigate incidents, and maintain security posture across an environment. This write-up details the initial setup, a multi-stage troubleshooting process, and the final successful data ingestion.

---

## Step 1: Log Source Analysis

The first step is to identify the most valuable log sources on the Linux machine. For this project, the focus was on authentication and general system logs.

**Key Logs for Security Monitoring:**
* `/var/log/auth.log`: A **high-value security log** that records all authentication events, including SSH logins, `sudo` usage, and user management. It is critical for detecting brute-force attacks and unauthorized access.
* `/var/log/syslog`: Provides broad **operational context** by capturing messages from the kernel and various system services, which is useful for correlating system errors with potential malicious activity.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/af1cded7-8da3-4267-9f8c-9aa82dff000e" />

---

## Step 2: Forwarder Deployment

A **log forwarder** is a lightweight agent installed on an endpoint to collect and send logs to a central location. **Filebeat** was selected for this project to demonstrate flexibility with non-native Splunk tools.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/4c4b9ff1-be59-4727-88a7-33279e30c278" />

---

## Step 3: Initial Forwarder Configuration

The `/etc/filebeat/filebeat.yml` file was configured to define the log sources (**inputs**) and the Splunk destination (**outputs**).

The `inputs` section was configured to monitor `/var/log/auth.log` and `/var/log/syslog`.
<img width="726" height="543" alt="image" src="https://github.com/user-attachments/assets/5bb34abc-1a5f-4255-8ea5-ac6b071360ec" />

The `outputs` section was configured to point to the local Splunk HEC on port 8088 using a generated token.
<img width="822" height="424" alt="image" src="https://github.com/user-attachments/assets/31bf775c-1bdf-46d1-91af-3b72dac9c21a" />

---

## Step 4: Advanced Troubleshooting

After the initial configuration, no logs appeared in Splunk. This began a multi-stage troubleshooting process that demonstrates a systematic approach to problem-solving—a critical skill for a SOC analyst.

### 4.1 - Diagnosis: Identifying the Root Cause

Though the service appeared to be running, a deeper check of the logs with `journalctl -u filebeat.service` revealed a critical error: **`output type splunk undefined`**.
<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/570b850d-3f22-4929-8a1f-775f60bf75b7" />

This error indicated that the installed version of Filebeat was the OSS (Open Source Software) package, which does not include the necessary Splunk output module. The solution was to perform a clean re-installation with the correct "Standard" package from Elastic. The process involved backing up the configuration, purging the old package, and installing the correct one.

*Backing up the config, purging the old package, and installing the new one:*
<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/e8fe28a1-cbfa-4847-82f6-1eee86529f11" />
<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/768f8d75-ca8e-4ffd-b7d0-97c1cd7f49c1" />
<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/aa3e2f9a-6976-4611-beeb-faf605666b13" />
<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/227759cb-9729-4eb3-a66f-5db907cfdef2" />

### 4.2 - Verification: Isolating the Pipeline with `curl`

Even with the correct software, logs were still not appearing. The next step was to test the Splunk HEC receiver directly, bypassing Filebeat entirely. This was done using a `curl` command. The test initially failed, but revealed two new issues:
1.  The HEC token in Splunk had been disabled. This was re-enabled in the Splunk UI.
2.  The `curl` command failed with an SSL error, indicating a protocol mismatch.

<img width="1918" height="897" alt="image" src="https://github.com/user-attachments/assets/4eb93a5e-ad96-408a-aa7e-86ec20d36a94" />

By changing the protocol in the `curl` command from `https` to `http`, the test was successful, proving the Splunk receiver was now working correctly.
<img width="862" height="322" alt="image" src="https://github.com/user-attachments/assets/65160db6-84cd-4b4e-aec2-8d9a6458f300" />

---

## Step 5: Implementing the Final Fix

The successful `curl` test confirmed the final required change: the `hosts` entry in `filebeat.yml` needed to use `http` instead of `httpss`.

<img width="862" height="322" alt="image" src="https://github.com/user-attachments/assets/6f843af2-a985-4e8e-95dd-0dbf633d92c9" />

With this change, and after restarting the service, the data pipeline was fully functional.

---

## Step 6: Final Verification in Splunk

The final step was to query the data in Splunk. The `curl` test event appeared immediately, and shortly after, the Linux `auth.log` and `syslog` events began streaming in as expected.

<img width="1917" height="892" alt="image" src="https://github.com/user-attachments/assets/f30f7b07-e1ce-4178-951a-8c095a33206c" />

This project successfully established a logging pipeline and demonstrated a comprehensive, multi-step troubleshooting methodology to resolve real-world configuration and software issues.
