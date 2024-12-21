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
- Log into DC-1 VM and disable the Windows Firewall (for testing connectivity)
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

### 1. ) Create Domain Controller (DC-1)

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

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/634d8ac0-bea6-43fd-8aff-d83c77d754b5">
<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f8677c03-b2a1-4d96-a7d6-73dedc966bfe">

***

### 1.A ) Create Virtual Network and Subnet

  - Virtual Network: `Active-Directory-Vnet`

  - *The Subnet will create itself*

  - Click: `Review + Create`

    Then..

  - Click:  `Create`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/96a240c7-0f58-4546-9396-9daea85d85fe">

***

### 2. ) Set Domain Controller's (DC-1) NIC Private IP Address to be Static

- Go To: `DC-1's NIC Private IP Address`

  - Resource Group > DC-1 > Network Settings > `Network Interface` (dc-1139_z1) > `ipconfig1`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8157e36d-83eb-4b6c-8b54-a76fc77c9145">

***

### 2.A ) Set Domain Controller's (DC-1) NIC Private IP Address to be Static

- Resource Group > DC-1 > Network Settings > `Network Interface` (dc-1139_z1) > `ipconfig1`

  - Select: `Static`

  - Click: `Save`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/235993e5-227e-45cb-80c7-3bc3490de57b">

***

### 3. ) Log into DC-1 VM and Disable the Windows Firewall (for testing connectivity)

Now you can Remote Desktop (RDP) into DC-1 and disable the Windows Firewall so we can be ready to test the connectivity from Client-1 to DC-1 by pinging DC-1 from the Client-1 VM.

  - Copy and Paste: `DC-1 Public IP Address` into RDP app

  - Input: `Username and Password` you made when creating you Domain Controller

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f30b969a-fe63-47af-ba82-9584b976b0c9">
<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eb2a5b62-fa30-46ff-b4a3-fff86d72af03">

### 3.A ) Log into DC-1 VM and Disable the Windows Firewall (for testing connectivity)

- Once inside DC-1 search: `wf.msc` (Windows Firewall - Microsoft)

- Turn off Firewall State for: `Domain Profile`

- Turn off Firewall State for: `Public Profile`

- Select: `Apply`

- Click: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/20756d48-363a-4843-b127-4bf9771d4d27">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/a65dccf4-00e9-4e94-ab20-5e83acf7e253">

***

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4c4cf756-bea9-4efa-9e52-47a79595a532">

***

### 4. ) Create Client-1 VM

<ins>So similar to creating you domain controller, here are your Client-1 Configurations</ins>:

- Resource Group: `Active-Directory-Lab` (Same as your Domain Controller: DC-1)

- Virtual Machine Name: `Client-1`

- Region: `East US` (Same as your Domain Controller: DC-1)

- Image: `Windows 10 Pro, version 22H2 - x64 Gen2`

- Size: `Standard_D2s_v3 - 2 vcpus, 8 GiB memory ($70.08/month)` (Same as your Domain Controller: DC-1)

- Username: `Use the same one you used for you Domain Controller` for simplicity sake

- Password: `Use the same one you used for you Domain Controller` for simplicity sake

- Check: `The Licensing Boxe` at the bottom

- Go To: `Networking` Tab

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/50418389-0e5b-4a63-8ffe-49a7ecc71bee">

***

### 4.A ) Create Client-1 VM

<ins>Within you Network Tab</ins>:

- Choose the `Same Virtual Network and Subnet` as your Domain Controller
   
    - *This part is crucial. If your Client-1 VM and Domain Controller are not on the same VNET and Subnet they will be unable to communicate, preventing domain-related operations like joining the domain or authenticating.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/0c987656-62e0-4b28-9597-38c5b973ee10">

***

### 5. ) Set Client-1’s DNS settings to DC-1’s Private IP address

So now we will Set Client-1’s DNS settings to DC-1’s Private IP address, which will allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).

<ins>Follow this path to achieve this step</ins>:

- Resource Group > Client-1 > Network Settings > `Network Interface (client-160_z1)` > `DNS servers`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d3608405-a2aa-406f-a1c9-1ac73fed7407">

***

### 5.A ) Set Client-1’s DNS settings to DC-1’s Private IP address

<ins>Follow this path to achieve this step</ins>:

- Resource Group > Client-1 > Network Settings > Network Interface (client-160_z1) > `DNS servers`

- Select: `Custom`

- Input: `DC-1's Private IP Address` (Example; mine is: 10.0.0.4)

- Click: `Save` when done

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/daedca2e-0620-4b32-a1a1-15777b0be394">

***

### 5.B ) Set Client-1’s DNS settings to DC-1’s Private IP address

<ins>Now for the DNS Settings to sync in you must restart you Client-1's VM so</ins>:

- Go To: `Resource Group` > `Client-1`

- Restart: `Client-1` VM when done doing this

   - *If you do not restart your Client-1 VM after setting it's DNS Server to DC-1’s Private IP address then it will not successfully allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).*

<img width="800" alt="isolated" src="forgot to put image">

***

### 6. ) Attempt to Ping DC-1’s Private IP address from Client-1 VM

- If everything is setup correctly so far then you should have a successful ping from Client-1 to DC-1.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/a5df7280-47a1-42a6-bc1d-4fb45c6ed47b">

***

### 7. ) From Client-1 VM Open PowerShell and Run: ipconfig /all

- The output for the DNS settings under `DNS Server` should show DC-1’s Private IP Address as shown in the image below.

  - If the DNS server on Client-1 is not set to DC-1's private IP (e.g., it shows 168.63.129.16), update the DNS settings manually to DC-1's private IP. Restarting Client-1's VM may also help apply the changes. If successful, the client may log you out, indicating it's trying to connect to the domain.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d1c6ac2c-a8bb-4e53-a70e-26d6704a4853">

***

</details>

<details>

<summary>

## ⚙️ Part 2: Deploying Active Directory and User Creation

</summary>

### 8. ) Install Active Directory

<img width="800" alt="isolated" src="">

***

### 9. ) Create a Domain Admin User within the Domain D-1 VM

<img width="800" alt="isolated" src="">

***

### 10. ) Join Client-1 to Domain Controller

<img width="800" alt="isolated" src="">

***

### 11. ) Setup Remote Desktop for Non-Administrative Users on CLient-1 VM

<img width="800" alt="isolated" src="">

***

### 12. ) Attempt to log into Client-1 with one of the Created Users

<img width="800" alt="isolated" src="">

***

### 13. ) 

<img width="800" alt="isolated" src="">

***

</details>
