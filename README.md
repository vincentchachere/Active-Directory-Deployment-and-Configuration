<br>

<p align="center">
<img width="700" src="https://github.com/user-attachments/assets/9b6b0a51-6411-4e01-96c5-1bb31e6fd986" alt="Microsoft Active Directory Logo"/>
<br>

<br>

<br>

<h1 align="center">Active Directory Infrastructure Setup, Deployment, and Configuration</h1> 
<br>

<p align="center">
<img src="https://github.com/user-attachments/assets/5d4b8c68-d1a6-4a4e-93e3-d8b8602d9123" height="85%" width="85%" alt="9"/><br />
</p>
<br />

## Lab Overview

This project is the first in our Azure and Active Directory tutorial series, laying the foundation for the upcoming lessons.

The goal is to create a basic Azure lab environment that simulates an enterprise Active Directory setup. By completing this project, we’ll build the necessary infrastructure to explore Active Directory functionalities in an Azure network, preparing for more advanced topics in future projects.

In the simulated Active Directory environment, we will deploy and configure Active Directory, create user accounts, and log into the client's virtual machine with one of these users to verify proper authentication, credentials, and permissions.

In this "Active Directory Infrastructure Setup, Deployment, and Configuration" project, we’ll cover key aspects like installation, forest creation, user management, domain integration, and custom Remote Desktop access, providing a solid foundation in Active Directory services.

## On-Premises Active Directory Deployed in the Cloud (Azure)
Active Directory essentially manages user accounts, passwords, permissions, and devices at large scale. This tutorial explains how to implement on-premises Active Directory in Azure Virtual Machines.

<ins>The difference between On-Premise Active Directory and Azure Active Directory</ins>:

- `On-Premise Active Directory`: Refers to infrastructure hosted and managed locally within an organization's physical data centers. This requires direct management and maintenance by the organization.

- `Azure Active Directory`: Refers to infrastructure and services provided remotely by Microsoft, hosted on their global data centers. This offers scalable, managed cloud services with remote access.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)

## High-Level Infrastrusture, Deployment, and Configuration Steps

#### Part 1: Building the Infrastructure
- Setup Domain Controller (Windows Server 2022) in Azure named "DC-1"
- Set Domain Controller's (DC-1's) NIC Private IP address to be static
- Log into DC-1 VM and Enable Both ICMPv4 Inbound Rules (for testing connectivity)
- Setup Client-1 VM (Windows 10 (21H2)) in Azure named "Client-1"
- Attempt to ping DC-1's Private IP Address from Client-1 VM

#### Part 2: Deploying Active Directory and User Creation
- Install Active Directory
- Create a Domain Admin User within the Domain D-1 VM
- Join Client-1 to Domain Controller
- Setup Remote Desktop for Non-Administrative Users on CLient-1 VM
- Attempt to log into Client-1 with one of the created users

## Configuration Steps

<details>

<summary>

## ⚙️ Part 1: Building Active Directory Infrastructure

</summary>

### 1.A ) Create Domain Controller (DC-1)

First, create a resource group to host the virtual machines: DC-1 (Domain Controller) and Client-1.

<ins>Here are the following configurations</ins>:

  - Resource Group: `Active-Directory-Lab`

  - Virtual Machine Name: `DC-1`

  - Region: `East US`

  - Image: `Windows Server 2022 Datacenter: Azure Edition - x64 Gen2`
    - *Make sure to choose the right one here or it will mess up when setting up your domain controller*
   
  - Size: `Standard_D2s_v3 - 2 vcpus, 8 GiB memory ($137.24/month)`

  - Username: `labuser` (or whatever you want - just remember it)

  - Password: `SomethingYouCanRemember`

  - Check: `The Two Licensing Boxes` at the bottom

  - Go To: `Networking` Tab for Step 1.A so that you can create your Virtual Network and Subnet

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/de53543c-2bef-488e-8f95-a10422ed15ce">

***

### 1.B ) Create Virtual Network and Subnet

  - Virtual Network: `Active-Directory-Vnet`

  - *The Subnet will create itself*

  - Click: `Review + Create`

    Then..

  - Click:  `Create`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/0eccc2b2-0b13-4351-8315-b84b42e26d1a">

***

### 2.A ) Set Domain Controller's (DC-1) NIC Private IP Address to be Static

- Go To: `DC-1's NIC Private IP Address`

  - Resource Group > DC-1 > Network Settings > `Network Interface` (dc-1139_z1) > `ipconfig1`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/fd418504-f33c-4938-b41d-52e5326486b7">

***

### 2.B ) Set Domain Controller's (DC-1) NIC Private IP Address to be Static

- Resource Group > DC-1 > Network Settings > `Network Interface` (dc-1139_z1) > `ipconfig1`

  - Select: `Static`

  - Click: `Save`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c017c1de-e443-4abe-963b-ede58dceb837">

