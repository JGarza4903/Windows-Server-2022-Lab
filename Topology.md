# Active Directory Topology Overview

This document provides a snapshot of the current **Active Directory logical topology** for the `NetLabz.local` domain.  

Objects are organized by Organizational Unit (OU) and categorized into **Users, Computers, and Groups** to reflect the present state of the environment.

---

## Domain Controllers OU
**Distinguished Name:**  
`OU=Domain Controllers,DC=NetLabz,DC=local`

### Users
- None
### Computers
- DC01
### Groups
- None

---

## Human Resources OU
**Distinguished Name:**  
`OU=Human Resources OU,DC=NetLabz,DC=local`

### Users
- HR User Template  
- Paige Turner  
- Willie Makit  
### Computers
- None
### Groups
- HR_Administration  
- HR_Employees  

---

## Production OU
**Distinguished Name:**  
`OU=Production OU,DC=NetLabz,DC=local`
### Users
- Billy Bob  
- Janet Jackson  
- Jerry Seinfeld  
- ProductionUser Template  
### Computers
- None
### Groups
- ProductionEmployees  
- ProductionManagers  

---

## Research OU
**Distinguished Name:**  
`OU=Research OU,DC=NetLabz,DC=local`

### Users
- Bill Nye  
- Nikola Tesla  
- RES_User Template  
### Computers
- RES-COMP-01  
- RES-COMP-02  
### Groups
- Research_Employees  
- Research_Managers  

---

## Notes

- User templates are maintained to support **consistent account provisioning**
- Departmental groups align with **role-based access control (RBAC)**
- Research systems are isolated to support **testing and experimentation**
- This topology is designed to support future **Group Policy** and **security baseline enforcement**
