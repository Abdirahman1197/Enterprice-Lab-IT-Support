
# Windows Server 2022 Domain Controller Deployment (DC01)

This repository documents the end-to-end process of deploying and configuring a new **Windows Server 2022** Domain Controller for the `tayo.com` domain. It includes VM creation, OS installation, Active Directory, DNS, DHCP, Group Policy, user management, backups, and security hardening via PowerShell.

---

## 🧱 Topology
- **Domain / Forest:** `tayo.com`
- **Domain Controller (DC):** `DC01-MainServer` (IP: `10.10.10.3`)
- **Gateway:** `10.10.10.1`
- **Clients:** Windows 10/11 joined to domain
- **Purpose:** Centralized authentication, DNS, DHCP, and policy enforcement

---

## 📦 Repository Structure
```
.
├─ docs/            
├─ scripts/
│  └─ dc01-firewall-hardening.ps1
├─ screenshots/
└─ README.md
```

---

## 1) Virtual Machine & OS Installation
- Create VM, choose Windows Server 2022, allocate disk, configure network.
![Step-1-Choose-how-to-install-vm.png](screenshots/Step-1-Choose-how-to-install-vm.png)
![Step-2-Choose-Operating-Sytem.png](screenshots/Step-2-Choose-Operating-Sytem.png)
![Step-3-VM-Name.png](screenshots/Step-3-VM-Name.png)
![Step-4-Specify-vm-size.png](screenshots/Step-4-Specify-vm-size.png)
![Step-5-Network-Adapter.png](screenshots/Step-5-Network-Adapter.png)
![Step-6-Installation-Language.png](screenshots/Step-6-Installation-Language.png)
![Step-7-OS.png](screenshots/Step-7-OS.png)
![Step-8-Custom-Installation.png](screenshots/Step-8-Custom-Installation.png)

---

## 2) Initial Server Configuration
- Set Administrator password, enable network discovery, install VMware Tools, verify IP.
![Step-10-Set-Admin-Password.png](screenshots/Step-10-Set-Admin-Password.png)
![Step-11-Set-dicoverability.png](screenshots/Step-11-Set-dicoverability.png)
![Step-12-Install-vmware-tools.png](screenshots/Step-12-Install-vmware-tools.png)

---

## 3) Active Directory & DNS
- Add AD DS role, create new forest `tayo.com`, set DSRM, promote to DC.
![Step-14-Add-AD-Role.png](screenshots/Step-14-Add-AD-Role.png)
![Step-15-Root-Domain-Name.png](screenshots/Step-15-Root-Domain-Name.png)
![Step-16-DSRM-Pass.png](screenshots/Step-16-DSRM-Pass.png)
![Step-17-Install-AD-Role.png](screenshots/Step-17-Install-AD-Role.png)
![Step-18-Promote-Server.png](screenshots/Step-18-Promote-Server.png)
![Step-19-Change-Computer-Name.png](screenshots/Step-19-Change-Computer-Name.png)

---

## 4) Static IP & DHCP Setup
- Set static IPv4 (`10.10.10.3/24`), add DHCP role, authorize DHCP in AD.
![Step-21-Set-IPV4-Manually.png](screenshots/Step-21-Set-IPV4-Manually.png)
![Step-22-DHCP-Server-Role.png](screenshots/Step-22-DHCP-Server-Role.png)
![Step-23-Install-DHCP-Server.png](screenshots/Step-23-Install-DHCP-Server.png)

---

## 5) Backup & File Sharing
- Enable shared folder, add Windows Server Backup, run full server backup.
![Step-25-Enable-Sharedfile-Vmware.png](screenshots/Step-25-Enable-Sharedfile-Vmware.png)
![Step-26-Confirm-file-in-DC01.png](screenshots/Step-26-Confirm-file-in-DC01.png)
![Step-27-Add-WSB.png](screenshots/Step-27-Add-WSB.png)
![Step-28-Full-server-Backup.png](screenshots/Step-28-Full-server-Backup.png)
![Step-29-Select-Backup-Destination.png](screenshots/Step-29-Select-Backup-Destination.png)

---

## 6) User & Client Management
- Create user, join client to domain, confirm DNS, first logon password change.
![Step-31-Overview.User-Account.png](screenshots/Step-31-Overview.User-Account.png)
![Step-32-Jon-Client-to-Domain.png](screenshots/Step-32-Jon-Client-to-Domain.png)
![Step-33-Password-Change-Succesful.png](screenshots/Step-33-Password-Change-Succesful.png)
![Step-34-Confirm-User.png](screenshots/Step-34-Confirm-User.png)
![Step-35-Promted to-change-Password.png](screenshots/Step-35-Promted to-change-Password.png)

---

## 7) Group Policy Management
- Create wallpaper share, GPO to set wallpaper, link to domain, enforce via gpupdate.
![Step-37-Wallpaper-Folder.png](screenshots/Step-37-Wallpaper-Folder.png)
![Step-38-Create-Wallpaper-GPO.png](screenshots/Step-38-Create-Wallpaper-GPO.png)
![Step-39-Set-ShareFolder-Path.png](screenshots/Step-39-Set-ShareFolder-Path.png)
![Step-40-Enforce-GPO.png](screenshots/Step-40-Enforce-GPO.png)

---

## 🔒 Security Hardening (PowerShell)
Script: `scripts/dc01-firewall-hardening.ps1`

```powershell
# Run in elevated PowerShell on DC01
Set-ExecutionPolicy RemoteSigned -Scope Process -Force
.\scripts\dc01-firewall-hardening.ps1
```

**Actions performed:**
- Disable consumer-grade services (mDNS, Cast to Device)
- Remove unnecessary rules (BranchCache, Media Player, UPnP)
- Ensure AD, DNS, DHCP, and RDP rules are enabled
- Restrict rules to the **Domain** profile only

---

## 🔍 Notes & Tips
- Point clients’ **DNS** to the DC’s IP (10.10.10.3) for domain join & GPOs.
- Place users and computers in OUs before linking GPOs.
- Always back up system state and AD regularly.

---

## 📜 License
This project is provided as-is for educational and lab purposes.
