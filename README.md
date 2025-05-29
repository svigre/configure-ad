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


<h2>Deployment and Configuration Steps</h2>

<p>

![image 1](https://github.com/user-attachments/assets/505e55b3-712c-40ff-8f04-464a8ad203ce) ![image 2](https://github.com/user-attachments/assets/c679a690-617b-4257-85de-2e4100b54067) ![image 3](https://github.com/user-attachments/assets/e3596f2a-0ce6-4566-883c-f1cef5a889c4)







First step, we need to create two Virtual Machines: One running  Windows Server 2022, named DC-1. Pick the vm with 2vcpus. The other VM will run on Windows 10, named Client1. Pick the vm with 2vcpus. Both VM's should be connetcted to the same virtual network (VNet). 
Once you have created both of the VMs, we will configure the IP address on the domain controller (DC-1), changing it from dynamic to static. By doing this, we will ensure that the client vm (Client-1) can join the domain and use DC-1 as its DNS server.   


</p>
<p>
<p>
  
</p>
 
  ![image 4](https://github.com/user-attachments/assets/03f63707-045d-471e-adf6-478dfd56e17f) ![image 5](https://github.com/user-attachments/assets/7ccacb7e-b538-496b-bef1-565cfcd177c8)


Next step is to enter the domain controller (DC-1) through a remote desktop connection (RDP). Here we will disable the firewall for the domain, private and publick profile. To do this we right-click the windows symbol and select "run", or you can simply click on "windown button+R". Type in "wf.msc"-->ok. 
In the Windows Defender Firewall window, ensure the firewall is turned off for all profiles, then click "apply" and "ok".  
</p>
<br />

![image 7](https://github.com/user-attachments/assets/9891a7df-1d0b-4a7e-b891-577823663064)


Next we will change the DNS server on Client-1 to the static private IP adress of DC-1. To do this we need to go back to Azure portal --> Virtual Machines --> Client-1 and go to Network Settings--> DNS server. After making this change, we need to restart both of the VMs to apply the new DNS configuration. 
<p>
  
</p>

![image 9](https://github.com/user-attachments/assets/271b55bf-4286-40b7-a9d5-b8559ec02f75)


<p>
Once both of the VMs have restarted, we will log into the Client-1 virtual machine through Remote Desktop. Inside the Client-1 vm, we will attempt to ping DC-1's IP address using Powershell. This should be successfull for us now since we disabled DC-1's firewall earlier, allowing the machine to respond to the ping. To do this, open Powershell as an administrator and write in the command "ping". When the ping is a success, write in the command "ipconfig /all" on Client-1 to confirm that DC-1 is configured as the DNS server for the virtual machine.  
</p>
<br />

<p>

![image 11](https://github.com/user-attachments/assets/7e5bccc6-0745-45a5-92ee-e2710954e236)

Now we log into the Domain Controller (DC-1) and install Active Directory Domain Services (AD DS). 



![image 12](https://github.com/user-attachments/assets/49c3174b-e1b3-4c18-8f4b-2c476b2f5ce6)

--> We will promote DC-1 to a Domian Controller and set up a new forest using the domain name "mydomain.com". 


![image 15](https://github.com/user-attachments/assets/9dfdcf9a-4643-4d19-b7e9-4e3ebaacd671)
![image 16](https://github.com/user-attachments/assets/ebb6caf0-b7e1-4f9e-8f60-282eea11663a)

We now create two organizational units called "_EMPLOYEES" and "_ADMINS". To do this, open Active Directory Users and Computers --> right-click on mydomain.com and select "new"--> choose "Organizational Unit". 


![image 17](https://github.com/user-attachments/assets/e7975006-ef6c-43ba-8f96-251ed537a1a3)
![image 18](https://github.com/user-attachments/assets/eadcc633-0799-48a4-a54b-8d3404ffdc60)

Next step is to create a user. We will do this in the _ADMINS organizational unit -->right-click and choose new -->user. This users name is Kate Doe and will have the username "kate_admin".

![image 20](https://github.com/user-attachments/assets/323682e5-b769-4cf5-9906-6f4b9fe5b242)

Next, we will add Kate as a Domain Admin. To do this right-click on Kates account --> properties --> member off --> click "add". Now a new promp will appear. Here you can write in "domain admins"--> check name (it will find a domain admin build in group)--> ok. Apply the changes.
Now, we will log out of DC-1 and reconnect using RDP with the credentials "mydomain.com\kate_admin" and the assigned password. We will use this account for all future logins to DC-1. 


![image 21](https://github.com/user-attachments/assets/376650cc-9448-4b11-9e74-367cf0fb871f)


Next step is to join Client-1 to the domain. (We have already set Client-1's DNS settings to the DC1's Private IP address from the Azure Portal). So to do this, log into Client-1 VM. Right-click on the Windows logo, select System--> rename this PC (Advances). A new promp will appera, click Change. In the new promp select Domain and enter the domain name "mydomain.com" --> ok. The VM will restart to aplly the changes.


![image 23](https://github.com/user-attachments/assets/1e5cee8f-1397-431c-9a46-adf209dbc041)

Go back to DC-1 and open Active Directory Users and Computers. We will now create another organizational unit named _CLIENTS under mydomain.com









</p>
<p>
  
</p>
<br />
