# Lab Architecture & Windows VM Setup

This document outlines the foundational setup of the SOC home lab environment. It covers the host machine, the virtualization software, and the creation of the Windows 11 endpoint that will be monitored by the Splunk SIEM.

---
## 1. Lab Architecture Overview

This lab operates on a single host machine which serves as both the SIEM server and a monitored Linux endpoint. Virtualization is used to create an additional Windows endpoint for monitoring, allowing for analysis of events from multiple operating systems.

| Component | Technology / OS | Purpose |
| :--- | :--- | :--- |
| **Host / SIEM / Linux Endpoint** | Linux Mint 21 | Runs Splunk, hosts VMs, and is **also monitored as the primary Linux endpoint**. |
| **Windows Endpoint**| Windows 11 (VM) | A virtualized endpoint used to generate and forward Windows-specific security logs. |

---
## 2. Virtualization Platform Setup

**Oracle VirtualBox** was chosen as the virtualization platform due to its robust feature set and cross-platform compatibility. It was installed on the Linux Mint host using the standard APT package manager.

*Installing VirtualBox from the terminal:*
<img width="654" height="373" alt="image" src="https://github.com/user-attachments/assets/70a3882e-8f3b-42fc-a8ec-8095665bc4e4" />

*The VirtualBox application running on the Linux Mint host:*
<img width="960" height="569" alt="image" src="https://github.com/user-attachments/assets/6143ac97-7bee-426c-9d3b-4a2c4530e92d" />

---
## 3. Windows 11 VM Creation

A new Windows 11 virtual machine was created using an official ISO image. The VirtualBox wizard was used to configure the VM's hardware specifications, including user creation for an unattended installation, RAM allocation, and virtual hard disk size.

*Configuring the VM name, domain, and user accounts:*
<img width="790" height="424" alt="image" src="https://github.com/user-attachments/assets/8cc87fa9-34c4-4bca-aba5-b3d0c73051a4" />
<img width="790" height="424" alt="image" src="https://github.com/user-attachments/assets/47903ec2-f6b6-40b9-bc38-b737868d7552" />

*Allocating memory and creating the virtual hard disk:*
<img width="790" height="424" alt="image" src="https://github.com/user-attachments/assets/92baacc9-6898-4d96-8290-af2077e4ced9" />
<img width="790" height="424" alt="image" src="https://github.com/user-attachments/assets/f9e28ca7-7e2d-4cc7-99ee-79de3a7d72ba" />

---
## 4. Troubleshooting: Resolving the Kernel Driver Error

Upon the first attempt to launch the VM, VirtualBox returned a `VERR_VM_DRIVER_NOT_INSTALLED` error. This is a common issue on Linux hosts where the **Secure Boot** feature in the UEFI/BIOS prevents unsigned third-party kernel modules (like VirtualBox's drivers) from loading.

*The initial error message:*
<img width="500" height="358" alt="image" src="https://github.com/user-attachments/assets/64bf3fdd-0826-4006-bddd-77547da0f712" />

The solution was to reboot the host machine, enter the UEFI/BIOS setup, and disable the **Secure Boot** setting. This is a practical and necessary step for many virtualization and development lab environments on Linux.

*Disabling Secure Boot in the host machine's BIOS:*
<img width="1012" height="655" alt="image" src="https://github.com/user-attachments/assets/8f4c0b0b-7f5d-4d2e-afb6-ff43a3051718" />

---
## 5. Post-Installation and Final Configuration

After resolving the driver issue, the Windows 11 installation proceeded normally.
<img width="1406" height="834" alt="image" src="https://github.com/user-attachments/assets/0ae66c2c-f22d-41a0-bb24-ce555c78de90" />

Once at the desktop, several final configuration steps were taken:
1.  **VirtualBox Guest Additions** were installed to improve performance and enable features like shared clipboards and better screen resolution.
2.  The **Network Adapter** was configured to **`Bridged Adapter`** mode. This allows the VM to have its own IP address on the local network, enabling communication with the Splunk host.
3.  A **Snapshot** of the clean, freshly installed state was taken. This is a critical best practice that creates a "save point," allowing for a quick revert if any future configurations cause issues.

<img width="480" height="353" alt="image" src="https://github.com/user-attachments/assets/a0817525-1faf-4f73-8bcb-6ec98cb7ee80" />

---
## 6. Final Verification

The Windows 11 VM is now fully installed, configured, and running on the VirtualBox platform. The endpoint is ready for the next phase: the installation of Sysmon and the Splunk Universal Forwarder for security monitoring.

<img width="1411" height="839" alt="image" src="https://github.com/user-attachments/assets/9fdfd77a-5701-4ae9-9a9e-1bea3b3e2d5e" />
