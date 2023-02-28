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

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/obLSRf5.png" height="80%" width="80%" alt="Creating DC-1"/>
</p>
<p>
Create a Virtaul Machine running Windows Server 2022. Make note of the Resource Group being created as we want our second machine to part of the same group.
</p>
<br />

<p>
<img src="https://i.imgur.com/eVbnbUN.png" height="80%" width="80%" alt="DC-1 v-net"/>
</p>
<p>
Make note of the virtaul network being created in the networking tab of the creation process.
</p>
<br />

<p>
<img src="[https://i.imgur.com/DJmEXEB.png](https://i.imgur.com/fslBhiK.png)" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Windows 10 machine called Client-1 in the DC-1 Resource Group.
</p>
<br />

<p>
<img src="https://i.imgur.com/WI5zoFO.png" height="80%" width="80%" alt="Client-1 v-net"/>
</p>
<p>
Make sure that Client-1 is on the same V-net as DC-1 in the Networking tab of the creation process
</p>
<br />

<p>
<img src="https://i.imgur.com/bSDrF6Q.png" height="80%" width="80%" alt="V-net diagram"/>
</p>
<p>
Using Azure's V-net Diagram we can ensure that both of our machines are on the same virtual network.
</p>
<br />

<p>
<img src="https://i.imgur.com/2SHTMoE.png" height="80%" width="80%" alt="Changing DC-1 to static Ip adress"/>
</p>
<p>
Following the path outlined at the top of the picture, we want to change the IP adress of DC-1 from dynamic to static.
</p>
<br />

<p>
<img src="https://i.imgur.com/8xSaBIo.png" height="80%" width="80%" alt="Ping timeout"/>
</p>
<p>
Iff we try to ping the private IP Adress of DC-1 we see that the request times out.
</p>
<br />

<p>
<img src="https://i.imgur.com/BWmMPT5.png" height="80%" width="80%" alt="Changing firewall to allow ICMP"/>
</p>
<p>
From DC-1 open the windows firewall advanced settings. Expand your window to fill the screen and sort by protocol by clicking the column titles Protocol. We're looking for ICMPv4, the network protocol used to send messages regarding connectivity between computers and devices. We want to turn those on. 
</p>
<br />

<p>
<img src="https://i.imgur.com/x7KVudW.png" height="80%" width="80%" alt="Succesful ping between Client-1 and DC-1"/>
</p>
<p>
If we go back to Client-1 we should see that the ping is now successful.
</p>
<br />

<p>
<img src="https://i.imgur.com/7374uuf.png" height="80%" width="80%" alt="Role-Based Features"/>
</p>
<p>
Select the Add Roles and Features option on our Server Manager and choose Role-Based or Feature-Based Installation.
</p>
<br />

<p>
<img src="https://i.imgur.com/nzTtGLK.png" height="80%" width="80%" alt="installing Active Directory part two"/>
</p>
<p>
Select DC-1 as our Server for the install.
</p>
<br />

<p>
<img src="https://i.imgur.com/47cSR20.png" height="80%" width="80%" alt="Choosing our flavor or Active Directory"/>
</p>
<p>
We want to install Active Directory Domain Services. (Ignore the fact this says, "installed", I took this screenshot after the fact.)
</p>
<br />

<p>
<img src="https://i.imgur.com/qbbn7Rg.png" height="80%" width="80%" alt="Promoting DC-1 to a Domain Controller"/>
</p>
<p>
Notice the yellow cone on the upper right-hand side of the application. Click here to see the option to promote our server to a Domain Controller.
</p>
<br />

<p>
<img src="https://i.imgur.com/2c0FFvC.png" height="80%" width="80%" alt="New forest, mydomain.com"/>
</p>
<p>
Create a new forest called, mydomain.com
</p>
<br />

