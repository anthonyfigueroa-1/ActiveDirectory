<p align="center">
<img src="https://i.imgur.com/AskeISC.png" alt="Active Directory logo"/>
</p>

<h1>Active Directory</h1>
This tutorial outlines how to set up active directory using Azure.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>Prerequisites<h2>

- Have a virtual machine within Azure that is running Windows Server 2022 Datacenter Azure Edition, name this DC-1.
- Have a second virtual machine within Azure that is running Windows 10, name this Client-1.
- Have both virtual machines connected to the same vnet when setting up.

<h2>Steps</h2>

<p>
<img src="https://i.imgur.com/aydK00C.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
First you will want to login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> which is a perpetual ping to keep us updated on the connectivity. Now login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. Afterwards check back into Client-1 to see the ping should go through.

  <br />
  
<p>
<img src="https://i.imgur.com/SE06Fvz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

You will now want to login to DC-1 and install Active Directory Domain Services. Go in and promote as a domain controller, then setup a new forest as "mydomain.com", doesn't have to be this exactly it can be whatever you would like as long as you can remember it. Now restart DC-1 and then log back into it as user "mydomain.com\labuser".

<br />

<p>
<img src="https://i.imgur.com/ikCMoQ2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

 Now in this step we are going to have to create a few things. First we want to create an Admin and Normal User Account in Active Directory. Within Active Directory Users and Computers you will want to create an Organizational Unit called “_EMPLOYEES” and another called “_ADMINS”. Now you will create a new employee named “Jane Doe”, you can use whatever password you would like as long as you can remember it, with the username of “jane_admin”. Go in and add jane_admin to the “Domain Admins” Security Group. Then you will want to log out and close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”. From now on you will want to login using user jane_admin as your admin account from now on.

  <br />
  
<p>
<img src="https://i.imgur.com/eLSWAC6.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/h7VEvSC.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

 Next join Client-1 to your domain "mydomain.com", or whatever domain you have chosen. From within the Azure portal you will set Client-1’s DNS settings to the DC’s Private IP address. Stay within the Azure portal and go ahead and restart Client-1. Now remote desktop and login to Client-1 as the original local admin "labuser" and join it to the domain. Login as well, through remote desktop, to the Domain Controller and verify that Client-1 shows up in Active Directory Users and Computers inside the “Computers” container on the root of the domain. Go ahead and create another new Organizational Unit named “_CLIENTS” and drag Client-1 into there.

  <br />
  
<p>
<img src="https://i.imgur.com/Mo199lY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

  - Setup Remote Desktop for non-administrative users on Client-1
- Log into Client-1 as mydomain.com\jane_admin and open system properties
- Click “Remote Desktop”
- Allow “domain users” access to remote desktop
- You can now log into Client-1 as a normal, non-administrative user now
- Normally you’d want to do this with Group Policy that allows you to change MANY systems at once

  <br />
  
  <p>
<img src="https://i.imgur.com/ZsO8bhA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
  - Create a bunch of additional users and attempt to log into client-1 with one of the users
- Login to DC-1 as jane_admin
- Open PowerShell_ise as an administrator
- Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
- Run the script and observe the accounts being created
- When finished, open ADUC and observe the accounts in the appropriate OU
- attempt to log into Client-1 with one of the accounts (take note of the password in the script)
  
    <br />
