# SIEM Log Ingestion: Linux Endpoint

This project documents the end-to-end process of ingesting logs from a Linux endpoint into a Splunk SIEM. Centralizing logs is a core function of a **Security Operations Center (SOC)**, enabling analysts to monitor for threats, investigate incidents, and maintain security posture across an environment.

---

## Step 1: Log Source Analysis

The first step is to identify the most valuable log sources on the Linux machine. These logs, located in `/var/log`, contain detailed records of system and user activity. For a SOC analyst, certain logs are more critical than others.

**Key Logs for Security Monitoring:**
* `/var/log/auth.log`: This is a **high-value security log**. It records all authentication events, including successful and failed SSH login attempts, `sudo` command usage (privilege escalation), and user account modifications. Analyzing this log is critical for detecting brute-force attacks, unauthorized access, and insider threats.
* `/var/log/syslog`: This log provides broad **operational context**. It captures messages from the kernel, system daemons, and various applications. It's useful for correlating system errors or service crashes with potential malicious activity.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/af1cded7-8da3-4267-9f8c-9aa82dff000e" />

---

## Step 2: Forwarder Deployment

A **log forwarder** is a lightweight agent installed on an endpoint to collect logs and send them to a central location. For this project, **Filebeat** was selected. While Splunk has a native Universal Forwarder, experience with Filebeat demonstrates flexibility with different toolsets. The image below confirms the successful installation of the agent.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/4c4b9ff1-be59-4727-88a7-33279e30c278" />

---

## Step 3: Forwarder Configuration

With the agent installed, the next phase is configuration. This involves editing the `/etc/filebeat/filebeat.yml` file to define which logs to monitor (**inputs**) and where to send them (**outputs**).

First, the `filebeat.inputs` section was configured to monitor the target log files. Custom fields were added to tag the data for easier searching in Splunk.

<img width="726" height="543" alt="image" src="https://github.com/user-attachments/assets/5bb34abc-1a5f-4255-8ea5-ac6b071360ec" />

Next, the `output.splunk` section was configured with the Splunk instance's local address and the unique HEC token generated from the Splunk UI.

<img width="822" height="424" alt="image" src="https://github.com/user-attachments/assets/31bf775c-1bdf-46d1-91af-3b72dac9c21a" />

---

## Step 4: Troubleshooting and Validation

After configuring the YAML file, the Filebeat service was enabled to run on startup and then started.
<img width="822" height="356" alt="image" src="https://github.com/user-attachments/assets/f8cfd732-2a02-49ca-9404-866dbbe6ad3c" />

Initially, the service failed to start, entering a crash loop. This is a common issue when configuring YAML files. The troubleshooting process involved:
1.  Using `sudo systemctl status filebeat` to identify the crash loop.
2.  Using `sudo filebeat test config -e` to check for syntax errors.
3.  Reviewing the configuration, which revealed the root cause: the default `output.elasticsearch` was still active alongside the new `output.splunk`. Filebeat can only have one active output.

After commenting out the `output.elasticsearch` section, the service started successfully. This validation demonstrates a key analyst skill: **systematic troubleshooting**.
<img width="1358" height="543" alt="image" src="https://github.com/user-attachments/assets/9e824ebf-a43e-4a16-b988-189733c06f7a" />

---

## Step 5: Verifying Data Ingestion in Splunk

The final step is to confirm that the data pipeline is working by querying the logs in Splunk. A simple search for `index="main"` in the Search & Reporting app confirms that Linux logs are being successfully received, parsed, and indexed by the SIEM.

[Add a screenshot of your Splunk search results here]


Troubleshooting starts after i cannot find anything from search queries

Journalctl to find the error log message: 

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/570b850d-3f22-4929-8a1f-775f60bf75b7" />

Copying backup file of the yaml config for filebeat

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/e8fe28a1-cbfa-4847-82f6-1eee86529f11" />

Purging current filebeat install 

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/768f8d75-ca8e-4ffd-b7d0-97c1cd7f49c1" />

Sudo wget filebeat package

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/aa3e2f9a-6976-4611-beeb-faf605666b13" />

sudo installing the package 

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/227759cb-9729-4eb3-a66f-5db907cfdef2" />

Still did not work, final change was checking the HEC token, and finding it was disabled within Splunk. Screenshot is me enabling it. 

<img width="1918" height="897" alt="image" src="https://github.com/user-attachments/assets/4eb93a5e-ad96-408a-aa7e-86ec20d36a94" />

Curl command with a successful return after removing https to http 

<img width="862" height="322" alt="image" src="https://github.com/user-attachments/assets/65160db6-84cd-4b4e-aec2-8d9a6458f300" />

Searching index="main" in splunk to find the successful search result 

<img width="1917" height="892" alt="image" src="https://github.com/user-attachments/assets/f30f7b07-e1ce-4178-951a-8c095a33206c" />

Making the change in the filebeat.yaml file, output https to http 

<img width="862" height="322" alt="image" src="https://github.com/user-attachments/assets/6f843af2-a985-4e8e-95dd-0dbf633d92c9" />





