# Server Baseline Configuration  
*Preparing Windows Server 2022 for AD DS Installation*

---

## Objective
Configure server identity, static IP, and DNS settings to establish a stable foundation for Active Directory.

---

## Steps Performed
1. Opened **Server Manager → Local Server → WIN-DBEUT1M6PLA and renamed the server to `DC01`.  
2. Set **static IPv4 address**:
 
IP address: 192.168.20.5
Subnet mask: 255.255.255.0
Default gateway: (none)
Preferred DNS: 127.0.0.1

3. Rebooted to apply changes.  
4. Verified new settings with PowerShell commands.
5. Verified configuration by running `ipconfig /all`.  
- Captured screenshots of NIC properties and Server Manager overview.

---

### 🧠 What I Learned  
- **Static IPs are non-negotiable** for servers — DHCP changes can break DNS registration and AD functionality.  
- **Self-referencing DNS** (127.0.0.1) ensures the DC can later resolve its own domain records.  
- Even small things like **time zone mismatch** can cause replication or Kerberos authentication errors later.
