# Windows Server 2022 – Enterprise Domain Lab

## Quick Snapshot
- Hyper-V based Windows Server 2022 lab
- Single-domain Active Directory environment
- Focused on enterprise fundamentals and troubleshooting
- Built and documented for entry-level IT roles

---

## Skills Demonstrated
- Windows Server administration
- Active Directory & DNS fundamentals
- Virtualization with Hyper-V
- Network configuration & troubleshooting
- Technical documentation

---

<details>
<summary><strong>Phase 1 – Lab Setup & Server Installation</strong></summary>

### Objective
Establish a clean Windows Server 2022 baseline in a virtualized environment.

### Implementation Steps
- Created Generation 2 VM in Hyper-V
- Installed Windows Server 2022 (Desktop Experience)
- Verified baseline configuration

### Challenges & Fixes
- Encountered PXE over IPv4 boot error  
  - Root cause: incorrect boot order  
  - Resolution: prioritized ISO boot device

### Evidence
![Server Install](Images/phase1/baseline.png)

</details>

---

<details>
<summary><strong>Phase 2 – Server Identity & Networking</strong></summary>

### Objective
Prepare the server for Active Directory by stabilizing identity and networking.

### Implementation Steps
- Renamed server to DC01
- Assigned static IPv4 address
- Configured DNS self-reference
- Verified time synchronization

### Importance
Active Directory relies on stable DNS, IP addressing, and time synchronization.

### Evidence
![Static IP](Images/phase2/02-staticIP.png)
---
![ipconfig](Images/phase2/04-ipconfig.png)
---
![Verifification](Images/phase2/03-staticIP.png)
---
![TimeVerification](Images/phase2/time%20verification.png)

</details>

---

<details>
<summary><strong>Phase 3 – AD DS & DNS Role Installation</strong></summary>

### Objective
Install Active Directory Domain Services and DNS roles in preparation for domain promotion.

### Implementation Steps
- Installed AD DS role
- Installed DNS Server role
- Verified role installation via Server Manager

### Importance
Separating role installation from domain promotion allows administrators to validate system readiness and reduces the risk of configuration errors during forest creation.

### Evidence
![AD DS Installed](Images/phase3/01-role_install.png)
---
![Role Confirmation](Images/phase3/02-role_confirmation.png)

</details>

---

<details>
<summary><strong>Phase 4 – Domain Promotion & Forest Creation</strong></summary>

### Objective
Create a new Active Directory forest and promote the server to a Domain Controller.

### Implementation Steps
- Created a new forest (`NetLabz.local`)
- Promoted DC01 to a Domain Controller
- Installed and verified AD-integrated DNS
- Verified ADUC and DNS post-promotion

### Importance
Forest creation defines the security boundary of an Active Directory environment. Proper domain naming, DNS integration, and functional level selection are critical for long-term stability.

### Evidence
![Installation](Images/phase4/01-domain_promotion.png)
---
![ADUC Console](Images/phase4/02-ADUC.png)
---
![DNS Zone](Images/phase4/03-DNS.png)

</details>

---

<details>
<summary><strong>Phase 5 – Active Directory Users, Groups, and Client Integration</strong></summary>

### Objective
Simulate real-world Active Directory administration by structuring Organizational Units, managing users and groups, and successfully joining a Windows 11 client to the domain.

---

### Implementation Steps
- Created Organizational Units for departmental organization:
  - Research OU
  - Human Resources OU
  - Production OU
- Pre-staged a client computer object in Active Directory Users and Computers
- Created a security group for departmental access control
- Implemented a user template account and created new users by copying the template
- Assigned users to the appropriate security groups
- Deployed a Windows 11 Enterprise Evaluation client VM
- Attempted and completed domain join of the client system

---

### Issue Encountered
During the domain join process, the client system was unable to contact the Domain Controller, resulting in a domain connectivity error.

Initial review suggested the domain name and credentials were correct, and the computer object already existed in Active Directory. However, the join process consistently failed.
---
![error](Images/phase5/troubleshoot1.png)

---

### Analysis & Troubleshooting
Using `ipconfig /all`, I identified multiple network configuration issues on the client system:

- The client IP address was not within the same subnet as the Domain Controller
- The subnet mask did not align with the domain network
- The DNS server configuration did not initially point to the Domain Controller.
  - I forgot to capture the evidence of the DNS server mismatch before changing it to DC01
---
![analyze1](Images/phase5/troubleshoot3.png)

---

These misconfigurations prevented the client from locating the Domain Controller and resolving the domain properly.

---

### Resolution
- Manually configured the client with a static IPv4 address within the same subnet as the Domain Controller
- Corrected the subnet mask to match the domain network
- Set the DNS server to the Domain Controller’s IP address
- Re-attempted the domain join using domain administrator credentials

After correcting the network configuration, the client successfully joined the domain and automatically associated with the pre-staged computer object in Active Directory.
---
![Resolution](Images/phase5/troubleshoot4.png)

---

### Validation
- Client system displays full domain membership (`RES-COMP-01.NetLabz.local`)
- Domain credentials authenticate successfully
- Computer object appears correctly in the designated OU
- User group memberships reflect intended access design

Screenshots included in this phase document the troubleshooting process, configuration changes, and successful domain join.
---
![Verification](Images/phase5/08-confirmation.png)

---

### Key Takeaway
This phase reinforced the dependency Active Directory has on correct DNS and network configuration. Even with properly created OUs, users, and computer objects, domain operations will fail if fundamental networking requirements are not met.

</details>

---

## 🚧 Project Status – Work in Progress

This environment is actively being expanded to reflect additional real-world Active Directory, systems administration, and security scenarios. The foundation is complete, and future phases will build directly on the existing domain, users, groups, and client systems already in place.

Rather than restarting or restructuring the lab, upcoming work will focus on layering functionality, tightening controls, and introducing realistic operational challenges.

---

## Planned Next Phases

### Phase 6 – File Services & Permissions
- Implement file shares based on departmental structure
- Apply NTFS and share permissions using security groups
- Validate access control from domain-joined client systems
- Document permission inheritance and troubleshooting 

---

### Phase 7 – Group Policy Implementation
- Create and link GPOs at the OU level
- Enforce basic security and workstation policies
- Test policy application and results on client machines
- Troubleshoot policy precedence and inheritance

---

### Phase 8 – Authentication, Access, and Hardening
- Review authentication flow (DNS, Kerberos, domain logons)
- Apply baseline hardening where appropriate
- Introduce intentional misconfigurations to test troubleshooting skills
- Document security considerations and lessons learned

---

## 📌 Ongoing Goals
- Continue documenting both successes and failures
- Prioritize realism over perfection
- Maintain clear evidence of configuration, validation, and troubleshooting
- Evolve the lab without unnecessary redesign or scope resets

This project is intended to grow alongside hands-on experience and learning, mirroring how real environments are improved over time rather than built all at once.
