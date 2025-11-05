# Lab Journal — Setup Overview
*Windows Server 2022 · Enterprise AD DS (GUI-First)*

---

## Session Overview
**Date:** [2025-11-04]  
**Goal:** Establish a clean Windows Server 2022 base for AD DS (no domain yet).  
**Scope:** Document baseline state, create project folders, and prep for static IP + role installation.

---

## Environment Snapshot
- **Hypervisor:** Oracle VirtualBox   
- **VM Name:** DC01  
- **OS Build:** Windows Server 2022 Standard Edition  
- **vCPU / RAM / Disk:** [e.g., 2 vCPU / 6 GB RAM / 60 GB VHDX]  
- **Network Mode:** Isolated  

> Save a VM snapshot before any major configuration step to document the process.

---

## Project Folder Layout
Create a local working directory to organize your evidence:

Windows-Server-2022-Lab
├─ Docs
  |_Procedural Steps
├─ Images
  |_Procedural Images
└─ README.md

---

## Actions Performed (Baseline)
1. Verified that the initial boot leads to **Server Manager** with no roles installed.  
2. Set the correct **time zone** and verified **Windows Updates** were current.  
3. Created project folders listed above.  
4. Screenshot of steps along the way.
