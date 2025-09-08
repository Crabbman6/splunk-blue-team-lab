# SIEM Log Ingestion: Linux Endpoint

This project documents the end-to-end process of ingesting logs from a Linux endpoint into a Splunk SIEM. This write-up is presented as a two-part case study. **Part 1** covers an in-depth troubleshooting exercise with a third-party shipper (Filebeat). **Part 2** details the successful implementation using Splunk's native Universal Forwarder.

---
## Part 1: Ingestion via Filebeat (A Troubleshooting Case Study)

This initial phase focused on using Filebeat to demonstrate flexibility with different toolsets. While the final implementation was changed, the troubleshooting process provided a valuable, real-world learning experience.

### Step 1.1: Log Source Analysis & Initial Setup
The project began by identifying high-value log sources (`/var/log/auth.log` and `/var/log/syslog`) and performing an initial installation and configuration of the Filebeat agent.

<img width="822" height="441" alt="image" src="https://github.com/user-attachments/assets/af1cded7-8da3-4267-9f8c-9aa82dff000e" />
<img width="726" height="543" alt="image" src="https://github.com/user-attachments/assets/5bb34abc-1a5f-4255-8ea5-ac6b071360ec" />

### Step 1.2: Advanced Troubleshooting
After the initial setup, no logs appeared in Splunk. A systematic investigation was launched, demonstrating a core SOC analyst skillset. Key findings included:

* **Incorrect Software Package:** A `journalctl` review revealed the error **`output type splunk undefined`**. This proved the installed Filebeat was the OSS version, which lacks the Splunk output module. The issue was resolved by purging the old package and installing the correct "Standard" version.
* **Splunk HEC Validation:** The Splunk HEC input was tested directly with `curl`. This test identified and resolved two additional issues: the HEC token was disabled in Splunk, and the connection required `http` instead of `https`.

<img width="1174" height="322" alt="image" src="https://github.com/user-attachments/assets/570b850d-3f22-4929-8a1f-775f60bf75b7" />
<img width="862" height="322" alt="image" src="https://github.com/user-attachments/assets/65160db6-84cd-4b4e-aec2-8d9a6458f300" />
<img width="1918" height="897" alt="image" src="https://github.com/user-attachments/assets/4eb93a5e-ad96-408a-aa7e-86ec20d36a94" />

### 1.3: Strategic Pivot
Despite resolving multiple complex issues, the Filebeat pipeline remained unstable. At this point, a strategic decision was made to pivot to Splunk's native Universal Forwarder, which is the best-practice solution for this use case.

---
## Part 2: Ingestion via Splunk Universal Forwarder (Best Practice)

This phase details the successful implementation using the Splunk Universal Forwarder (UF) for a robust and seamless integration.

### Step 2.1: System Preparation
The Filebeat package was completely purged from the system to ensure a clean environment for the new agent.

<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/ee1ea412-0399-4be8-8229-02981f7bf938" />

### Step 2.2: Universal Forwarder Installation
The latest UF tarball was downloaded and extracted to the standard `/opt` directory.

<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/4d0014eb-1b24-417c-a1ba-de34d4885137" />
<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/a17fac8e-7821-45fc-9f3e-2deaefa15729" />

### Step 2.3: Initial Forwarder Setup
During the first run, a port conflict on the default management port (8089) was identified, as the main Splunk instance was already using it. The conflict was resolved by reconfiguring the forwarder to use port 8090.

<img width="654" height="577" alt="image" src="https://github.com/user-attachments/assets/45e3d527-f521-437d-8a26-57626bf6a300" />
<img width="654" height="373" alt="image" src="https://github.com/user-attachments/assets/7ee1cf12-497e-4f94-8e43-972e5dc42117" />

### Step 2.4: Configuring the Data Pipeline
The data pipeline was configured in two steps:
1.  **Splunk Receiver:** The main Splunk instance was configured to listen for incoming forwarder data on port `9997`.
2.  **Forwarder Sender:** The UF was configured to send data to `localhost:9997` and to monitor the `/var/log/auth.log` and `/var/log/syslog` files.

<img width="943" height="241" alt="image" src="https://github.com/user-attachments/assets/191b12a3-fce7-4e7a-97f8-328d46c33fcb" />
<img width="654" height="373" alt="image" src="https://github.com/user-attachments/assets/288b65bf-7d5e-433d-ba3d-81dc3c5230db" />
<img width="654" height="373" alt="image" src="https://github.com/user-attachments/assets/5bbc2b94-80a2-40f9-8889-fffbfc14b3db" />

### Step 2.5: Verification and Success
After restarting the forwarder, logs began streaming into Splunk immediately. The final search results confirm that authentication logs and general system logs from the Linux host (`Selena`) are being successfully ingested and indexed.

<img width="1919" height="893" alt="image" src="https://github.com/user-attachments/assets/e00ab1da-935e-4058-9512-7ff34cfd27c9" />
<img width="1919" height="891" alt="image" src="https://github.com/user-attachments/assets/d595c13d-da51-4cbc-9022-cc237cf62607" />
