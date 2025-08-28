
# pfSense Router & Firewall Deployment

This repository documents the deployment and configuration of **pfSense** as the primary router/firewall in the `tayo.com` lab environment. pfSense provides secure routing, DHCP services, and web-based management for the virtual network.

---

## 🧱 Topology
- **Firewall/Router:** pfSense VM (WAN ↔ LAN bridge between host and lab network)
- **Domain Controller (DC01):** `10.10.10.3`
- **File Server (FS01):** `10.10.10.5`
- **Clients:** Windows 10/11 joined to the `tayo.com` domain
- **Purpose:** Act as a firewall, DHCP/DNS forwarder, and gateway for lab services

---

## 📦 Repository Structure
```
.
├─ docs/            # Additional notes
├─ screenshots/     # Step-by-step pfSense setup visuals
└─ README.md
```

---

## 1) Virtual Machine Setup
- **Step 01:** Select pfSense ISO in VMware VM wizard  
- **Step 02:** Configure VM hardware, especially network adapters (WAN + LAN)
![01_vm_wizard_select_pfsense_iso.png](screenshots/01_vm_wizard_select_pfsense_iso.png)
![02_vm_hardware_configure_network_adapter.png](screenshots/02_vm_hardware_configure_network_adapter.png)

---

## 2) pfSense Installation
- **Step 03–04:** Start installer and proceed through copyright & welcome screens  
- **Step 05:** Configure LAN IP (e.g., `10.10.10.1/24`)  
- **Step 06–07:** Configure DHCP start and end range  
![03_pfsense_installer_copyright_notice.png](screenshots/03_pfsense_installer_copyright_notice.png)
![04_pfsense_installer_welcome_screen.png](screenshots/04_pfsense_installer_welcome_screen.png)
![05_pfsense_setup_lan_ip_address.png](screenshots/05_pfsense_setup_lan_ip_address.png)
![06_pfsense_setup_dhcp_start_range.png](screenshots/06_pfsense_setup_dhcp_start_range.png)
![07_pfsense_setup_dhcp_end_range.png](screenshots/07_pfsense_setup_dhcp_end_range.png)

---

## 3) Post-Install Configuration
- **Step 08:** Complete setup and validate subscription check  
- **Step 09:** Review post-install details  
- **Step 10:** Access pfSense console main menu  
![08_pfsense_setup_subscription_validation.png](screenshots/08_pfsense_setup_subscription_validation.png)
![09_pfsense_post_install_details.png](screenshots/09_pfsense_post_install_details.png)
![10_pfsense_console_main_menu.png](screenshots/10_pfsense_console_main_menu.png)

---

## 4) Networking Verification
- **Step 11:** Verify VMware Virtual Network Editor for NIC assignments  
![11_vm_virtual_network_editor_overview.png](screenshots/11_vm_virtual_network_editor_overview.png)

---

## 5) Web Interface
- **Step 12:** Access pfSense Web UI (`https://10.10.10.1`)  
- **Step 13:** Confirm dashboard overview and firewall status  
![12_pfsense_web_login_page.png](screenshots/12_pfsense_web_login_page.png)
![13_pfsense_dashboard_overview.png](screenshots/13_pfsense_dashboard_overview.png)

---

## 🔍 Notes & Best Practices
- Assign **WAN** to bridged adapter (host’s internet) and **LAN** to host-only adapter (lab network).
- Change default **admin** credentials immediately after setup.
- Use **DNS Forwarder/Resolver** to integrate with DC01’s DNS.
- Always back up pfSense configuration (`Diagnostics → Backup & Restore`).
- Consider enabling **pfBlockerNG** for ad/malware blocking in the lab.

---

## 📜 License
This project is provided as-is for educational and lab purposes.
