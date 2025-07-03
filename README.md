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

- Create two Virtual Machines (DC-1 and Client-1)
- Set DC-1's private IP address to static
- Join the Client-1 VM to the domain of the DC-1 VM
- Install Active Directory Domain Services on the DC-1 VM
- Set up our domain admin user in DC-1
- Generate users with script


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://imgur.com/CQJjpQI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/7HtxykD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/QW1N3ly.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
</p>
<p>
First step, we need to create two Virtual Machines: One running  Windows Server 2022, named DC-1. Pick the VM with 2vcpus. The other VM will run on Windows 10, named Client1. Pick the VM with 2vcpus. Both VM's should be connected to the same virtual network (VNet). 
Once you have created both of the VMs, we will configure the IP address on the domain controller (DC-1), changing it from dynamic to static. By doing this, we will ensure that the client VM (Client-1) can join the domain and use DC-1 as its DNS server.  
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/Z5APCif.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/cWNx0cG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next step is to enter the domain controller (DC-1) through a Remote Desktop Connection (RDP). Here we will disable the firewall for the domain, private and public profile. To do this we right-click the windows symbol and select "run", or you can simply click on "windows button+R". 
Type in "wf.msc" --> ok. 
In the Windows Defender Firewall window, ensure the firewall is turned off for all profiles, then click "apply" and "ok". 
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/T8mhA7n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
</p>
<p>
Next we will change the DNS server on Client-1 to the static private IP adress of DC-1. To do this we need to go back to Azure portal --> Virtual Machines --> Client-1 and go to Network Settings --> DNS server. 
After making this change, we need to restart both of the VMs to apply the new DNS configuration. 


</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/5Dohsx1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once both of the VMs have restarted, we will log into the Client-1 virtual machine through Remote Desktop. Inside the Client-1 VM, we will attempt to ping DC-1's IP address using Powershell. 
This should be successfull for us now since we disabled DC-1's firewall earlier, allowing the machine to respond to the ping. 
To do this, open Powershell as an administrator and write in the command "ping". When the ping is a success, write the command "ipconfig /all" on Client-1 to confirm that DC-1 is configured as the DNS server for the Virtual Machine.  
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/2seRFTe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we log into the Domain Controller (DC-1) and install Active Directory Domain Services (AD DS).
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/vq5aQ1P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
--> We will promote DC-1 to a Domian Controller and set up a new forest using the domain name "mydomain.com".
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/kCWa67Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/h1Grc3l.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We now create two organizational units called "_EMPLOYEES" and "_ADMINS". To do this, open Active Directory Users and Computers --> right-click on mydomain.com and select "new"--> choose "Organizational Unit".
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/nOoy76e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/nNGIALy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next step is to create a user. We will do this in the _ADMINS organizational unit --> right-click and choose new --> user. This users name is Kate Doe and will have the username "kate_admin".
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/bX3psQd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we will add Kate as a Domain Admin. To do this right-click on Kates account --> properties --> member off 
--> click "add". Now a new prompt will appear. Then write "domain admins"--> check name (it will find a domain admin build in group)
)--> ok. Apply the changes.  Now, we will log out of DC-1 and reconnect using RDP with the credentials "mydomain.com\kate_admin" and the assigned password. 
We will use this account for all future logins to DC-1.
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/z3KKlKQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next step is to join Client-1 to the domain (We have already set Client-1's DNS settings to the DC1's Private IP address from the Azure Portal).
To do this, log into Client-1 VM. Right-click on the Windows logo, select System --> rename this PC (Advances).
A new prompt will appear, click Change. In the new prompt select Domain and enter the domain name "mydomain.com" --> ok. The VM will restart to apply the changes.
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/Qweb5cu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Go back to DC-1 and open Active Directory Users and Computers. We will now create another organizational unit named _CLIENTS under mydomain.com
</p>
<br />
<h2></h2>


<p>
<img src="https://imgur.com/CKtulNJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The next step is to allow domain users to RDP access into the VM. To do this, we go back to Azure portal and log into Client-1 VM as Kate_admin.
Once inside the VM, right-click on the Windows logo and select "system". Find "Remote Desktop" on the left side and click on it.
Click on "Select users that can remotely access this PC". 
Now a new prompt will appear --> click on "add" --> wite "Domain Users" (you can click on "check names" to be sure you wrote the correct name) --> click OK.
</p>
<br />
<h2></h2>

<p>
<img src="https://imgur.com/Mf0sYAY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/nV0qC8T.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In this step we will use a script found here: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1. 
Log into DC-1 as Kate_admin and open PowerShell ISE as an administrator. Create a new file and paste the script into it, then execute it. 
Now we can observe as thousands of accounts are created automatically.
</p>
<br />
<h2></h2>


<p>
<img src="https://imgur.com/19qMn6q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After running the script, we open Active Directory Users and Computers to vertify that the account have been created. Click on the folder "_EMPLOYEES". 
Here you will see the accounts in the appropriate organizational unit.
</p>
<br />
<h2></h2> 

<p>
<img src="https://imgur.com/o0I7C4D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Last step we log into the Client-1 VM using one of the accounts we just made using the PowerShell script.
We do this by using the username for the account we choose and the default password "Password1". 
When you are logged inn, open PowerShell to vertify that you are logged in as one of the script-created users.
</p>
<br />


