# dc01-firewall-hardening.ps1
# Purpose: Harden Windows Server 2022 Domain Controller firewall rules for AD DS, DNS, DHCP, Kerberos
# Author: A
# Date: 2025-08-19

# --- Trim noise / consumer services ---
Disable-NetFirewallRule -DisplayGroup "mDNS" -ErrorAction SilentlyContinue
Disable-NetFirewallRule -DisplayGroup "Cast to Device functionality" -ErrorAction SilentlyContinue
Disable-NetFirewallRule -DisplayGroup "Network Discovery" -ErrorAction SilentlyContinue

# BranchCache cleanup
Get-NetFirewallRule | Where-Object {$_.DisplayGroup -like "BranchCache*"} | Disable-NetFirewallRule

# Optional: Disable Media/UPnP clutter often enabled by default
Get-NetFirewallRule | Where-Object {
    $_.DisplayGroup -like "Windows Media Player*" -or
    $_.DisplayGroup -like "Microsoft Media Foundation*" -or
    $_.DisplayName -like "*SSDP*" -or
    $_.DisplayName -like "*UPnP*" -or
    $_.DisplayName -like "*qWave*"
} | Disable-NetFirewallRule

# --- Ensure core infra rules are enabled ---
Enable-NetFirewallRule -DisplayGroup "Active Directory Domain Services"
Enable-NetFirewallRule -DisplayGroup "DNS Service"
Enable-NetFirewallRule -DisplayGroup "DHCP Server"
Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

# --- Lock infra rules to Domain profile only (reduces exposure) ---
$infra = @(
    "DNS (UDP, Incoming)","DNS (TCP, Incoming)",
    "RPC (TCP, Incoming)","RPC Endpoint Mapper (TCP, Incoming)",
    "Active Directory Domain Controller - LDAP (TCP-In)",
    "Active Directory Domain Controller - LDAP (UDP-In)",
    "Active Directory Domain Controller - Secure LDAP (TCP-In)",
    "Active Directory Domain Controller - LDAP for Global Catalog (TCP-In)",
    "Active Directory Domain Controller - Secure LDAP for Global Catalog (TCP-In)",
    "Kerberos Key Distribution Center (TCP-In)",
    "Kerberos Key Distribution Center (UDP-In)",
    "DHCP Server v4 (UDP-In)","DHCP Server v6 (UDP-In)"
)
$infra | ForEach-Object { Set-NetFirewallRule -DisplayName $_ -Profile Domain -ErrorAction SilentlyContinue }

# Keep RDP accessible on Domain profile only
Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Profile Domain -ErrorAction SilentlyContinue

# --- Verification commands ---
Write-Output "DNS Rules:"
Get-NetFirewallRule | Where-Object {$_.DisplayGroup -eq "DNS Service"} | Select DisplayName,Enabled,Profile

Write-Output "DHCP Rules:"
Get-NetFirewallRule | Where-Object {$_.DisplayGroup -like "DHCP Server*"} | Select DisplayName,Enabled,Profile

Write-Output "Active Directory Domain Services Rules:"
Get-NetFirewallRule | Where-Object {$_.DisplayGroup -eq "Active Directory Domain Services"} | Select DisplayName,Enabled,Profile
