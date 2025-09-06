# SIEM Initialization and Health Check

This document outlines the standard procedure for initializing the Splunk service, accessing its web-based management console, and performing a basic system health check. This is a critical first step for any analyst or engineer to ensure the SIEM is fully operational before configuring data inputs or building detections.

---

## Step 1: Starting the Splunk Service

By default, Splunk is installed in the `/opt/splunk/` directory on Linux systems. All command-line operations are executed from the `/bin` subdirectory.

The core Splunk process, the **`splunkd` daemon**, is initiated using the `splunk start` command. On the first launch, Splunk requires accepting the software license agreement.

```bash
# Navigate to the Splunk binary directory
cd /opt/splunk/bin
```

# Start the Splunk service with superuser privileges

```bash
sudo ./splunk start
```

<img width="734" height="441" alt="image" src="https://github.com/user-attachments/assets/9d3d13b8-d7f5-4a94-9c21-4161b5c94adc" />

<img width="734" height="441" alt="image" src="https://github.com/user-attachments/assets/dca5cbb3-01f0-4631-8a73-eeb77535ded8" />

## Step 2: Accessing the Splunk Web UI

Once the service is running, the Splunk web interface becomes available. This is the primary GUI for searching data, building dashboards, and performing administrative tasks. By default, it runs on **port 8000**.

* **URL:** `http://localhost:8000`

<img width="570" height="425" alt="image" src="https://github.com/user-attachments/assets/3f0aa391-b3a1-4acd-8403-9ff7e088b2a9" />

<img width="1916" height="895" alt="image" src="https://github.com/user-attachments/assets/5edb6cdd-ee04-4559-8706-ab8dbaa32f19" />

---

## Step 3: Performing an Initial Health Check

Before ingesting any external logs, it's a **critical best practice** to verify that the Splunk instance itself is healthy. This is done by searching Splunk's own internal logs.

The `index=_internal` search queries the data that Splunk generates about its own operations and performance. Seeing recent events in this index confirms that the core indexing and search functions are working correctly. It is the SIEM's equivalent of **checking for a heartbeat**, verifying it is ready to receive and process data.

<img width="1917" height="898" alt="image" src="https://github.com/user-attachments/assets/fb145007-494c-4413-93b4-297b3f6a8bcb" />

This successful query confirms the Splunk instance is operational and ready for the next step: configuring data inputs.
