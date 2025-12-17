# Windows Server 2022 – Single Domain Lab (Phase 1)

## Overview
This project documents the initial setup and baseline configuration of a Windows Server 2022 environment using Hyper-V. The goal of this lab is to simulate a small enterprise server environment and establish a strong foundation for Active Directory, DNS, and client integration in later phases.

This project emphasizes **hands-on system administration**, **virtualization fundamentals**, and **professional documentation practices** aligned with real-world IT support and junior systems roles.

---

## Environment & Tools
- **Hypervisor:** Hyper-V Manager  
- **Operating System:** Windows Server 2022 Standard Evaluation (Desktop Experience)  
- **Host Platform:** Windows 11 Pro with Hyper-V enabled  

---

## Virtual Machine Configuration
The virtual machine was created in Hyper-V Manager with the following specifications:

- **VM Name:** `Win_Server_22`
- **Generation:** 2
- **Memory:** 4 GB RAM
- **Networking:** Internal Virtual Switch
- **Storage:** New dynamically expanding `.vhdx` virtual disk
- **Installation Media:** Windows Server 2022 ISO (Microsoft Evaluation)
[!IMPORTANT]
This configuration reflects a small-business server setup while remaining resource-efficient for a home lab environment.

---

## Installation Process
1. Downloaded the Windows Server 2022 Evaluation ISO directly from [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022).
2. Created a new Generation 2 virtual machine in Hyper-V Manager.
3. Attached the ISO file as the boot media.
4. Selected **Windows Server 2022 Standard Evaluation (Desktop Experience)** to allow full GUI-based administration.
5. Completed the setup by creating an administrator username and password.

Screenshots documenting the installation process and VM configuration are stored in the `Images/phase1/` directory.

---

## Troubleshooting & Problem Solving
During setup, I encountered and resolved several common issues:

### ISO Download Issue
- Initial installation attempts failed due to using an expired ISO file. 
- Resolution: Verified the ISO source and re-downloaded a fresh evaluation copy directly from Microsoft.

### PXE over IPv4 Boot Error
- The virtual machine repeatedly attempted to PXE boot instead of loading the installer.
- Root Cause: Incorrect boot order configuration.
- Resolution: Adjusted the VM firmware settings to prioritize the virtual DVD drive containing the ISO.

These issues highlighted the importance of validating installation media and understanding VM boot processes—skills directly applicable to enterprise troubleshooting scenarios.

---

## Skills Demonstrated
- Virtual machine provisioning with Hyper-V
- Windows Server 2022 installation and configuration
- Understanding of VM boot order and PXE behavior
- Troubleshooting installation and media-related issues
- Technical documentation and evidence collection

---

## Evidence
Screenshots and supporting images are organized by phase:

![Creating the Virtual Machine in Hyper-V](Images/phase1/VM_Setup.png)

---

![Installing Standard Edition - Desktop Experience](Images/phase1/winServerSetup2.png)

---

![Confirming successful installation of Windows Server 2022](Images/phase1/install_confirmation.png)
