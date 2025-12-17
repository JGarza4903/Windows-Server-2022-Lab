# Phase 2 – Server Identity & Network Configuration

## Project Overview
This phase documents the preparation steps required before promoting a Windows Server 2022 system to a Domain Controller. The focus is on stabilizing the server’s identity, network configuration, and time settings to ensure Active Directory and DNS function reliably.

These steps reflect real-world best practices used in enterprise environments to prevent authentication, name resolution, and replication issues later in the deployment lifecycle.

---

## Objectives
- Establish a permanent server identity
- Configure a static network configuration
- Verify time and time zone accuracy
- Prepare the system for Active Directory Domain Services (AD DS)

---

## Actions Performed

### Server Rename
- Renamed the server from the default system-generated name to **DC01**
- Restarted the system to apply the change

Renaming the server prior to domain promotion prevents Active Directory metadata inconsistencies and avoids the need for post-promotion cleanup.

---

### Static IP Configuration
Configured the primary network adapter with a static IPv4 address to ensure consistent network identity.

- **IP Address:** `192.168.50.3`
- **Subnet Mask:** `255.255.255.0`
- **DNS Server:** `127.0.0.1` (self-referencing)
- **DHCP:** Disabled

The server was configured to use itself as the DNS server in preparation for hosting Active Directory–integrated DNS zones.

---

### Network Verification
- Verified configuration using `ipconfig /all`
- Confirmed:
  - Static IP assignment
  - DNS self-reference
  - Correct hostname (`DC01`)
  - IPv4 connectivity

---

### Time & Time Zone Verification
- Confirmed system time accuracy
- Verified correct time zone:
  - **(UTC-08:00) Pacific Time (US & Canada)**
- Confirmed successful time synchronization

Accurate timekeeping is critical for Kerberos authentication, which is used by Active Directory for secure logons.

---

## Troubleshooting & Considerations

- Leaving a server on DHCP prior to domain promotion can cause IP changes that break DNS records and authentication.
- Using external DNS servers before AD DS installation can prevent proper registration of SRV records.
- Time drift between domain members can result in Kerberos authentication failures.

All configurations were verified before proceeding to role installation.

---

## Skills Demonstrated
- Windows Server 2022 administration
- Server identity management
- Static IPv4 network configuration
- DNS planning for Active Directory
- Command-line verification (`ipconfig`)
- Enterprise documentation practices

---

## Evidence
Screenshots validating each configuration step are stored in:

Images/
└─ phase2/
├─ 01-server-rename.png
├─ 02-static-ip-config.png
├─ 03-ipconfig-verification.png
├─ 04-time-verification.png

---

## What I Learned
Configuring a static IP address and DNS self-reference before domain promotion is critical to Active Directory stability. Performing these steps early prevents DNS registration issues and reduces troubleshooting complexity later in the deployment.
