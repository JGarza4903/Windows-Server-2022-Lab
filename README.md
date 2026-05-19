🖥️ Windows Server 2022 Enterprise Lab
Status Platform Domain Focus

A hands-on Windows Server 2022 enterprise lab environment simulating real-world identity management, network services, access control, and security hardening — built from scratch using Hyper-V virtualization.

📋 Table of Contents
Overview
Lab Environment
Technologies Used
Skills Demonstrated
Lab Phases
Phase 1 – Baseline Installation
Phase 2 – Server Identity & Networking
Phase 3 – AD DS & DNS Role Installation
Phase 4 – Domain Promotion & Forest Creation
Phase 5 – Active Directory Users, Groups & Client Integration
Phase 6 – File Server & NTFS Permission Enforcement
Phase 7 – Group Policy (Planned)
Phase 8 – Hardening & Security Baselines (Planned)
Active Directory Topology
Troubleshooting Log
What I Would Do Differently
Planned Additions
Overview
This lab simulates a small but realistic enterprise domain environment built on Windows Server 2022 and Hyper-V. It is not a scripted walkthrough — every configuration decision, mistake, and fix was made independently to reflect real-world troubleshooting and systems thinking.

The focus areas are:

Identity and access management via Active Directory
Name resolution through DNS
Centralized authentication and domain join
File server configuration with layered permission enforcement
Security foundations including NTFS permissions, least-privilege access, and GPO (in progress)
This project is designed to demonstrate job-ready skills for IT Support, Systems Administration, and entry-level Security roles.

Lab Environment
Component	Details
Hypervisor	Microsoft Hyper-V
Server OS	Windows Server 2022 (Desktop Experience)
Client OS	Windows 11 Enterprise Evaluation, Windows 10
Domain Name	NetLabz.local
Domain Controller	DC01
Topology	Single-domain, single-site (expanding)
Network	Static IP addressing, internal Hyper-V switch
Technologies Used
Windows Server 2022
Active Directory Domain Services (AD DS)
DNS Server
DHCP
Hyper-V (Gen 2 VMs)
NTFS & Share Permissions
Group Policy (in progress)
PowerShell (expanding)
TCP/IP Networking
Skills Demonstrated
Deploying and promoting a Domain Controller from scratch
Configuring AD DS, DNS, and DHCP roles
Designing an OU structure for departmental organization
Managing users, groups, and computer objects in Active Directory
Implementing RBAC via security group assignment
Configuring file shares and enforcing NTFS least-privilege permissions
Disabling permission inheritance and applying explicit access control entries
Diagnosing and resolving DNS, subnet, and domain join failures
Documenting troubleshooting steps with evidence and root cause analysis
Lab Phases
Phase 1 – Lab Setup & Server Installation
Objective: Establish a clean Windows Server 2022 baseline in a virtualized environment.

Steps Taken:

Created a Generation 2 VM in Hyper-V
Installed Windows Server 2022 (Desktop Experience)
Verified baseline system configuration
Issue Encountered:

PXE over IPv4 boot error on first launch
Root cause: incorrect VM boot order (ISO not prioritized)
Resolution: Reordered boot devices to prioritize ISO in VM firmware settings
Evidence:

Server Baseline

Phase 2 – Server Identity & Networking
Objective: Prepare the server for Active Directory by stabilizing identity and network configuration.

Steps Taken:

Renamed server to DC01
Assigned a static IPv4 address
Pointed DNS to itself (127.0.0.1) in preparation for AD DS
Verified NTP/time synchronization
Why This Matters: Active Directory requires stable DNS, static IP addressing, and time synchronization. Without these, AD promotion and Kerberos authentication will fail.

Evidence:

Configuration	Screenshot
Static IP Assignment	View
ipconfig Verification	View
Time Sync Verification	View
Phase 3 – AD DS & DNS Role Installation
Objective: Install the Active Directory Domain Services and DNS Server roles prior to domain promotion.

Steps Taken:

Installed AD DS role via Server Manager
Installed DNS Server role
Confirmed role installation before proceeding to promotion
Why This Matters: Separating role installation from forest creation allows validation of system readiness and reduces configuration risk during the promotion process.