***

### 3.A ) Log into DC-1 VM and Enable Both ICMPv4 Inbound Rules (for testing connectivity)

Now you can Remote Desktop (RDP) into DC-1 and Enable ICMPv4 so we can be ready to test the connectivity from Client-1 to DC-1 by pinging DC-1 from the Client-1 VM.

<ins>Once inside DC-1</ins>:

- Search: `wf.msc` (Windows Firewall - Microsoft)

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/aad49eba-fdb0-4671-bfae-8d28a4b7bb95">

***

### 3.B ) Log into DC-1 VM and Enable Both ICMPv4 Inbound Rules (for testing connectivity)

- Look for the rules with Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)

- Enable: `Both ICMPv4 Inbound Rules` (There are <ins>two</ins> you need to enable)

  - *They should have two green check marks next to them when you enable them just like the others.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3b0ec287-a457-4508-a08c-fb6c0c3b1c39">

***

### 4.A ) Create Client-1 VM

<ins>So similar to creating your domain controller, here are your Client-1 Configurations</ins>:

- Resource Group: `Active-Directory-Lab` (Same as your Domain Controller: DC-1)

- Virtual Machine Name: `Client-1`

- Region: `East US` (Same as your Domain Controller: DC-1)

- Image: `Windows 10 Pro, version 22H2 - x64 Gen2`

- Size: `Standard_D2s_v3 - 2 vcpus, 8 GiB memory ($70.08/month)` (Same as your Domain Controller: DC-1)

- Username: `Use the same one you used for you Domain Controller` for simplicity sake

- Password: `Use the same one you used for you Domain Controller` for simplicity sake

- Check: `The Licensing Boxe` at the bottom

- Go To: `Networking` Tab

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d68b9dde-7cf9-4c3b-9a3b-54b07117a782">

***

### 4.B ) Create Client-1 VM

<ins>Within your Network Tab</ins>:

- Choose the `Same Virtual Network and Subnet` as your Domain Controller
   
    - *This part is crucial. If your Client-1 VM and Domain Controller are not on the same VNET and Subnet they will be unable to communicate, preventing domain-related operations like joining the domain or authenticating.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f60620f6-dfdf-4c06-8cc4-58bc7cdfa909">

***

### 5.A ) Set Client-1’s DNS settings to DC-1’s Private IP address

So now we will Set Client-1’s DNS settings to DC-1’s Private IP address, which will allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).

- Go To: Resource Group > Client-1 > Network Settings > `Network Interface (client-160_z1)` > `DNS servers`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2be62c15-ec58-4afe-8579-2e0bd3929243">

***

### 5.B ) Set Client-1’s DNS settings to DC-1’s Private IP address

- Go To: Resource Group > Client-1 > Network Settings > Network Interface (client-160_z1) > `DNS servers`

- Select: `Custom`

- Input: `DC-1's Private IP Address` (Example; mine is: 10.0.0.4)

- Click: `Save` when done

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c0e0d75a-6a25-4fd1-9c76-d50539b68c97">

***

### 5.C ) Set Client-1’s DNS settings to DC-1’s Private IP address

<ins>Now for the DNS Settings to sync in you must restart you Client-1's VM so</ins>:

- Go To: `Resource Group` > `Client-1`

- Restart: `Client-1` VM when done doing this

   - *If you do not restart your Client-1 VM after setting it's DNS Server to DC-1’s Private IP address then it will not successfully allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/b5c26b71-a822-46b3-a7ca-7fe04ead9877">

***

### 6. ) Attempt to Ping DC-1’s Private IP address from Client-1 VM

- If everything is setup correctly so far then you should have a successful ping from Client-1 to DC-1.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2bfa98e9-d1db-4e19-a9f3-3d23b5701ee6">

***

### 7. ) From Client-1 VM Open PowerShell and Run: ipconfig /all

- The `DNS Server` should show DC-1’s Private IP Address as shown in the image below.

  - If the DNS server on Client-1 is not set to DC-1's private IP (e.g., it shows 168.63.129.16), update the DNS settings manually to DC-1's private IP. Restarting Client-1's VM may also help apply the changes. If successful, the client may log you out, indicating it's trying to connect to the domain.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/da8e2d18-ec3e-4cef-b395-88e9d6cfe263">

***

</details>

<details>

<summary>

## ⚙️ Part 2: Deploying Active Directory and User Creation

</summary>

### 8. ) Install Active Directory

<ins>Go to the Domain Controller (DC-1) and in Server Manager Dashboard</ins>:

- Click": `Add roles and features`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4109a1e0-694c-4404-9109-4c69f23ca2ce">

<br>
<br>
<br>

