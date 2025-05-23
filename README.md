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
Once both of the VMs have restarted, we will log into the Client-1 virtual machine through Remote Desktop. Inside the Client-1 vm, we will attempt to ping DC-1's IP address using Powershell. This should be successfull for us now since we disabled DC-1's firewall earlier, allowing the machine to respond to the ping. Now open Powershell as an administrator and write in the command "ipconfig /all" on Client-1 to confirm that DC-1 is configured as the DNS server for the virtual machine.   
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