<p>
<img src="https://i.imgur.com/dp54tB0.png" height="80%" width="80%" alt="Logging into DC-1 from our new domain"/>
</p>
<p>
Log back into Dc-1, this time with the username: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://i.imgur.com/j4RFUhy.png" height="80%" width="80%" alt="Path to create Organization Units"/>
</p>
<p>
Within our domain we'd like to create new Organizational Units, these are specific groups that contain specfic permissions that we, as the sysadmin can modify to extreme specificity. Organizational Units are the building blocks of Active Directory and can contain other users, computers, other Organizational Units, printers and more.
</p>
<br />

<p>
<img src="https://i.imgur.com/sUyTmKK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create an OU called _EMPLOYEES and one called _ADMINS.
</p>
<br />

<p>
<img src="https://i.imgur.com/marlqaX.png" height="80%" width="80%" alt="Creating new user jane_admin"/>
</p>
<p>
Create a new user called Jane Does and give her the username jane_admin. (It was supposed to be Jane Doe but I didn't notice until much later.)
</p>
<br />

<p>
<img src="https://i.imgur.com/Wnox9qZ.png" height="80%" width="80%" alt="Adding Jane to Domain Admins Security Group"/>
</p>
<p>
Add user Jane Does to Security Group: Domain Admins
</p>
<br />

<p>
<img src="https://i.imgur.com/zsqeX1O.png" height="80%" width="80%" alt="using admin account to login to DC-1"/>
</p>
<p>
Logout of DC-1 and log back in as jane_admin. This will be our admin account going forward.
</p>
<br />

<p>
<img src="https://i.imgur.com/ZCwuGNt.png" height="80%" width="80%" alt="Setting DC-1 to act as DNS server for Client-1 throuh Azure"/>
</p>
<p>
From the Azure Portal navigate to Client-1's NIC and select DNS Settings. Set the DNS Server to be the private IP address of DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/teMSNFt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We want to add Client-1 to our domain. From Advanced System Swettings, under Computer Name, we click Change.
</p>
<br />

<p>
<img src="https://i.imgur.com/nBYevg0.png" height="80%" width="80%" alt="Adding Client-1 to mydomain.com"/>
</p>
<p>
Add Client-1 to mydomain.com
</p>
<br />

<p>
<img src="https://i.imgur.com/JQIiCWs.png" height="80%" width="80%" alt="Client-1 inside of our domain"/>
</p>
<p>
Confirm Client-1 is inside of our domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/0uCs8D2.png" height="80%" width="80%" alt="Client-1 inside OU _CLIENTS"/>
</p>
<p>
Create a new Organizational Unit called _CLIENTS and drag Client-1 there.
</p>
<br />

<p>
<img src="https://i.imgur.com/Yskrw9h.png" height="80%" width="80%" alt="Logged into Client-1 as Jane Admin"/>
</p>
<p>
Login to Client-1 using our acount: jane_admin
</p>
<br />

<p>
<img src="https://i.imgur.com/DTui0ST.png" height="80%" width="80%" alt="adding domain users to RDP settings"/>
</p>
<p>
From the system settings of Client-1 go into Remote Desktop Protocol then Select users that can use remotely access this PC. Add the group "Domain Users".
</p>
<br />

<p>
<img src="https://i.imgur.com/cUCHfbF.png" height="80%" width="80%" alt="Powershell script to create new user accounts"/>
</p>
<p>
From inside DC-1 open Powershell ISE as an administrator. Paste the contents of <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">this script</a> into Powershell.
</p>
<br />

<p>
<img src="https://i.imgur.com/nTJ6oLd.png" height="80%" width="80%" alt="Observing sript output"/>
</p>
<p>
Run the script and observe its output in Powershell.
</p>
<br />

<p>
<img src="https://i.imgur.com/blEHfWK.png" height="80%" width="80%" alt="Observing in Active Directory our new users"/>
</p>
<p>
Observe in ADUC under the _Employees tab our new users.
</p>
<br />

<p>
<img src="https://i.imgur.com/muJGnkz.png" height="80%" width="80%" alt="Login to client-1 using a new employee account"/>
</p>
<p>
Login to Client-1 using one of our newly created employee accounts.
</p>
<br />
