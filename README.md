# Enterprise Active Directory Lab  

*Windows Server 2022*

---

## Snapshot  

## Snapshot  
| Item | Summary |
|------|----------|
| **Domain** | `JGCyber.net` |
| **Server** | DC01 – Windows Server 2022 |
| **Focus** | GUI-based Active Directory configuration |
| **Result** | Fully functional domain controller with DNS, OUs, users, and GPOs |
| **Tools** | Server Manager, ADUC, ADAC, DNS Manager, GPMC |

---


## Overview  

This lab demonstrates the setup of an enterprise-style Active Directory domain in **Windows Server 2022**, using only GUI tools.  

The objective was to learn the relationships between **DNS, OUs, user management, and Group Policy** before automating these processes with PowerShell in later projects.  

Each configuration step was verified with screenshots and summarized below.

---

## Build Summary  


| Phase | Task | Outcome |
|-------|------|----------|
| 1 | Server prep and baseline configuration | Clean Windows Server 2022 install documented |
| 2 | Static IP and rename | Configured `192.168.20.5` and renamed host to `DC01` |
| 3 | Installed AD DS, DNS, GPMC | Core roles installed and verified via Server Manager |
| 4 | Promoted to Domain Controller | Created new forest `JGCyber.net` |
| 5 | Verified AD DS \& DNS | Confirmed services and DNS zones functioning |
| 6 | Built OU structure | Created departments and resource OUs |
| 7 | Added users \& groups | Created template and security groups in Sales OU |
| 8 | Configured GPO baseline | Enforced password and lockout policies |
| 9 | Domain-joined client | Validated login and policy application |

---

## Highlights  
- `assets/images/01-server-baseline.png` – Server setup  
- `assets/images/03-domain-promotion.png` – Domain promotion summary  
- `assets/images/05-ou-structure.png` – Organizational Units layout  
- `assets/images/06-users-groups.png` – User and group configuration  
- `assets/images/07-gpo-baseline.png` – GPO configuration view  

*(All evidence available in the `Images/` folder.)*

---

## Key Takeaways  

- Learned how **DNS and AD DS** rely on each other for domain functionality.  
- Practiced **OU design and group-based role assignment** using AGDLP principles.  
- Gained foundational understanding of **Group Policy management** and inheritance.  
- Reinforced the importance of **clean documentation** and verification.  

---

## Next Steps  

- Rebuild this environment entirely through **PowerShell automation**.  
- Add a **secondary DC** and **pfSense firewall** for multi-network simulation.  
- Integrate **AD CS (Certificate Services)** and security auditing.