Evidence:

Step	Screenshot
Role Installation	View
Role Confirmation	View
Phase 4 – Domain Promotion & Forest Creation
Objective: Create a new Active Directory forest and promote DC01 to a Domain Controller.

Steps Taken:

Created new forest: NetLabz.local
Promoted DC01 to Domain Controller
Verified AD-integrated DNS zone creation
Confirmed ADUC and DNS Manager post-promotion
Why This Matters: The forest is the highest-level security boundary in Active Directory. Domain naming, DNS integration, and functional level selection directly impact long-term stability and feature availability.

Evidence:

Step	Screenshot
Domain Promotion	View
ADUC Console	View
DNS Zone Verification	View
Phase 5 – Active Directory Users, Groups & Client Integration
Objective: Build out the AD structure and successfully join a Windows 11 client to the domain.

Steps Taken:

Created Organizational Units: Research, Human Resources, Production
Built user template accounts for consistent provisioning
Created domain users by copying templates
Created security groups aligned to department roles
Pre-staged a computer object in ADUC
Deployed a Windows 11 Enterprise Evaluation client VM
Completed domain join and validated computer object placement
Issue Encountered:

Domain join failed with a connectivity error despite correct credentials and a pre-staged computer object.

Domain Join Error

Root Cause Analysis:

Using ipconfig /all on the client revealed three misconfigurations:

Client IP was not in the same subnet as DC01
Subnet mask was incorrect
DNS server was not pointing to the Domain Controller
Resolution:

Assigned a static IP within the correct subnet
Corrected the subnet mask
Set DNS to DC01's IP address
Re-attempted domain join — successful
Network Fix

Validation:

Client displayed full domain membership: RES-COMP-01.NetLabz.local
Domain credentials authenticated successfully
Computer object appeared in the correct OU
Domain Join Confirmation

Key Takeaway: Active Directory will always fail if DNS and subnet configuration are wrong — regardless of how well the AD objects are configured.

Phase 6 – File Server & NTFS Permission Enforcement
Objective: Configure a centralized file server using AD security groups, enforce least-privilege NTFS permissions, and validate access from a domain-joined client.

Steps Taken:

Created C:\Shared_Folder with department subfolders: HR_Shared, Production_Shared, Research_Shared
Enabled Advanced Sharing and configured share permissions using AD security groups
Disabled NTFS inheritance on department folders to enforce explicit permissions
Applied explicit ACEs:
Administrators – Full Control
SYSTEM – Full Control
Department security group – Modify
Validated access with a domain user from the HR group
Verified access denial for users outside the assigned group
Issues Encountered:

Disabling inheritance removed all inherited permissions, temporarily locking the folder
Share permissions alone were insufficient; NTFS permissions also had to be configured
Permission controls were grayed out until ownership was confirmed
Test user also required membership in Remote Desktop Users to connect for validation
RDP Group Fix

Validation:

Logged in as Paige Turner (HR user) from Win11_RES-COMP-01
Successfully accessed \\DC01\HR_Shared and created a test folder
Confirmed access denial for users not in the HR security group
Key Takeaway: Share permissions and NTFS permissions are independent layers. The most restrictive permission always applies — both must be configured correctly for access to work as intended.

Evidence:

Step	Screenshot
Shared Folder Creation	View
Advanced Sharing Config	View
Network Path Validation	View
NTFS Permission Config	View
Inheritance Disabled	View
Explicit Permissions Applied	View
Client Access Test	View
Access Denied Proof	View
Phase 7 – Group Policy Implementation (Planned)
Objective: Implement Group Policy Objects to enforce security and workstation configuration at scale.

Planned Tasks:

Create and link GPOs at the OU level
Enforce password policies, screen lock, and account lockout thresholds
Restrict access to Control Panel and removable media for standard users
Test GPO inheritance, precedence, and filtering
Use gpresult and rsop.msc to validate applied policies
Document policy conflicts and resolution steps
Steps to Implement:

Open Group Policy Management (gpmc.msc) on DC01
Right-click an OU (e.g., Human Resources OU) → Create a GPO in this domain and link it here
Name it (e.g., HR_SecurityBaseline)
Right-click the new GPO → Edit
Navigate to Computer Configuration > Policies > Windows Settings > Security Settings to configure account and audit policies
On a domain client, run gpupdate /force then gpresult /r to verify the policy was applied
Phase 8 – Hardening & Security Baselines (Planned)
Objective: Apply security hardening aligned with real-world best practices and test troubleshooting skills.

Planned Tasks:

Review and document the Kerberos authentication flow
Apply Microsoft Security Baseline settings via GPO
Disable legacy protocols (e.g., NTLMv1, SMBv1)
Introduce intentional misconfigurations to practice diagnosis and remediation
Document attack surfaces and hardening decisions with reasoning
Steps to Implement (SMBv1 Disable Example):

On DC01, open PowerShell as Administrator
Check current state: Get-SmbServerConfiguration | Select EnableSMB1Protocol
Disable SMBv1: Set-SmbServerConfiguration -EnableSMB1Protocol $false -Force
Verify: Get-SmbServerConfiguration | Select EnableSMB1Protocol
Document the change and test that domain-joined clients can still access shares
Active Directory Topology
Current snapshot of the NetLabz.local domain structure. See Topology.md for the full breakdown.

NetLabz.local
├── Domain Controllers OU
│   └── Computers: DC01
│
├── Human Resources OU
│   ├── Users: Paige Turner, Willie Makit, HR User Template
│   └── Groups: HR_Employees, HR_Administration
│
├── Production OU
│   ├── Users: Billy Bob, Janet Jackson, Jerry Seinfeld, ProductionUser Template
│   └── Groups: ProductionEmployees, ProductionManagers
│
└── Research OU
    ├── Users: Bill Nye, Nikola Tesla, RES_User Template
    ├── Computers: RES-COMP-01, RES-COMP-02
    └── Groups: Research_Employees, Research_Managers
Troubleshooting Log
A summary of real issues encountered and resolved throughout this lab.

Phase	Issue	Root Cause	Resolution
1	PXE IPv4 boot error	Incorrect VM boot order	Prioritized ISO in Hyper-V firmware
5	Domain join failure	Client subnet/DNS misconfigured	Corrected static IP, subnet, and DNS
5	Computer object not associating	Pre-staged name mismatch	Verified hostname matched pre-staged object
6	Permissions greyed out	Ownership not set after disabling inheritance	Confirmed ownership, re-applied explicit ACEs
6	Share accessible but NTFS denied	Share permissions and NTFS not aligned	Configured both layers to match
6	RDP validation failing for test user	User not in Remote Desktop Users group	Added domain user to RDP Users group
What I Would Do Differently
Honest reflection improves both the lab and your credibility with recruiters.

Capture evidence before fixing issues — Phase 5's DNS mismatch was fixed before a screenshot was taken. Going forward, I document before making changes.
Expand PowerShell usage earlier — Manual GUI configuration works, but automating user creation and OU structure with PowerShell would demonstrate scripting fundamentals and scale better in real environments.
Use DHCP from the start — The lab currently uses static addressing throughout. Adding a DHCP scope would reflect how most enterprise environments actually function.
Add a second domain controller — Replication and redundancy are core AD concepts. A secondary DC would demonstrate understanding of forest/domain replication topology.
Planned Additions
[ ] Phase 7 – Group Policy Objects (account lockout, password policy, desktop restrictions)
[ ] Phase 8 – Security baselines, SMBv1 disabled, NTLM hardening
[ ] PowerShell scripts for bulk user creation and OU provisioning
[ ] DHCP scope configuration and lease management
[ ] Secondary Domain Controller + AD replication
[ ] Audit policy configuration and Windows Event Log review
[ ] Network diagram (visual topology)
📌 Project Philosophy
This lab prioritizes realism over perfection. Every mistake is documented alongside the fix because that's how real IT work happens. The goal isn't a clean walkthrough — it's evidence that I can deploy, troubleshoot, and reason through enterprise infrastructure independently.

Built and maintained by JGarza4903
