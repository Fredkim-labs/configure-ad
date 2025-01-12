
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>


<h1>Active Directory Deployment and Configuration </h1>


<p>This project provides a comprehensive guide to deploying and configuring Active Directory on a designated Domain Controller (DC-1) virtual machine. We will install Active Directory, promote the server to a Domain Controller, create user accounts, and join a client machine (Client-1) to the domain. The tutorial also covers configuring Remote Desktop access for non-administrative users, ensuring a secure and functional Active Directory environment within Azure.

</p>





<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h1>Creating Our Resource, Network, & Virtual Machines </h1> 
<br>

<h3>&#9312; Create The Resource Group</h3>


<img width="348" alt="0" src="https://github.com/user-attachments/assets/6c7e9a17-44eb-44f1-a383-abb8bf2a97b0">

  <br>
  
- Go to https://portal.azure.com/#home to get started
  
- Once you are there click on/ search Resource Groups and afterward click on create
  
- For me, I named mine Active-Directory-Lab, and for the region, since I'm located in the Eastcoast, East US 2 worked out

- Create the resource group once you receive the Validation passed green check mark
  
  <br><br>

<h3>&#9313; Create the Virtual Network</h3>

<img width="410" alt="0A" src="https://github.com/user-attachments/assets/5dbe7a9b-87a0-4c3f-a73f-5e85faaf0e47">

<br>

- For this new step we will be creating the virtual network instead of having the virtual machine do it for us

- Search for Virtual Networks then hit create 

- Put it in your resource Group that we made, this network will be named Active-Directory-VNet

- select your region, once you hit click on "Review + Create," hit "Create"
  
<h3>&#9314; Please proceed with the creation of the virtual machines, beginning with DC-1</h3>

<img width="558" alt="1" src="https://github.com/user-attachments/assets/fa689f9f-99ff-4d91-86ac-0be9625dc3f5">


<br>

- Search for Virtual Machines and hit create 

- Please select your designated resource group. For the name of the virtual machine, I have chosen "DC-1"
  
- I have selected my region and configured the availability options to "Availability Zone," ensuring that "Self-Selected Zone" is also marked. For all other settings, I have retained the options as displayed. Furthermore, I have chosen the image for this virtual machine as "Windows Server 2022 Datacenter Azure Edition - x64 Gen 2"
  
<img width="545" alt="1A" src="https://github.com/user-attachments/assets/b3585c30-9b36-410e-b2c8-76b6c8fc3bb4">

<br>

- When determining the appropriate size, it is essential to ensure that there are a minimum of 2 virtual CPUs (VCPUs) allocated. Additionally, it is advisable to document your username and password in a secure location for future reference

  <br>

<img width="554" alt="1B" src="https://github.com/user-attachments/assets/8506c85c-be52-4ff6-b380-99c1d316bd8c">

<br>

- Please ensure that both of the licensing agreements presented are checked, and then proceed by selecting the Networking option
  
  <br>

<img width="556" alt="1C" src="https://github.com/user-attachments/assets/2fb1fb90-1a66-4952-9346-943647be0548">
  
  <br>
  
- When configuring the virtual network option, please ensure that you select the Virtual Network that has been established, It is advisable to retain all other settings at their default values, After confirming these selections, click on "Review + Create," followed by "Create" Upon completion of this process, we will proceed to create the subsequent virtual machine, designated as Client 1

<h3>&#9315; Set up the virtual machine named Client-1 </h3>

<br>
  
<img width="551" alt="2" src="https://github.com/user-attachments/assets/0a1cb73f-08da-4ec7-8ab1-c315c39e7eb3">

<br>

- The virtual machine will be designated as "Client-1"

- Put it in the same resource group that you made

- We will be using Windows 10 Pro, Version 22H2 - x64 Gen 2 for the image

  <br>

<img width="552" alt="3" src="https://github.com/user-attachments/assets/df842a56-0c16-4278-8404-2550690f59c5">
  
  <br>

- Make sure the size has at least 2 vcpus, and write down the username 
 and password for this virtual machine as well

  <br>

<img width="557" alt="4" src="https://github.com/user-attachments/assets/183db735-7a52-4d52-8c82-59253e9c8358">
  
  <br>

- Heading over to Networking we will place this Virtual Machine in the Active-Directory-VNet that we made
 
- Leave the rest to its defaults and click on "Review + Create," followed by "Create"

  <br>

