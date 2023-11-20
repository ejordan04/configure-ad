<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between Client-1 and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account
- Add Client-1 to the domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into Client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We're going to create the Domain Controller Virtual Machine(VM) using Windows Server 2022 and name it DC-1. We will set the Domain Controller's NIC Private Address to be static because we don't want it to change. Next, we will create the Client VM that'll use Windows 10 and name it Client-1. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we will log into Client-1 with Remote Desktop and ping DC-1's Private address with a perpetual ping. Notice that it is unsuccessful. To fix this, we'll need to log into the Domain Controller and enable ICMPv4 on the local Windows firewall. If we check back at Client-1, the ping now works. 
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Let's now log in to DC-1 and install Active Directory Domain Services. When the installation is complete, promote it as a DC. Then setup a new forest to anything you want. I'll be calling my domain example.com. After that, restart the VM and log back into DC-1 using the new domain (example.com\user)
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we're going to create an admin and normal user account in Active Directory. Under the tools tab in Server Manager, click Active Directory Users and Computers (ADUC) and then create an Organizational Unit (OU) called _EMPLOYEES. Also, create a new OU named _ADMINS. We'll create a new employee named Jane Doe with the username of jane_admin. Next, we'll add jane_admin to the Domain Admins Security Group, and once that's done we'll log out and log back in with example.com\jane_admin.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We're now going to join Client-1 to our domain. In the Azure Portal, we need to set Client-1's DNS settings to the DC's Private IP address. After doing that, we restart Client-1 and then log back in as the original local admin to join Client-1 to the domain we created. If everything is successful, we should see a welcome to the domain message appear.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we're going to log into Client-1 with example.com\jane_admin to setup Remote Desktop for non-administrative users. Once we have logged in, open system properties and click Remote Desktop. Allow domain users to access Client-1. If done correctly, any normal non-administrative user can use Client-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Finally, we will create a bunch of additional users and attempt to log into Client-1 with one of them. Log into DC-1 as jane_admin, then open up PowerShell_ISE as an administrator. We will be using a script from this link https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 to make additional users appear in the _EMPLOYEES OU. When the script is finished, we'll login in using one of the user's credentials. 
</p>
<br />
