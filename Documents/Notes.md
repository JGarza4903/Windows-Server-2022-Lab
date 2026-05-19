# Personal Lab Notes – Windows Server 2022 Domain Lab

These are informal notes meant to capture what I was thinking, what confused me at first, and what eventually clicked while building this lab.

---

## Phase 1 — Lab Setup / Install

### Ideas / What Clicked
- I keep forgetting how often the “simple” stuff breaks first (ISO files, boot order, firmware).
- PXE over IPv4 isn’t some advanced networking issue — it’s basically the VM saying it can’t find a bootable disk.
- Using an expired or bad ISO can waste hours troubleshooting the wrong thing.

### What I Did (Quick)
- Downloaded a fresh Windows Server 2022 evaluation ISO
- Created a Generation 2 VM in Hyper-V
- Installed Windows Server 2022 Standard (Desktop Experience)
- Created local administrator credentials

### Checklist
- [ ] VM is Generation 2  
- [ ] ISO attached to DVD drive  
- [ ] Boot order prioritizes ISO  
- [ ] Installation completes successfully  
- [ ] Server Manager opens with no roles installed  
- [ ] Baseline screenshots and `ipconfig` captured  

### Next Step
Move to Phase 2: rename the server and configure networking before touching Active Directory.

---

## Phase 2 — Server Rename, Static IP, and Time

### Ideas / What Clicked
- Renaming the server and setting a static IP is not optional — it’s foundational.
- Active Directory and DNS do not handle surprise changes well.
- Time matters more than I expected because Kerberos is very strict.

### What I Did (Quick)
- Renamed the server to `DC01`
- Assigned a static IPv4 address
- Configured DNS to point to itself
- Verified time zone and system time

### Checklist
- [ ] System name shows `DC01`  
- [ ] `ipconfig /all` confirms static IP  
- [ ] DNS is self-referencing  
- [ ] Correct time zone set  
- [ ] Screenshots saved under `Images/phase2/`  

### Next Step
Phase 3: install AD DS and DNS roles (do not promote yet).

---

## Phase 3 — AD DS & DNS Role Installation (No Promotion)

### Ideas / What Clicked
- Installing Active Directory roles is not the same as having a domain.
- This phase is basically installing the tools before building the structure.
- Pausing here feels slow, but it’s actually a clean checkpoint.

### What I Did (Quick)
- Installed the Active Directory Domain Services role
- Installed the DNS Server role
- Verified both roles were installed successfully

### Checklist
- [ ] AD DS role installed  
- [ ] DNS Server role installed  
- [ ] Server Manager shows promotion warning (expected)  
- [ ] Installation completion screenshot captured  

### Next Step
Phase 4: promote the server and create the forest.

---

## Phase 4 — Domain Promotion & Forest Creation

### Ideas / What Clicked
- This phase is permanent — naming and structure decisions matter.
- Forest vs domain was confusing at first; forest is the security boundary.
- The DNS delegation warning looks scary but is expected in a new forest.

### What I Did (Quick)
- Promoted `DC01` to a Domain Controller
- Created a new forest with an internal lab domain
- Integrated DNS with Active Directory
- Rebooted and verified services

### Checklist
- [ ] Domain login format works (`DOMAIN\\Administrator`)  
- [ ] Active Directory Users and Computers opens  
- [ ] DNS Manager shows forward lookup zone  
- [ ] SYSVOL and NETLOGON shares exist  
- [ ] Screenshots saved in `Images/phase4/`  

### Next Step
Phase 5: create OUs, users, groups, and start simulating real IT work.

---

## Patterns I’m Noticing

- If I skip documentation, I end up restarting work later.
- Most issues come from configuration order, not advanced problems.
- Breaking work into phases makes troubleshooting much easier.

---

## Future Phases (Rough Plan)
--NEW--05/19/206--


**Planned Tasks:**
- Create and link GPOs at the OU level
- Enforce password policies, screen lock, and account lockout thresholds
- Restrict access to Control Panel and removable media for standard users
- Test GPO inheritance, precedence, and filtering
- Use `gpresult` and `rsop.msc` to validate applied policies
- Document policy conflicts and resolution steps

> **Steps to Implement:**
> 1. Open **Group Policy Management** (`gpmc.msc`) on DC01
> 2. Right-click an OU (e.g., `Human Resources OU`) → **Create a GPO in this domain and link it here**
> 3. Name it (e.g., `HR_SecurityBaseline`)
> 4. Right-click the new GPO → **Edit**
> 5. Navigate to `Computer Configuration > Policies > Windows Settings > Security Settings` to configure account and audit policies
> 6. On a domain client, run `gpupdate /force` then `gpresult /r` to verify the policy was applied

--OLD --
### Phase 5 — Active Directory Structure & Users
- Create OUs (Servers, Workstations, Users, Groups)
- Create test users and security groups
- Apply a basic password policy
- Join a Windows client to the domain

### Phase 6 — Day-to-Day Admin Tasks
- NTFS and share permissions
- Group-based access control
- Drive mapping via GPO
- Basic onboarding/offboarding workflow

### Phase 7 — Troubleshooting & Hardening
- DNS troubleshooting scenarios
- Event Viewer log analysis
- Backup and snapshot strategy
