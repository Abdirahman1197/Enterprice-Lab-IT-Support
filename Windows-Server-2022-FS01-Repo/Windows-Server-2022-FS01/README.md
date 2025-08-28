
# Windows Server 2022 File Server Deployment (FS01)

This repository documents the deployment and configuration of a **Windows Server 2022 File Server** (FS01) within the `tayo.com` domain. The server provides shared storage for departments, with security group–based access, Group Policy drive mapping, and Access-Based Enumeration (ABE).

---

## 🧱 Topology & Hostnames
- **Domain Controller (DC):** `DC01-MainServer` (`10.10.10.3`)
- **File Server (FS):** `FS01` (joined to `tayo.com`)
- **Clients:** Windows 10/11 machines joined to the domain
- **Purpose:** Centralized file sharing with security-group–based folder access

---

## 📦 Repository Structure
```
.
├─ docs/            # Additional notes & exports
├─ screenshots/     # Step-by-step visuals
└─ README.md
```

---

## 1) Join Server to Domain
![Step-1-Join-Server-to-Domain.png](screenshots/Step-1-Join-Server-to-Domain.png)

---

## 2) Disk Partitioning for File Share
![Step-2-Partition-Disk-For-Fileshare.png](screenshots/Step-2-Partition-Disk-For-Fileshare.png)
![Step-3-Disk-Name-&-Size.png](screenshots/Step-3-Disk-Name-&-Size.png)

---

## 3) Department Folder Creation
![Step-4-Create-Department-Folders.png](screenshots/Step-4-Create-Department-Folders.png)
![Step-5-Enable-Share.png](screenshots/Step-5-Enable-Share.png)
![Step-6-File-Server-manager-shares.png](screenshots/Step-6-File-Server-manager-shares.png)

---

## 4) Group Policy Drive Mapping
![Step-7-Open-GPO-Create-Map-Drive-Linked-to-FS01-Dept-Folders.png](screenshots/Step-7-Open-GPO-Create-Map-Drive-Linked-to-FS01-Dept-Folders.png)
![Step-8-Link-Security-Groups.png](screenshots/Step-8-Link-Security-Groups.png)
![Step-9-All-Maps.png](screenshots/Step-9-All-Maps.png)

---

## 5) GPO Enforcement & Testing
![Step-10-Link-GPO-to-Users.png](screenshots/Step-10-Link-GPO-to-Users.png)
![Step-11-Force-Update-from-Terminal.png](screenshots/Step-11-Force-Update-from-Terminal.png)
![Step-12-Summary-Of-Policies.png](screenshots/Step-12-Summary-Of-Policies.png)

---

## 6) Access-Based Enumeration (ABE)
![Step-13-ABE.png](screenshots/Step-13-ABE.png)

---

## 7) Verification
![Step-14-File-Exists.png](screenshots/Step-14-File-Exists.png)

---

## 🔍 Notes & Best Practices
- Use **security groups** (not individual users) for share permissions.
- Combine **NTFS permissions** with **share permissions** for layered security.
- Enable **Access-Based Enumeration (ABE)** to improve user experience and security.
- Back up FS01 regularly to protect department data.
- Consider DFS Namespaces & DFS Replication for future scalability and high availability.

---

## 📜 License
This project is provided as-is for educational and lab purposes.
