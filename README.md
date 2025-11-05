\# Enterprise AD DS Build   

\*Windows Server 2022\*  



---



\## Overview

\*\*Project Goal:\*\*  

Build a fully functional Windows Server 2022 Active Directory Domain Services (AD DS) environment \*\*entirely through the GUI\*\*, documenting every stage with screenshots and brief explanations.  



\*\*Scope:\*\*  

This project focuses on learning the logical flow of an enterprise domain setup — understanding the “why” behind each step before automating it in later PowerShell-based projects.  

PowerShell was used \*\*only to verify network configuration\*\* using `ipconfig /all` before and after the build. However, I will be recreating this environment using only PowerShell in due time.



---



\## Phase 1 – Lab Setup  

\*\*Objective:\*\* Establish a clean, ready-to-configure Windows Server 2022 environment.  



\### Actions

\- Verified that Windows Server 2022 booted to \*\*Server Manager\*\* with no roles installed.  

\- Installed available updates and confirmed the system time zone was correct.  

\- Created a local project folder structure for organization:  



Windows-Server-2022-Lab

├─ Docs/				(Explanations and Documentation of procedural steps)

├─ Images/			(Database for all images screenshotted)

└─ README.md			(Overview of the project)



\- Captured baseline screenshots of Server Manager and Device Manager.  

\- Ran `ipconfig /all` in PowerShell and saved the output for reference.



\*\*Evidence:\*\*  

\- `exports/ipconfig-before.txt`  

\- `assets/images/00-server-manager-baseline.png`



\*\*Notes:\*\*  

This first snapshot captures the system in its clean, unconfigured state.  

It provides a starting point for comparison as each role and setting is applied later.



---



\## Phase 2 – Server Baseline Configuration  

\*\*Objective:\*\* Rename the server, assign a static IP address, and verify connectivity.  



\### Actions

\- Opened \*\*Server Manager → Local Server → Computer Name\*\* and changed the name to `DC01`.  

\- Rebooted the system to apply changes.  

\- Opened \*\*Network Connections → Ethernet → Properties → IPv4 Settings\*\* and configured:  



IP Address: 192.168.20.5

Subnet Mask: 255.255.255.0

Default Gateway: (blank)

Preferred DNS: 127.0.0.1



\- Verified the configuration using PowerShell:



ipconfig /all



\- Took screenshots of the NIC settings and Server Manager showing updated name and IP.



\*\*Evidence:\*\*

\- `exports/ipconfig-before.txt`

\- `assets/images/00-server-manager-baseline.png`



---



\## ⚙️ Phase 3 – Installing AD DS and DNS Roles  



\*\*Objective:\*\* Use \*\*Server Manager\*\* to install the core roles required for Active Directory Domain Services (AD DS) and DNS.



---



\### Actions  

\- Opened \*\*Server Manager\*\* from the Start menu.  

\- Selected \*\*Manage → Add Roles and Features\*\* to launch the installation wizard.  

\- Chose \*\*Role-based or feature-based installation\*\*, then selected the local server (`DC01`).  

\- From the server roles list, selected:

&nbsp;  -  \*\*Active Directory Domain Services (AD DS)\*\*  

&nbsp;  -  \*\*DNS Server\*\*  

&nbsp;  -  \*\*Group Policy Management\*\*  

\- Left all other options at their defaults and clicked \*\*Install\*\*.  

\- Waited for the installation to complete and confirmed success in the \*\*Results\*\* window.  

\- Verified completion via the yellow notification flag in Server Manager, which indicated that post-deployment configuration (domain promotion) was required.



---



\### 📸 Evidence Collected  

\- `assets/images/02-role-install-summary.png` – Installation summary screen  

\- `assets/images/02-server-manager-notification.png` – Server Manager AD DS configuration prompt  



---



\### Notes 

This phase establishes the server’s identity and management foundation.  

Installing \*\*Active Directory Domain Services\*\*, \*\*DNS\*\*, and \*\*Group Policy Management\*\* provides the backbone for user authentication, name resolution, and centralized policy enforcement.  



---



\## Phase 4 – Domain Promotion  

\*\*Objective:\*\* Promote `DC01` to a domain controller and create a new Active Directory forest named `JGCyber.net`.  



---



\### Actions  

\- In \*\*Server Manager\*\*, clicked the yellow notification flag and selected \*\*Promote this server to a domain controller\*\*.  

\- Chose \*\*Add a new forest\*\* and entered the root domain name:  

&nbsp; `JGCyber.net`  

\- Left the default options selected for \*\*DNS Server\*\* and \*\*Global Catalog (GC)\*\* roles.  

\- Entered and confirmed a \*\*Directory Services Restore Mode (DSRM)\*\* password. (Store this password offline for better security.)  

\- Accepted the default paths for \*\*Database\*\*, \*\*Log files\*\*, and \*\*SYSVOL\*\*:  



C:\\Windows\\NTDS

C:\\Windows\\NTDS

C:\\Windows\\SYSVOL





\- Clicked \*\*Next\*\* through the prerequisite check summary and verified all checks passed successfully.  

\- Selected \*\*Install\*\*, allowing the server to automatically reboot after promotion.  



---



\### 📸 Evidence Collected  

\- `assets/images/03-domain-promotion-summary.png` – Summary screen before installation  

\- `assets/images/03-domain-promotion-success.png` – Confirmation of successful promotion  

\- `assets/images/03-server-manager-post-reboot.png` – Server Manager dashboard showing new domain  



---



\### Notes  

This phase created the new \*\*forest root domain\*\* `JGCyber.net`, transforming the standalone server into a \*\*domain controller\*\*.  

DNS integration was automatically configured during the promotion process, ensuring that the server can resolve its own AD-integrated records.  

After reboot, all core domain services were verified as healthy, marking the successful establishment of the Active Directory environment.



---



\## 🧭 Phase 5 – Post-Promotion Verification  

\*\*Objective:\*\* Verify that Active Directory Domain Services (AD DS) and DNS are properly installed, configured, and operational after promoting the server to the domain `JGCyber.net`.  



---



\### Actions  

\- After reboot, logged into `DC01` using the domain administrator account (`JGCyber\\Administrator`).  

\- Opened \*\*Server Manager\*\* to confirm the following roles were listed under “Roles and Features”:  

&nbsp; - \*\*Active Directory Domain Services\*\*  

&nbsp; - \*\*DNS Server\*\*  

&nbsp; - \*\*Group Policy Management\*\*  

\- Opened \*\*Active Directory Users and Computers (ADUC)\*\* to confirm the new domain structure was visible under:  

&nbsp; `JGCyber.net`  

&nbsp; Default containers observed:  

&nbsp; - Builtin  

&nbsp; - Computers  

&nbsp; - Domain Controllers  

&nbsp; - Users  

\- Opened \*\*DNS Manager\*\* and verified that the \*\*Forward Lookup Zone\*\* for `JGCyber.net` was automatically created.  

&nbsp; - Confirmed the presence of \*\*A\*\*, \*\*SOA\*\*, and \*\*SRV\*\* records for `DC01`.  

\- Verified that \*\*Netlogon\*\* and \*\*ADWS (Active Directory Web Services)\*\* services were running.  

\- Opened \*\*PowerShell\*\* and ran "ipconfig /all" to confirm the following:



Primary DNS Suffix: JGCyber.net



DNS Servers: 127.0.0.1



Captured screenshots of ADUC and DNS Manager showing the active domain and functional DNS zone.

