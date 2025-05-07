# Vulnerable Active Directory Environment Setup on Windows Server 2019 Using Vulnerable Script
## Introduction
This project demonstrates how to build a vulnerable Active Directory (AD) environment from scratch on Windows Server 2019, using publicly available scripts to simulate common misconfigurations and security flaws. This lab is specifically designed for penetration testers, red teamers, and cybersecurity learners who want a realistic, hands-on playground to explore AD attack paths and defenses.

By automating misconfigurations with a vulnerable script, the environment closely mirrors real-world insecure AD deployments often encountered in enterprise networks. This includes insecure permissions, poorly configured Group Policies, weak service accounts, and exploitable trusts.

The main objectives of this lab are to:

 - Set up a fully functional Windows domain controller

 - Deploy a vulnerable AD setup using a misconfiguration script

 - Practice attacks such as Kerberoasting, Pass-the-Hash, AS-REP Roasting, ACL abuse, and more

 - Understand how attackers exploit weak AD environments and how defenders can secure them

---

## 🔐 Main Features

- Preloaded with common AD vulnerabilities
- Supports various real-world attack techniques
- Requires AD-installed Domain Controller (DC)
- Some attacks need a connected client workstation

---

## 🎯 Supported Attacks

- Abusing ACLs/ACEs  
- Kerberoasting  
- AS-REP Roasting  
- DNSAdmins abuse  
- Password stored in user comment  
- Password Spraying  
- DCSync attack  
- Silver Ticket  
- Golden Ticket  
- Pass-the-Hash  
- Pass-the-Ticket  
- SMB Signing Disabled  
- Misconfigured WinRM permissions  
- Anonymous LDAP query  
- Public SMB Share  
- Zerologon (requires vulnerable version)

---

## 🛠️ Step-by-Step Setup Guide

> ⚠️ Replace `change.me` with your domain name as required.

### ✅ Step 1: Bypass PowerShell Execution Policy

Allows execution of scripts on the system.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Bypass
```
### ✅ Step 2: Import AD Deployment Module

Loads the necessary module to install Active Directory.

```powershell
Import-Module ADDSDeployment
```

### ✅ Step 3: Install Active Directory Domain Services (AD DS)

> This command installs a new forest and domain controller.  
> Replace `change.me` with your domain

```powershell
# if you didn't install Active Directory yet , you can try 
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\\Windows\\NTDS" -DomainMode "7" -DomainName "change.me" -DomainNetbiosName "change" -ForestMode "7" -InstallDns:$true -LogPath "C:\\Windows\\NTDS" -NoRebootOnCompletion:$false -SysvolPath "C:\\Windows\\SYSVOL" -Force:$true
# if you already installed Active Directory, just run the script !
IEX((new-object net.webclient).downloadstring("https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1"));
Invoke-VulnAD -UsersLimit 100 -DomainName "change.me"
```