<img width="1274" alt="4A" src="https://github.com/user-attachments/assets/a4fde97d-0d0c-4c2c-a518-78ba07d70d88">

  <br>

- Here are the 2 Virtual machines that we will be using for the rest of this project

<h1> Configuration Steps</h1>
  <br>

<h3>&#9312; Change DC-1's Nic Private IP address to be static</h3>

<br>

- We will begin by going to the virtual machine dc-1 and going to the network settings

<img width="1486" alt="5" src="https://github.com/user-attachments/assets/fd555109-48a1-4e0e-8ba9-796d9f6f6582" />

<br>

- Click on the Network interface card for dc-1 
<br>

<img width="1325" alt="6" src="https://github.com/user-attachments/assets/40354938-12eb-4927-9693-6559be383ce2" />

<br>

- Click on ipconfig to alter this to Static instead of dynamic
<br>

<img width="505" alt="7" src="https://github.com/user-attachments/assets/9cb9dee8-084d-4824-9912-10be5239f355" />

<br>

- Under Private IP address settings instead of dynamic click on static and save this

- Once you save this head back into dc-1's network settings

- The Private IP address should no longer change regardless of how many times the virtual machine is restored

  <br>

<h3>&#9313; Connect to DC-1</h3>

- (insert pic 12:21)
  
- First, copy the public IP address in dc-1

- Open Microsoft Remote Desktop --> name it dc-1 ---> paste the public IP address in the PC name ---> press add to connect (if needed put in the username and password u made to connect)

<br>

<img width="1676" alt="12" src="https://github.com/user-attachments/assets/a1fafc6e-e3af-441a-af58-bd18ac2b7a8e" />

<br>

- Once connected and loaded if you don’t have Server Manager pop up at the start then you logged in to the wrong virtual machine or created the wrong type

<br>
  

<h3>&#9314; Turn the firewall off for DC-1</h3>



<img width="1046" alt="8" src="https://github.com/user-attachments/assets/c16c4619-f998-493a-8177-e550011100b4" />

<br>

(insert pic 13:34)

- In the domain controller right click the start menu and press run, type wf.msc this is for Windows firewall

(insert pic 13:40)

- In the firewall, we are going to disable it so click on Windows Defender Firewall Properties ---> hit the off option for the Firewall state on every profile ---> hit apply then OK

- After that, the firewall should be off

<br>

<h3>&#9315; Set Client-1's DNS settings to DC-1's Private IP address </h3>

(insert pic 14:40)  (insert pic 15:01)

- First back in the Azure portal get DC-1's Private IP address and copy it, then go to Client 1 --> Networking --> Network Settings and click on Client 1's Virtual Network Interface Card

(insert pic 15:35)

- Click on DNS servers hit custom and then paste the IP address of DC-1 that you copied from the Azure portal

- Whenever the computer needs to lookup anything like for instance Google.com, it will look to DC-1 for it

- Doing this will allow us to join the domain

- Hit Save

- Then go to your Virtual Machines in Azure click the box next to Client-1 and press restart at the top

- From here we will attempt to log in to Client 1 and attempt to ping DC-1's private IP address

(insert pic 17:15)

- Click on Client 1 in Azure and copy its Public IP address 

(insert pic 17:25)

- Then head on over to remote desktop to enter Client-1, we are going to name this Client 1, and make sure to put in your username and password that you wrote down

- Head on over to DC-1 in Azure to get the private IP address

(insert pic 18:24)

- Login to Client-1 and Open up PowerShell

(insert pic 18:24)

- In PowerShell type ping and DC-1's private IP address and press enter

- If the output says "Destination host unreachable," then they are probably in different virtual networks, or DC-1's firewall is probably still on, or ping is being blocked somehow.

(insert pic 19:38)

- Type in PowerShell ipconfig /all

(insert pic 19:56)

- Upon execution, the line pertaining to the DNS servers should display the private IP address of DC-1 in a highlighted manner.

- Now client 1 should be using DC-1 as the DNS server







<h1> Configuration Steps Pt. 2 </h1>

<br>


<h3>&#9312; Install Active Directory and Domain services in DC-1 </h3>

<br>

(insert pic 2:56)
<br> 

- To make sure which virtual machine you are in see if the Microsoft store is located below in the taskbar if so that would be Windows 10 which is not DC-1

- Another way is going into your system settings and looking at the operating system

- Please navigate to the search bar located below and enter "Server Manager" if it is not displayed in the Start menu.

