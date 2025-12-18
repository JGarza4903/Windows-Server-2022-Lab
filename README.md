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

### Actions Taken
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

### Actions Taken
- Renamed server to DC01
- Assigned static IPv4 address
- Configured DNS self-reference
- Verified time synchronization

### Why This Matters
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

### Actions Taken
- Installed AD DS role
- Installed DNS Server role
- Verified role installation via Server Manager

### Why This Matters
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

### Actions Taken
- Created a new forest (`NetLabz.local`)
- Promoted DC01 to a Domain Controller
- Installed and verified AD-integrated DNS
- Verified ADUC and DNS post-promotion

### Why This Matters
Forest creation defines the security boundary of an Active Directory environment. Proper domain naming, DNS integration, and functional level selection are critical for long-term stability.

### Evidence
![Installation](Images/phase4/01-domain_promotion.png)
---
![ADUC Console](Images/phase4/02-ADUC.png)
---
![DNS Zone](Images/phase4/03-DNS.png)

</details>
