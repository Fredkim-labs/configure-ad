
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>


<h1>Active Directory Deployment and Configuration </h1>


<p>This project provides a guide to deploying and configuring Active Directory on a designated Domain Controller (DC-1) virtual machine. 
  We will install Active Directory, promote the server to a Domain Controller, create user accounts, and join a client machine (Client-1) to the domain. 
  This will also cover configuring Remote Desktop access for non-administrative users and ensuring a secure and functional Active Directory environment within Azure.

</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure Virtual Machine and Network
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h1>Creating Our Resource, Network, & Virtual Machines </h1> 
<br>

<h3>Create The Resource Group</h3>

![image](https://github.com/user-attachments/assets/2816a875-3d80-43cb-8bc8-adfb07a60116)
<br>
  
- Go to https://portal.azure.com/#home 
  
- Select Resource Groups and create a new one.
  
- I named mine Active-Directory-net and select East US 2. I am on the East Coast.

- Create the resource group once you receive the green check mark
  
  <br><br>

<h3>&#9313; Create the Virtual Network</h3>

![image](https://github.com/user-attachments/assets/e2ba70b0-4eb6-4ef4-872c-a5cb2c323fe9)
<br>

- Next, create a virtual network,

- Search for Virtual Networks then hit create 

- Select active Resource Group and name the Virtual Network - Active-Directory-VNet

- Select your region, click on "Review + Create," and "Create".
  
<h3>Create a Virtual Machine, the beginning of DC-1</h3>

![image](https://github.com/user-attachments/assets/1708079b-6256-46ea-a0fc-a17957181e8e)
<br>
![image](https://github.com/user-attachments/assets/04b0257a-4d86-4dbb-a8ae-736dcf0a21d1)
<br>

- Search for Virtual Machines and hit create 

- Please select your designated resource group. Name the virtual machine, I named mine "DC-1"
  
- I have selected East US 2 for Region
- Select "Windows Server 2022 Datacenter Azure Edition Hotpatch- x64 Gen 2"
  
![image](https://github.com/user-attachments/assets/b172051b-b136-44ff-8991-ba601ddfa421)
<br>
![image](https://github.com/user-attachments/assets/31dd2735-33bd-4231-9c12-247c3cf20b3d)
<br>
- It is essential to ensure that there are a minimum of 2 virtual CPUs (VCPUs) allocated.
- It is advisable to document your username and password for future reference
- Check both of the licensing agreements and then proceed by selecting the Networking option
<br>
  ![image](https://github.com/user-attachments/assets/c7a5ba9f-7745-46de-8d56-5473fbd7e430)
<br>
  
- After confirming these selections, click on "Review + Create," followed by "Create". This virtual machine will designated as DC-1(Domain Controller-1) 

<h3>Create a Virtual machine named Client-1 </h3>
<br> 

![image](https://github.com/user-attachments/assets/b9ee4fbb-434b-45c3-adaf-e2f3712cd6e1)
<br>
![image](https://github.com/user-attachments/assets/ca547d44-4197-4443-94de-2cae3aada4b7)
<br>
![image](https://github.com/user-attachments/assets/4cbea7d7-e379-432f-a5dd-70c32b635fff)
<br>
- The virtual machine will be designated as "Client-1"
- Select the same resource group that you made
- Select the Windows 10 Pro, Version 22H2 - x64 Gen 2 for the image
- Make sure the size has at least 2 vcpus.
- Go to Networking and select Active-Directory-VNet.
- Leave the rest to its defaults and click on "Review + Create," followed by "Create"
<br>

![image](https://github.com/user-attachments/assets/7d2babae-b5c2-4006-bb76-c93428a0df7e)
<br>
- Here are the 2 Virtual machines that we will be using for the rest of this project.
<br>
<h1> Configuration Steps</h1>
  <br>

<h3>Change DC-1's NIC Private IP address to static</h3>

![image](https://github.com/user-attachments/assets/6a4d753d-3825-4d33-9807-55672878dc3b)
<br>
![image](https://github.com/user-attachments/assets/35516c59-6b5f-4038-b8e9-1a9488782951)
<br>
![image](https://github.com/user-attachments/assets/a4cf3190-b84a-419c-b59c-0f486c99be49)
<br>
- We will begin by going to the virtual machine dc-1 and going to the network settings
- Click on the Network interface card for dc-1 
- Click on ipconfig to alter this to Static
- DC-1's IP address is 10.0.0.4 and should not change.
  <br>
<h3>Connect to DC-1 and Disable Firewall</h3>

![image](https://github.com/user-attachments/assets/58d8b817-b8fc-46b4-9ff8-c2bb359f58c5)
<br>
![image](https://github.com/user-attachments/assets/2e523125-eaab-4984-802e-76c20dc21cac)
<br>
![image](https://github.com/user-attachments/assets/7a46f009-189f-4118-9658-c638f5d7aed8)
<br>

- Copy the public IP address of DC-1
- Open Microsoft Remote Desktop
- Fill in DC-1's public IP addiress
- Fill in the Credentials
<br>

![image](https://github.com/user-attachments/assets/3f7dc033-d930-48e8-afc0-f3b1d2649b42)
<br>
- Once connected and loaded if you don’t have Server Manager pop up at start-up, then search for "Server Manager" in the start menu.
<br>
<h3>Turn the firewall off for DC-1</h3>



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
