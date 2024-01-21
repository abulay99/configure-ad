<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - Setup Resources in Azure 
- Step 2 - Ensure Connectivity between the client and Domain Controller
- Step 3 - Install Active Directory
- Step 4 - Create an Admin and Normal User Account in AD
- Step 6 - Join Client-1 to your domain (mydomain.com)
- Step 7 - Setup Remote Desktop for non-administrative users on Client-1
- Step 8 - Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/qKg89Dn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
  
<h2>Setup Resources in Azure</h2>
<li>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</li>
<li>Take note of the Resource Group and Virtual Network (Vnet) that get created at this time</li>
<li>Set Domain Controller’s NIC Private IP address to be static</li>
<li>Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a</li>
<li>Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher</li>

<h2>Ensure Connectivity between the client and Domain Controller</h2>
<li>Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)</li> 
<li>Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall</li>
<li>Check back at Client-1 to see the ping succeed</li>

<h2>Install Active Directory</h2>
<li>Login to DC-1 and install Active Directory Domain Services</li>
<li>Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)</li>
<li>Restart and then log back into DC-1 as user: mydomain.com\labuser</li>

<img src="https://i.imgur.com/KgCX53Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/f6BaxLi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<h2>Create an Admin and Normal User Account in AD</h2>
<li>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”</li>
<li>Create a new OU named “_ADMINS”</li>
<li>Create a new employee named “Jane Doe” (same password) with the username of “jane.doe”</li>
<li>Add jane.doe to the “Domain Admins” Security Group</li>
<li>Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane.doe”</li>
<li>User jane.doe as your admin account from now on</li>


</p>
<br />

<p>
<img src="https://i.imgur.com/pVO85gC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<h2>Join Client-1 to your domain (mydomain.com)</h2>
<li>From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address</li>
<li>From the Azure Portal, restart Client-1</li>
<li>Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)</li>
<li>Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain</li>
<li>Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)</li>


<h2>Setup Remote Desktop for non-administrative users on Client-1</h2>
<li>Log into Client-1 as mydomain.com\jane_admin and open system properties</li>
<li>Click “Remote Desktop”</li>
<li>Allow “domain users” access to remote desktop</li>
<li>You can now log into Client-1 as a normal, non-administrative user now</li>
<li>Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)</li>

</p>
<br />

<p>
<img src="https://i.imgur.com/7rzo1qk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/vpWWUYF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
</p>
<p>
Create a bunch of additional users and attempt to log into client-1 with one of the users
<li>Login to DC-1 as jane.doe</li>
<li>Open PowerShell_ise as an administrator</li>
<li>Create a new File and paste the contents of the script into it</li> (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
<li>Run the script and observe the accounts being created</li>
<li>When finished, open ADUC and observe the accounts in the appropriate OU</li>
<li>Attempt to log into Client-1 with one of the accounts (take note of the password in the script)</li>

</p>
<br />