<ins>Click Next until reaching the Server Roles section then</ins>:

- Select: `Active Directory Domain Services`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/612caea1-964c-4197-892d-fd3aa833c562">

<br>
<br>
<br>

<ins>Within this portion</ins>:

- Click: `Add Features`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3fa6a381-f5b0-4c94-bb48-794bea14b10b">

<br>
<br>
<br>

<ins>Click Next until reaching the Confirmation tab then</ins>:

- Check the: `Restart the destination server automatically if required` Box

- Click: `Yes`

- Click: `Install`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/81812bf1-e8b9-48f7-9507-fa4074af53c4">

<br>
<br>
<br>

<ins>When that's done installing</ins>:

- Click: `Close`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/7c73b736-cf0e-4545-a6d9-683b9ecf90ea">

<br>
<br>
<br>

### 9. ) Promote as a DC: Setup a new forest as mydomain.com

- *This can be anything, just remember what it is.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/377ca48e-b056-46a5-8504-9afa07a31297">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/47cd8d56-987c-4a4e-b2b8-4f586e2f85e8">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/fedd52a0-48d0-4c63-8adb-0c824671e1ae">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/35e80bf5-5348-4c1e-94c9-68cfe5f88de2">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6c03ba31-2e37-415a-bbd4-c993302a9e26">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/49bf91db-dd67-4e24-b1b3-09d8a6248db2">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/572f8848-94bd-49e9-9a33-692792af4d9f">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8d940c8e-4b2a-49be-a7af-3c8ad8a840f3">

***

- The DC-1 will now restart to complete its promotion to a Domain Controller.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/126fa108-df23-44f3-87ef-b1f58cfe7aea">

***

Now that your DC-1 VM is a domain controller, you need to decide how to log in: as a local user on your client VM or as a domain user on the domain controller. This means clarifying two things: which domain to use and which user account to use.

- For this lab you can log back into DC-1 as:

  - Username: `mydomain.com\labuser`

  - Password: `TheSamePasswordYou'veBeenUsing`

- *Make sure to use a backslash ( \ ) NOT a forward slash ( / ) or you will not be able to login.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8489277b-b15e-4ebc-947d-11c5a1988046">

***

### 10. ) Create a Domain Admin User within the Domain D-1 VM

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/190b5ddf-a472-4fc6-8195-819cab80ada6">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eb3f4120-b273-4901-b111-75081f98ea10">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/20ee7889-bf7a-4925-8d71-23711ed14f00">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bb799a7b-360b-455b-a480-f1b35549135f">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/911fdd20-3880-4ac9-92dc-d59cb5c2a4d6">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/886a3780-b19c-4802-be42-453199b288d0">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/dcb94011-6685-4c85-b501-7a0834d0b1d1">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d4b8977b-e96e-4da4-b683-4a8115ea2fb7">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/082a8038-dd74-407b-99f1-ba4c43210405">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c3fd5780-1fba-4847-9f73-89502b69ac53">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/06db3f2f-a19e-4baf-adc4-22aa750bef4a">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3c0508eb-56f6-4de5-9938-89324722b547">

***

### 11. ) Join Client-1 to Domain Controller

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9cc81c81-20c3-417f-8f4a-9231cb209170">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6e459538-9285-4016-8026-a3510456a931">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d2d79b1f-1cbb-4340-a5a1-de3e73524f98">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eadeed84-5714-431a-b95d-8e8cb841cf57">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/614eb7b1-1cad-441a-8e16-65f15b690e7f">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6bbc2ad2-a1a6-45ef-952f-1a19405cab2b">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9738fa42-b995-4983-b302-d9a869bc0711">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/94dc4c1b-eba9-4594-9429-bb83859bc9a8">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f06306b1-27cd-4a4e-878e-fd77c3c1cfb7">

***

### 12. ) Setup Remote Desktop for Non-Administrative Users on CLient-1 VM

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/0e2f3ecd-a2ed-4441-ac67-47a135955174">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/de0fa8ee-0fc9-46a3-9338-ac1611dd47c8">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/45f64722-e8f2-4651-93d3-be3d2a5beae2">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/aeb4af69-5f1a-4f52-a754-6c7b9dd7f4ba">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4609810b-6178-45ba-8552-9eaadc3eafe5">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/fe12756e-cb6f-4716-874e-5608f473b1bd">

***

### 13. ) Attempt to log into Client-1 with one of the Created Users

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/ecff73ad-7aa5-489f-a36c-f539dc699328">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2a0a8d88-09f1-43d6-8be6-69b43dc97a16">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/63b0c4aa-986d-42cc-888e-9fe67fd1a9b0">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bbb2ff68-8c58-4936-a0ff-503cb2584124">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/19c207bd-c074-4492-a880-a1c43276c21c">

***

</details>