<br>

<img width="1676" alt="12" src="https://github.com/user-attachments/assets/dc010b2f-170c-4fd3-8c1f-8a63bb7a29a4" />

<br>
<br>

- In the Server Manager dashboard, click 'Add Roles and Features' and continue the setup

<br>

- You’re going to click on "Next" mostly throughout the setup

  (insert pic 3:14)

- For Server Selection, there should only be 1 which is DC-1


<img width="774" alt="14" src="https://github.com/user-attachments/assets/9f9e56d2-a6bf-4bdb-a4c2-0eaf2402862a" />
<img width="680" alt="AD-setup" src="https://imgur.com/cQnpkfN.png">



<br>

- In Server Roles check mark Active Directory Domain Services --> hit add features

- Then hit Next, Check mark "Restart the destination server automatically if required" --> click YES --> press Install

- After the installation hit close


<h3>&#9313; Promote DC-1 to Domain Controller </h3>

- Once the installation is done, notice the flag on the Server Manager at the top right
  
- Click on the flag and press the highlighted text "promote this server a.k.a DC-1 to Domain Controller"

<img width="350" alt="notif" src="https://imgur.com/4W04gBQ.png">



-  Next, we will select 'Add a new forest' and set the Root domain name to “mydomain.com”

-  Hit Next, where it says "Directory Services Restore Mode (DSRM) password"  Just put in a password that you can remember and confirm it

  5:18

<img width="565" alt="my domain" src="https://imgur.com/ovGgm26.png"> 
  
- Finish setup and restart DC-1

- Log back into Remote Desktop with your username credentials following with "@mydomain.com" 



<h3>&#9314; Create an Admin in Active Directory </h3>

- Once DC-1 has rebooted, click on tools and select Active Directory Users and Computers

- Right click on mydomain.com; select -> New -> Organizational Unit 

<img width="500" alt="Users" src="https://imgur.com/VESNQeS.png">

<br>

<p><strong> We will create two OU's labeled "_EMPLOYEES" and "_ADMINS" </strong></p>

<img width="500" alt="admins" src="https://imgur.com/vsSxufF.png">



<p><strong>Right click on Users and create a new user named "Jane Doe" with the username "jane_admin"</strong></p>

<img width="450" alt="jane doe" src="https://imgur.com/n9RKfcz.png">



<p><strong>We will change Jane Doe into an admin account by right clicking her name and adding her to the “Domain Admins” security group</strong></p>

<img width="450" alt="add to group" src="https://imgur.com/n9RKfcz.png">



<p><strong>Logout of DC-1 and sign back in with Jane Doe’s credentials</strong></p>

<img width="400" alt="jane login" src="https://imgur.com/EnnzYVs.png">



<h3>&#9315; Join Client-1 to Domain </h3>

<p><strong> For Client-1 to join the domain, the DNS server must be changed. Therefore we will set it's DNS server as DC-1's private IP address</strong></p>

- In the Azure Portal, select Client-1 -> Networking -> Network interface -> Settings -> DNS Server

<img width="350" alt="dns servers" src="https://imgur.com/9bKXViA.png">


<p><strong>Select custom DNS server and type in the private IP address of DC-1 and restart Client-1 virtual machine in Azure</strong></p>

<img width="410" alt="dns servers2" src="https://imgur.com/5hhy1Ac.png">


<p><strong> Now log back in to Client-1 using your original credentials. Click start and go to Settings -> Rename this PC (advanced) -> Change and add “mydomain.com” and login with the admin credentials previously created (jane_admin) </strong></p>

<img width="310" alt="remote desktop first login" src="https://imgur.com/OsjB5gK.png">

<br>

<p> <strong>Once Client-1 has been added, the VM will restart.</strong></p>



<h3>&#9316; Setup Remote Desktop for non-administrative users </h3>

- Log back into Client-1 using "jane_admin" credentials and open Settings -> Remote Desktop -> User Accounts and click “Select users that can remotely access this PC”
- Add Domain Users

<br>


<img width="350" src="https://imgur.com/R2sxVPR.png">

<p><strong>This allows normal users to login to Client-1</strong></p>

<br>




<h2> Final Thoughts </h2>

<p>
We've successfully completed the Active Directory Deployment and Configuration phase. By configuring Active Directory on the Domain Controller, we established our infrastructure, created a forest and administrator account, and integrated Client-1 into the domain. </p>
