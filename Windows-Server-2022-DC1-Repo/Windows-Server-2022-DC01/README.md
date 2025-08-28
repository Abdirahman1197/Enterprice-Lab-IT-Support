
# Windows Server 2022 Domain Controller Deployment (DC01)

This repository documents the end-to-end process of deploying and configuring a new **Windows Server 2022** Domain Controller for the **`tayo.com`** forest. The project covers virtual machine creation, OS installation, role configuration (Active Directory, DNS, DHCP), security hardening, user management, backup, and Group Policy implementation. Every step is illustrated with screenshots and includes a PowerShell firewall-hardening script.

---

## 🧱 Topology & Hostnames
- **Domain / Forest:** `tayo.com`
- **Domain Controller (DC):** `DC01-MainServer`
- **IP (example):** `10.10.10.3`  
  *Note:* Use a subnet mask that matches your lab network (e.g., `255.255.255.0` for `10.10.10.0/24`).  
- **Gateway (example):** `10.10.10.1`
- **Client (example):** Joined to domain `TAYO` for sign-in and policy testing.

---

## 📦 Repository Structure
```
.
├─ docs/                         # Additional notes & exports
├─ scripts/
│  └─ dc01-firewall-hardening.ps1
├─ screenshots/                  # Step-by-step visuals used in this README
└─ README.md
```

---

## ✅ Prerequisites
- VMware Workstation (or any hypervisor)
- Windows Server 2022 ISO (Standard Evaluation w/ Desktop Experience)
- Host machine with virtualization support
- Basic understanding of AD DS, DNS, DHCP

---

## 1) Introduction
The goal is to build a production-ready domain controller named **`DC01-MainServer`** within the **`tayo.com`** domain. The process, from fresh VM to a working DC with Group Policies and a backup, is documented step by step.

---

## 2) Virtual Machine & OS Installation
- **Step 1–3:** Create a new VM, choose Windows Server 2022, and name it `Server-2022-Main`  
- **Step 4:** Allocate a 60 GB virtual disk (stored as multiple files)  
- **Step 5:** Set the Network Adapter to a **Custom** virtual network (lab isolation)  
- **Step 6–9:** Install Windows Server 2022 Standard Evaluation (Desktop Experience)

---

## 3) Initial Server Configuration
- **Step 10:** Set a strong password for the built-in `Administrator`  
- **Step 11:** Enable **Network discovery**  
- **Step 12:** Install **VMware Tools** (drivers & integration)  
- **Step 13:** Verify initial IP configuration with `ipconfig`

---

## 4) Active Directory & DNS
- **Step 14 & 18:** Add the **AD DS** role and **promote to Domain Controller**
- **Step 15:** Create a new **forest** with root domain **`tayo.com`**
- **Step 16:** Set forest & domain functional levels (e.g., Windows Server 2016). Configure **DSRM** password.
- **Step 17:** Confirm **prerequisite checks**

---

## 5) Network Services: Static IP & DHCP
- **Step 21:** Configure a **static IPv4** (e.g., `10.10.10.3`), DNS to self, and gateway to your router
- **Step 22–23:** Add the **DHCP Server** role
- **Step 24:** **Authorize** the DHCP server in AD with `TAYO\Administrator`

---

## 6) Security Hardening (PowerShell)
A focused firewall-hardening script is provided at `scripts/dc01-firewall-hardening.ps1`:

```powershell
# Run in an elevated PowerShell session on DC01
Set-ExecutionPolicy RemoteSigned -Scope Process -Force
.\scripts\dc01-firewall-hardening.ps1
```

**What it does:**
- Disables consumer-grade services (mDNS, Cast to Device)
- Removes unnecessary rules (BranchCache, Media Player, UPnP)
- Ensures core infra (AD, DNS, DHCP, RDP) rules are enabled
- Restricts critical rules to **Domain** profile only

---

## 7) Server Backup & File Sharing
- **Step 25–26:** Enable a VMware **Shared Folder** for host ↔ VM file transfer
- **Step 27:** Add **Windows Server Backup**
- **Step 28–30:** Run a **Full Server** one-time backup to a separate volume

---

## 8) User & Client Management
- **Step 31:** Create a user **Jonas Berg** (`jberg@tayo.com`) and require password change
- **Step 32–35:** Join a client to **TAYO** and sign in; user changes password at first logon
- **Step 36:** Use `nslookup` on the client to verify **`DC01.tayo.com`** is the DNS server

---

## 9) Group Policy Management (GPO)
- **Step 37:** Create a share `\\DC01\Wallpaper` for the corporate background
- **Step 38:** Create a GPO **“Wallpaper”** and **link** it to the domain
- **Step 39:** Configure the policy to set the wallpaper from the network share
- **Step 40:** On a client, run `gpupdate /force`
- **Step 41:** Confirm the wallpaper is applied

---

## 🔍 Notes & Tips
- If your lab uses `10.10.10.0/24`, set **Subnet Mask** to `255.255.255.0`.
- Set the client’s **DNS** to the DC’s IP (e.g., `10.10.10.3`) to ensure domain join and GPOs work.
- After joining the domain, consider creating OUs and moving computers/users into them before linking GPOs.

---

## 📜 License
This project is provided as-is for educational and lab purposes.
