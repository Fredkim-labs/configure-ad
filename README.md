
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

<h3Create the Virtual Network</h3>

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
- If "Server Manager" doesn't start up or can't be found, then you may have installed the wrong OS.
<br>
<h3>Turn the firewall off for DC-1</h3>

![image](https://github.com/user-attachments/assets/401d8d7b-1e73-47cb-bb3d-5d40464ac142)
<br>

![image](https://github.com/user-attachments/assets/4dda1c3b-83a8-4967-8566-1265c9fa8b52)
<br>

![image](https://github.com/user-attachments/assets/7a3b2222-99b1-4f99-bd31-6314488ad6e6)
<br>
- Right-click the start button and select Run.
- Type "wf.msc" to open Windows's Firewall.

![image](https://github.com/user-attachments/assets/9e6e591b-3202-488f-926a-8eb788688b94)
<br>

![image](https://github.com/user-attachments/assets/65d05861-6495-4659-9574-1ad97419a79b)
<br>
- In the firewall, select Windows Defender Firewall Properties then off the Firewall state on every profile, lastly hit apply then OK
- After that, the firewall should be off
<br>
<h3>Set Client-1's DNS settings to DC-1's Private IP address </h3>

![image](https://github.com/user-attachments/assets/e10e0a4d-e6d9-4a21-bb49-14efe66e7932)
<br>

![image](https://github.com/user-attachments/assets/c3d1646f-c8a2-4957-9859-202fc56bfaad)
<br>
- Go  Azure portal get DC-1's Private IP address and copy it
- Go to Client 1 to Networking to Network Settings and click on Client 1's Virtual Network Interface Card
- Click on DNS servers hit custom and then paste the IP address of DC-1 that you copied from the Azure portal
- Hit Save
- Whenever the Client-1 needs to look at anything like for instance Google.com, it will look to DC-1 for it
- Doing this will allow us to join the domain
<br>

![image](https://github.com/user-attachments/assets/3b518097-1cea-4dbe-ab4d-9e36db06652c)
<br>

![image](https://github.com/user-attachments/assets/bbdc3ff0-3e8a-4fd5-a9ec-ac15ae057661)
<br>
- Then go to your Virtual Machines in Azure click the box next to Client-1 and press restart at the top
- From here we will attempt to log in to Client 1 and attempt to ping DC-1's private IP address
- Get Client-1's Public IP address 
<br>

![image](https://github.com/user-attachments/assets/21174cc2-e703-4c99-afae-6fed9fb312b4)
<br>

![image](https://github.com/user-attachments/assets/a634e67b-5ac2-4058-976b-682f2aaae698)
<br>
- Open Remote Desktop and enter Client-1
- Paste the Client-1's Public IP into the Remote Desktop
- Fill in Client-1's username and password and log in to Client-1.
<br>

![image](https://github.com/user-attachments/assets/3d1932fe-60a4-4bc8-b532-2cb9bfc3c708)
<br>

![image](https://github.com/user-attachments/assets/ab17fba3-ee32-424b-82a2-660c33f99eef)
<br>

![image](https://github.com/user-attachments/assets/d44e11a9-0c45-4819-9cbd-94a4c84a7ef7)
<br>
- Open Powershell
- In PowerShell type ping and DC-1's private IP address and press enter
- If the output says "Destination host unreachable," then they are probably in different virtual networks, or DC-1's firewall is probably still on, or ping is being blocked somehow.

![image](https://github.com/user-attachments/assets/9f10e1c3-f0c4-4981-8867-74f398c3a827)
<br>

![image](https://github.com/user-attachments/assets/bbef8f2a-e4da-446e-98ca-b864277f9b8a)
<br>
- Type in PowerShell ipconfig /all
- After execution, the DNS servers should display the private IP address of DC-1 in a highlighted manner.
- Now client 1 should be using DC-1 as the DNS server
<br>

<h3>Install Active Directory and Domain services in DC-1 </h3>

![image](https://github.com/user-attachments/assets/f143c2f1-a3ec-49d7-805a-7d009ae2a870)
<br>

![image](https://github.com/user-attachments/assets/94927c0a-db83-46c6-bf57-77402e8822ec)
<br>

![image](https://github.com/user-attachments/assets/0762b4d3-1097-4106-a81b-842014385f29)
<br>
- In the Server Manager dashboard, click 'Add Roles and Features' and continue the setup
- You’re going to click on "Next" mostly throughout the setup
- For Server Selection, there should only be 1 which is DC-1
<br>

![image](https://github.com/user-attachments/assets/bb57718a-a9cf-4bf1-8145-0b451fa1a3bf)
<br>

![image](https://github.com/user-attachments/assets/95a92afe-c827-4135-95d2-0a0a0f4953e3)
<br>
- In Server Roles check mark Active Directory Domain Services.
- Then hit Next, Check mark "Restart the destination server automatically if required" 
- Install
- After the installation hit close
 <br> 
<h3>Promote DC-1 to Domain Controller </h3>

![image](https://github.com/user-attachments/assets/36775f91-aa0b-491e-8aa8-59dad5bff7bb)
<br>

![image](https://github.com/user-attachments/assets/9de26c98-6721-482c-8116-cbc91f4f5a63)
<br>

![image](https://github.com/user-attachments/assets/9715a72c-06d5-44f4-8d50-d5de966d9876)
<br>
- Once the installation is done, there is a Yellow Flag at the top right in the Server Manager.
- Click on the flag and press the highlighted text "promote this server to Domain Controller"
- Next, we will select 'Add a new forest' and set the Root domain name to “mydomain.com”
- Hit Next, where it says "Directory Services Restore Mode (DSRM) password"  Just put in a password that you can remember and confirm it
- Finish setup and restart DC-1
<br>

![image](https://github.com/user-attachments/assets/a3901e13-02c6-480a-82c5-b3c95120e208)
<br>
- Log back into Remote Desktop with your username credentials following with "\mydomain.com" 
<br>
<h3>Create an Admin in Active Directory </h3>

![image](https://github.com/user-attachments/assets/77310044-5a70-405c-9f0e-dba2a30772b7)
<br>

![image](https://github.com/user-attachments/assets/036cc4d1-e816-4541-948d-98a8dc054a72)
<br>

![image](https://github.com/user-attachments/assets/5150306f-951b-4a0f-994d-57f9fc90c2fd)
<br>
- Once DC-1 has rebooted, click on start, expand Windows Administrative Tools, and select Active Directory Users and Computers/
- Right-click on mydomain.com, select New then Organizational Unit 
- <p><strong> We will create two OU's labeled "_EMPLOYEES" and "_ADMINS" </strong></p>

![image](https://github.com/user-attachments/assets/871da000-3cd3-47ca-a57b-58ffe6f81d3f)
<br>

![image](https://github.com/user-attachments/assets/a1498e72-62b2-4b8b-a5b0-a93f6e06ff56)
<br>

![image](https://github.com/user-attachments/assets/d83b3220-874a-403f-b368-660fcd16ae13)
<br>

![image](https://github.com/user-attachments/assets/4e1ff84d-027a-4776-89d7-248b2fde02be)
<br>
- Right-click on Users and create a new user named "Jane Doe" with the username "jane_admin"
- Change Jane Doe into an admin account by right-clicking her name and adding her to the “Domain Admins” security group
<br>

![image](https://github.com/user-attachments/assets/fb9e25ef-487b-4958-88f7-f3b870897996)
<br>
<p><strong>- Logout of DC-1 and sign back in with Jane Doe’s credentials</strong></p>
<p><strong>- From now on we will be using Jane Doe’s credentials</strong></p>
<br>
<h3>Join Client-1 to Domain </h3>

![image](https://github.com/user-attachments/assets/2cb8238d-2721-4fc1-b941-95777c41c40b)
<br>

![image](https://github.com/user-attachments/assets/486ba761-99bd-42b4-9975-c98466d3047d)
<br>

![image](https://github.com/user-attachments/assets/c0633a7e-a519-4f18-8feb-53ad9851f9e3)
<br>
- Right-click the start button and select System
- On the right side, click Rename This PC(Advanced)
- Click change, select "Member of Domain" and type in mydomain.com and hit ok
<br>

![image](https://github.com/user-attachments/assets/e2f8eb9c-b901-462c-be4d-3d000c06f9f0)
<br>

![image](https://github.com/user-attachments/assets/331c5119-a73c-46c1-8d9f-75372321caa9)
<br>

![image](https://github.com/user-attachments/assets/f972bf50-8089-46b9-ad5d-31b6d03c2391)
<br>
- Use admin credentials to join the domain.
- Client-1 has been added, and the VM will restart.
<br>

![image](https://github.com/user-attachments/assets/d5af81d2-8967-4ebf-83a5-16055a5a306f)
<br>
- Open Active Directory Users and Computers
- To verify, click on computers and you will see Client-1
<br>
<h3>Setup Remote Desktop for non-administrative users </h3>

![image](https://github.com/user-attachments/assets/f9c2aa7e-a24e-4871-a673-0221d4856e6a)
<br>

![image](https://github.com/user-attachments/assets/634a60ef-7027-473d-93b4-0623dee87904)
<br>

![image](https://github.com/user-attachments/assets/fda7ad99-ed40-4694-aa27-7af2ff96c4b5)
<br>
- Log back into Client-1 using "mydomain.cpm\jane_admin" credentials
- Right-click Start and open "Systems"
- Select Remote Desktop
- Under User Accounts and click “Select users that can remotely access this PC”
- Add Domain Users and hit ok.
  
![image](https://github.com/user-attachments/assets/33856b33-634b-4ed7-83bd-688ff8d41b03)
<br>
<p><strong>- This allows normal users to login to Client-1</strong></p>
<br>

<h2> Final Thoughts </h2>

<p>
We've successfully completed the Active Directory Deployment and Configuration phase. By configuring Active Directory on the Domain Controller, we established our infrastructure, created a forest and administrator account, and integrated Client-1 into the domain. </p>
