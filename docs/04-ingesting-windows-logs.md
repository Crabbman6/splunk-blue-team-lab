# SIEM Log Ingestion: Windows Endpoint with Sysmon

This project documents the process of forwarding logs from a Windows 11 endpoint to a Splunk SIEM. The key focus is on integrating **Sysmon (System Monitor)**, a powerful tool that provides deep visibility into process creation, network connections, and other granular system activities far beyond standard Windows Event Logs.

---
## Step 1: Installing and Configuring Sysmon

The first phase of the project was to install and configure Sysmon on the Windows 11 virtual machine. Sysmon runs as a background service and logs its highly detailed event data to the Windows Event Log, which can then be forwarded to Splunk.

Finding the IP address of my Linux machine running Splunk

<img width="654" height="441" alt="image" src="https://github.com/user-attachments/assets/11fe783f-618f-4d85-9fad-dc344c4aa29c" />

IP address is 192.168.0.194

Image of Sysmon files downloaded in a folder along with the SwiftOnSecurity config file in the folder

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/78c033f0-03f6-4890-917c-c4ce6e4d652b" />

Installing Sysmon in the Powershell terminal including the SwiftOnSecurity config file 

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/514a4bf6-9f3e-4843-9c4e-6184c8974f2b" />

Splunk Forwarder installation on the Windows machine, Deployment Server configuration 

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/c098e73f-a37f-401f-a336-fbfab6628ff9" />

Next screen Receiver Indexer configuration

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/5e9887a4-8606-4c51-bcb2-9f1ddd0a5f50" />

Successful installation screen 

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/ff269af7-14b2-4d47-ab29-a194aa24f534" />

Editing the inputs.conf file within the Sysmon directory

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/518110d0-faf6-404a-acac-41224465c9bb" />

Outbound Firewall rule created allowing the Sysmong logs to forward to the Splunk server

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/046b7ad3-f71d-43a1-bbd3-4c559763671d" />

Image of a successful search in Splunk, showing a search for the index as well as the hostname returning results 

<img width="1919" height="894" alt="image" src="https://github.com/user-attachments/assets/3c092cbf-cbaa-4b09-ae02-62ae8aa2a39b" />

Troubleshooting steps for finding why source_type="Sysmon" is not showing any results on Splunk

Looking at the splunkd log file and finding it is due to permissions error 

<img width="969" height="649" alt="image" src="https://github.com/user-attachments/assets/278a71fa-df88-465f-80f2-803c7017d119" />

Using the command prompt to grant the necessary permissions 

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/ddb4de19-f2fc-4c38-b4e5-9937e3d31e82" />

Event Viewer still not showing me the Sysmon logs 

<img width="976" height="687" alt="image" src="https://github.com/user-attachments/assets/aeded886-e0e5-4108-8514-2ed7901c01b2" />

Found the underlying issue, the Sysmon file was located in the Users folder so it was denying permissions. I moved it to C:\Tools\Sysmon and completed the install in the terminal there below is an image of the event viewer showing correct logs

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/79e1b2c8-05ff-4725-810b-f6662e02ef2c" />

Running the wevtutil command once more 

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/f425993a-5d27-4630-8d53-4147ca0ab868" />

After troubleshooting and reading on forums, the issue was with the user account that was running the service. Image below shows the change I made

<img width="1024" height="840" alt="image" src="https://github.com/user-attachments/assets/5d0db3a0-1680-4689-8e39-417f249cf1db" />

Finally, a successful index search showing Sysmon events in Splunk

<img width="1919" height="896" alt="image" src="https://github.com/user-attachments/assets/3e20c185-e390-4601-b6a6-29766d380f8a" />












