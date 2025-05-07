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

Make sure to turn off the realtime protection and tamper protection for both your Host machine and VM

Download the required tools and operating system to begin your setup:

- **Windows Server 2019 ISO**  
  [Download from Microsoft Evaluation Center](https://www.microsoft.com/en-in/evalcenter/evaluate-windows-server-2019)

![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/server%20page%20iso.png?raw=true)

- **PowerView (for AD recon and abuse)**  
[https://github.com/vishalbagwan0/Vulnerable-AD](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/PowerView.ps1)
- **Vulnerable AD Script**  
https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/vulnadscript.ps1


## 🛠️ Step-by-Step Setup Guide

> ⚠️ Replace `change.me` with your domain name as required.

### ✅ Step 1: Bypass PowerShell Execution Policy

Allows execution of scripts on the system.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Bypass
```

![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/1.png?raw=true)

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

```

![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/71.png?raw=true)

---

### ✅ Step 4: Post-Restart – Bypass Policy Again

After the system restarts following Active Directory installation, log in as Administrator and re-apply the PowerShell execution policy to allow script execution.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Bypass
```
---

### ✅ Step 5: Import Vulnerable AD Script

> Run this in **PowerShell as Administrator**.  
> Ensure the `vulnadscript.ps1` file is in your working directory or provide the full path to the script.

```powershell
Import-Module C:\User\Administartor\Desktop\vulnadscript.ps1
```

![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/10.png?raw=true)

---

## 🧭 Windows Enumeration Using PowerView

Once your vulnerable AD environment is ready, you can begin enumeration to identify misconfigurations and gather valuable recon data. PowerView is one of the most powerful tools for AD recon.

---

### 🔍 Step 1: Import PowerView in a PowerShell Session

Open PowerShell **as Administrator** and import the PowerView script.

```powershell
#import PowerView module
Import-Module .\PowerView.ps1
#to  Enumerate Domain users
Get-Domainuser
```

![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/14.png?raw=true)

📝 Make sure PowerView.ps1 is in your current directory or provide the full path to the script.

### Users Enumeration

- **With PowerView**:
```powershell
# Get the list of users
Get-NetUser                       
# Grab the cn (common-name) from the list of users
Get-NetUser | select cn                           
# Grab the name from the list of users
Get-NetUser | select name           
```
### Domain Admins Enumeration

- **With PowerView:**
```powershell
# Get the current domain
Get-NetDomain
# Get items from another domain
Get-NetDomain -Domain change.me
# Get the domain SID for the current domain
Get-DomainSID                                         
# Get domai n policy for current domain
Get-DomainPolicy 
```
- You use the Powerview_enumeration_commands.txt for more enumeration commands

# We can also use Linux for enumeration

Linux provides powerful tools like `nmap` to scan for open ports, detect services, and identify vulnerabilities on a target system. Below are some basic `nmap` commands that can be used for enumeration:

```bash
nmap <Server_IP>
# Scan all 65535 ports on the target
nmap -p- <Server_IP>
```
![Image Description](https://github.com/vishalbagwan0/Vulnerable-AD/blob/main/Image/17.png?raw=true)

# All Done! 🎉

Your lab is now set up and ready to perform all the mentioned attacks. 💻🔓

## Next, we will continue with some well-known attacks to explore and test in this environment. 🚀

Stay connected for more updates and tutorials! 🔥📡


