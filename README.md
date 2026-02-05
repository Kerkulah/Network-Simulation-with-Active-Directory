<h1> Network Simulation with Active Directory
</h1>



<h2>Description</h2>
This project focuses on setting up a Windows Server 2022 Domain Controller and a Windows 10 Client Machine in a virtualized environment. The following steps outline the process.
<br />
<h2> Utilities Used</h2>
Oracle VirtualBox      Link https://www.virtualbox.org/wiki/Downloads
Windows 10 ISO         Link: https://www.microsoft.com/en-us/software-download/windows10
Windows Server 2022    Link: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
<h2>Project walk-through:</h2>

<p align="center">
Create the Domain Controller VM (Windows Server 2022): <br/>

Open VirtualBox and click New  → || Name the VM  and choose: (Type: Windows) → 
(Version: Windows Server 2022)  || → 
|| Click Settings → Network and set
Adapter 1: NAT (for internet access ) → 
Enable Adapter 2 Attached to Internal Network (for internal communication)  → 
Attach the Windows Server 2022 ISO under Storage  → 
Start the VM and install Windows Server 2022.
<br />
<br />
<img src="https://imgur.com/weRNjH1.jpg"  height="80%" width="80%">
<br />
Adapter 2: Attached to Internal Network <br/>
<img src="https://imgur.com/Y342fpd.jpg"  height="80%" width="80%">
<br />
<br />

<p align="center">
Configure the Domain Controller <br/>
<br Step 1.1:Domain Controller VM (Windows Server 2022)/>
Set Static IP Address on the Internal Network ||
Log in to Windows Server 2022  → Open Network & Internet Settings  →  || Go to Ethernet (Internal Network) → Change adapter options || Right-click Internal NIC → Properties → Internet Protocol Version 4 (TCP/IPv4)  →  || IP Address: 170.18.0.1   → || Subnet Mask: 255.255.255.0 →  || Default Gateway: Leave blank  →  || DNS Server Use the localhost of the machine itself(127.0.0.1)  →  || Click OK and close. 
<br />
<br />
<img src="https://imgur.com/TJbPVXB.jpg"  height="80%" width="80%">
<br />
Adapter 2: Set Static IP Address on the Internal Network <br/>
<img src="https://imgur.com/2RqvNxW.jpg"  height="80%" width="80%">
<br />
<br />
  
<p align="center">
Install Active Directory & Promote to Domain Controller
 <br/>
Open Server Manager → Click Manage → Add Roles and Features  → || Select: Role-based installation → || Active Directory Domain Services  → 
Click Next and install  → || After installation, open Server Manager → Click Promote this server to a domain controller → || Add a new forest → Enter your domain name || Set Directory Services Restore Mode password  → || Click Next → Install → Reboot 
<br />
<br />
<img src="https://imgur.com/bXeMP0e.jpg"  height="80%" width="80%">
<img src="https://imgur.com/fhKpETv.jpg"  height="80%" width="80%">
<br />
<br />
Create decade domain admin  <br/>
   <br/>
In Server Manager Tools → Active Directory Users and Computers →  Right click on domain new organization group unit create a new user within that folder  →  || Right Click on User you just  created  → Add to a group  → Domain Admin →  Check Names  →  OK →  Sign out and sign into the account you just created 
  <br />
<img src="https://imgur.com/NTnQToj.jpg"  height="80%" width="80%">
<img src="https://imgur.com/CybXl8Q.jpg"  height="80%" width="80%">
<img src="https://imgur.com/Znstd5F.jpg" height="80%" width="80%">
<img src="https://imgur.com/xDElfar.jpg"  height="80%" width="80%">
<br />
<br />

<p align="center">
Configure DHCP Server on Domain Controller 
 <br/>
Open Server Manager → Click Manage → Add Roles and Features  → Select: Role-based installation  → Install DHCP Server
Click Next and install →  || After installation, open Server Manager → Click Tools → DHCP ||  Right Click on IPV4  → New Scope  → Enter Scope Name ||  → Start IP Range: 170.18.0.100 - End IP address 170.18.0.200   → || Length 24 → || Subnet Mask: 255.255.255  → Router( Default Gateway)  170. 18.0.1  →  CLick Add  → Next  →  Yes, I want to activate this scope now  →  Right Click o on DHCP Server → Authorize  → refresh (SShould turn from Red to Grenn)

<br />
<br />
<img src="https://imgur.com/JBlBKKK.jpg"  height="80%" width="80%">
<img src="https://imgur.com/5V8dgd6.jpg"  height="80%" width="80%">
<img src="https://imgur.com/VnmXMRj.jpg"  height="80%" width="80%">

<br />
<br />
<p align="center">
Configure RAS/NAT for Internet Access
 <br/>
Open Server Manager → Click Manage → Add Roles and Features  → Select: Role-based installation  →   Install Remote Access → Routing 
Click Next and install →  || After installation, open Server Manager → Click Tools →  ||  Routing and Remote Access → Open Routing and Remote Access →  ||  Right CLick on Server  → Configure and Enable Routing and Remote Access  → Configuration(NAT)  →  NAT internet connection choose Internet → Finish || 
Your Internal machines should now have internet access
<br />
<br />
<img src="https://imgur.com/BqJ0OUn.jpg"  height="80%" width="80%">
<img src="https://imgur.com/4nRNsHy.jpg"  height="80%" width="80%">
<img src="https://imgur.com/HJVkq39.jpg"  height="80%" width="80%">

<br />
<br />

<p align="center" >
Create Users with PowerShell 
 <br/>
 Open PowerShell as Administrator on DC → CD to the path of the account and run the script


<br />
<br />
<p align="center" >
List of Users .txt
 <br/>
<img src="https://imgur.com/EFo6qtm.jpg"  height="80%" width="80%">
<img src="https://imgur.com/4zCRSlc.jpg"  height="80%" width="80%">
<img src="https://imgur.com/HOm4v8K.jpg"  height="80%" width="80%">
<img src="https://imgur.com/DOOcPj2.jpg"  height="80%" width="80%">

<br />
<br />

<p align="center">
Create two CLIENT Virtual Machine and Join the domain (Windows 10)
 <br/>
Click New in VirtualBox  → Name the VM  → Click Settings → Network and set Adapter 1  Attached to Internal Network →  Start the VM and install Windows 10, after Installation is complete  → ||
Join Windows 10 Client to the Domain  → Log into Windows 10  → open Command Prompt ipconfig /all to check if DHCP assigned an IP →  ping IP Address(170.18.0.1) → Change System Name  → Rename this PC(Advanced)→ Change →Member of Domain(enter domain name) →Enter Administrator credentials from DC
<p align="center">
^^^REPEAT STEPS TO SETUP MORE CLIENT SYSTEM ON THE DOMAIN ^^^
 <br/>
<br />
<br />
<img src="https://imgur.com/y8AJJht.jpg"  height="80%" width="80%">
<img src="https://imgur.com/jUHNHlc.jpg"  height="80%" width="80%">
<img src="https://imgur.com/OiasoL9.jpg"  height="80%" width="80%">

<br />
<br />

<p align="center" >
End Result/ Test & Verify
 <br/>
1) Log in to Windows 10 with a newly created domain user
 <br/>
2) Use Active Directory Users and Computers to verify users.
 <br/>
<img src="https://imgur.com/JRm511A.jpg"  height="80%" width="80%">
  <br/>
<p align="center" >
END


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
