# SIEM Log Ingestion: Linux Endpoint

This project documents the foundational process of ingesting logs from a Linux endpoint into a Splunk SIEM. Centralizing logs is a core function of a **Security Operations Center (SOC)**, enabling analysts to monitor for threats, investigate incidents, and maintain security posture across an environment.

---

## Step 1: Log Source Analysis

The first step is to identify the most valuable log sources on the Linux machine. These logs, located in `/var/log`, contain detailed records of system and user activity. For a SOC analyst, certain logs are more critical than others.

**Key Logs for Security Monitoring:**
* `/var/log/auth.log`: This is a **high-value security log**. It records all authentication events, including successful and failed SSH login attempts, `sudo` command usage (privilege escalation), and user account modifications. Analyzing this log is critical for detecting brute-force attacks, unauthorized access, and insider threats.
* `/var/log/syslog`: This log provides broad **operational context**. It captures messages from the kernel, system daemons, and various applications. It's useful for correlating system errors or service crashes with potential malicious activity.

The following image confirms the presence of these log files on the target endpoint.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/af1cded7-8da3-4267-9f8c-9aa82dff000e" />

---

## Step 2: Forwarder Deployment

A **log forwarder** is a lightweight agent installed on an endpoint to collect logs and send them to a central location, in this case, our Splunk SIEM.

For this project, **Filebeat** was selected as the forwarder. While Splunk has a native Universal Forwarder, experience with Filebeat is also valuable as it's widely used and demonstrates flexibility with different toolsets. The image below confirms the successful installation of the Filebeat agent on the Linux host.


<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/4c4b9ff1-be59-4727-88a7-33279e30c278" />

---

## Next Steps

With the agent installed, the next critical phase is **configuration**. This involves editing the `filebeat.yml` file to define which logs to monitor and where to send them (i.e., the Splunk instance's address and authentication token).
