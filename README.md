<br>

<p align="center">
<img width="700" src="https://github.com/user-attachments/assets/9b6b0a51-6411-4e01-96c5-1bb31e6fd986" alt="Microsoft Active Directory Logo"/>
<br>

<br>

<br>

<h1 align="center">Active Directory Deployment and Configuration</h1> 
<br>

<p align="center">
<img src="https://github.com/user-attachments/assets/5d4b8c68-d1a6-4a4e-93e3-d8b8602d9123" height="85%" width="85%" alt="9"/><br />
</p>
<br />

## Lab Overview

This lab builds on the previous one [here](https://github.com/vincentchachere/azure-on-prem-ad). It simulates an enterprise Active Directory setup in Azure, where you'll deploy and configure Active Directory, create groups and user accounts, then verify the credentials, authentication, and permissions by logging into a client VM with manually generated users. Key topics include AD installation, forest creation, user management, domain integration, and custom Remote Desktop access, providing a strong foundation for future projects.

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

## High-Level Active Directory Deployment and Configuration Steps

- Install Active Directory
- Promote DC-1 to Domain Controller
- Create an Admin in Acitive Directory
- Join Client-1 to Domain
- Setup Remote Desktop for Non-Administrative Users

## Configuration Steps

<details>

<summary>

### 1. ) Install Active Directory

</summary>

<ins>Go to the Domain Controller (DC-1) and in Server Manager Dashboard</ins>:

- Click: `Add roles and features`

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

</details>

<details>

<summary>

### 2. ) Promote DC-1 to Domain Controller

</summary>

<ins>Towards the top-right corner of the Server Manager window, there will be a flag and a yellow triangle ⚠️ symbol</ins>.

- Click: `Flag with Triangle`

- Click: `Promote this server to a domain controller`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/377ca48e-b056-46a5-8504-9afa07a31297">

<br>
<br>
<br>

<ins>Within the Deployment Configuration tab</ins>

- Select: `Add a new forest`

- Root domain name: `mydomain.com`

- Click: `Next`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/47cd8d56-987c-4a4e-b2b8-4f586e2f85e8">

<br>
<br>
<br>

<ins>Within the Domain Controller Options tab</ins>:

- Give it a DSRM password (*This is required but it will not be used in this tutorial*).

- Click: `Next`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/fedd52a0-48d0-4c63-8adb-0c824671e1ae">

<br>
<br>
<br>

<ins>Within the DNS Options tab</ins>:

- Uncheck: `Create DNS delegation`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/35e80bf5-5348-4c1e-94c9-68cfe5f88de2">

<br>
<br>
<br>

<ins>Click Next until you reach the Prerequisites Check tab then</ins>:

- Click: `Install`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8d940c8e-4b2a-49be-a7af-3c8ad8a840f3">

<br>
<br>
<br>

<ins>The DC-1 will now restart to complete its promotion to a Domain Controller</ins>.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/126fa108-df23-44f3-87ef-b1f58cfe7aea">

<br>
<br>
<br>

Now that your DC-1 VM is a domain controller, you need to decide how to log in: as a local user on your client VM or as a domain user on the domain controller. This means clarifying two things: which domain to use and which user account to use.

- For this lab you can log back into DC-1 as:

  - Username: `mydomain.com\labuser` (or whatever you made when creating your DC-1 VM)

  - Password: `TheSamePasswordYou'veBeenUsing`

*Make sure to use a backslash ( \ ) NOT a forward slash ( / ) or you will not be able to login.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8489277b-b15e-4ebc-947d-11c5a1988046">

</details>

<details>

<summary>

### 3. ) Create an Admin in Active Directory

</summary>

Now that you're logged back into DC-1 as local user: domain.com\labuser, create two organizational units called _EMPLOYEES and _ADMINS, then add a new domain admin user. Mine will be named Jane Doe (You can name yours the same or something different, just remember it).

- Go To: `Active Directory Users and Computers` (ADUC)

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/190b5ddf-a472-4fc6-8195-819cab80ada6">

<br>
<br>
<br>

<ins>Within Active Directory Users and Computers</ins>:

Create an Organizational Unit (OU) called “_EMPLOYEES”

- Right-Click: `domain.com` > Select: `New` > Select: `Organizational Unit`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eb3f4120-b273-4901-b111-75081f98ea10">

<br>
<br>
<br>

<ins>Within New Object - Organzational Unit</ins>:

- Name: `_EMPLOYEES`

*Make sure to spell it exactly as you see it or the scripts and policies referencing it may fail.*

<ins>Remember to Create the ADMINS Organizational Unit</ins>:

- Right-Click: `domain.com` > Select: `New` > Select: `Organizational Unit`

- Name: `_ADMINS`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/20ee7889-bf7a-4925-8d71-23711ed14f00">

<br>
<br>
<br>

<ins>Back in Active Directory Users and Computers Create Your Domain User</ins>:

- Click: `_ADMINS` > Right-Click: `the empty space` > Select: `New` > Select: `User`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bb799a7b-360b-455b-a480-f1b35549135f">

<br>
<br>
<br>

<ins>Within New Object - User</ins>:

- First Name: `Jane`

- Last Name: `Doe`

- Change User Logon Name To: `jane_admin` (The first name of your created domain user then underscore admin.)

- Click: `Next`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/29c7bceb-57a0-48d9-9763-6776b87e65a5">

<br>
<br>
<br>

<ins>Within New Object - User</ins>:

- Password: `SomethingYouCanRemember`

- Check: `Password never expires` (Normally you do not want to do this, but for the simplicity of the lab we will.)

- Click: `Next`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f22c31e6-776a-4ad0-8c55-5a9185b45e4b">

<br>
<br>
<br>

<ins>Within New Object - User</ins>:

- Click: `Finish`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/dcb94011-6685-4c85-b501-7a0834d0b1d1">

<br>
<br>
<br>

<ins>Back in Active Directory Users and Computers</ins>

- Click: `_ADMINS`

- Right-Click: `Jane Doe` (The Name of Your Domain User)

- Select: `Properties`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d4b8977b-e96e-4da4-b683-4a8115ea2fb7">

<br>
<br>
<br>

<ins>Within Jane Doe Properties</ins>:

- Go To: `Member Of`

- Select: `Add`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/082a8038-dd74-407b-99f1-ba4c43210405">

<br>
<br>
<br>

<ins>Within the Select Groups Windows</in>:

- Type In: `Domain Admins`

- Click: `Check Names`

- Click: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c3fd5780-1fba-4847-9f73-89502b69ac53">

<br>
<br>
<br>

<ins>Back in Jane Doe Properties</ins>:

- Click: `Apply`

- Click: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/06db3f2f-a19e-4baf-adc4-22aa750bef4a">

<br>
<br>
<br>

<ins>Now you can log out / close the connection to DC-1 and</ins>:

- Log back into DC-1 as: `mydomain.com\jane_admin` (the first name of your domain user then underscore admin with mydomain.com attached to the front)

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3c0508eb-56f6-4de5-9938-89324722b547">

<br>
<br>
<br>

<ins>Back inside DC-1 as: mydomain.com\jane_admin</ins>:

</ins>Create another Organizational Unit called CLIENTS</ins>:

- Name: `_CLIENTS`

*We will place Client-1 inside this folder for organizational purposes.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9738fa42-b995-4983-b302-d9a869bc0711">

<br>
<br>
<br>

<ins>Within Active Directory Users and Computers</ins>:

- Click: `Computers`

- Right-Click: `Client-1`

- Select: `_CLIENTS`

- Click: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/94dc4c1b-eba9-4594-9429-bb83859bc9a8">

<br>
<br>
<br>

<ins>Within Active Directory Users and Computers</ins>:

Click: `_CLIENTS` and you will see Client-1 inside there

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f06306b1-27cd-4a4e-878e-fd77c3c1cfb7">

<br>
<br>
<br>

</details>

<details>

<summary>

### 4. ) Join Client-1 to Domain

</summary>

To join Client-1 to the Domain Controller we will first set Client-1’s DNS servers settings to DC-1’s Private IP address, which will allow the client-1 VM to resolve domain-related DNS queries through the Domain Dontroller (DC-1).

<ins>Go To</ins>:

- Resource Group: `Active-Directory-Lab` > VM: `Client-1` > `Network Settings` > Network Interface: `client-1408_z1` > `DNS servers`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2be62c15-ec58-4afe-8579-2e0bd3929243">

<br>
<br>
<br>

<ins>Setting Client's DNS servers to DC-1's Private IP Address Go To</ins>:

- Resource Group: `Active-Directory-Lab` > VM: `Client-1` > `Network Settings` > Network Interface: `client-1408_z1` > `DNS servers`

- Select: `Custom`

- Input: `DC-1's Private IP Address` (Example; mine is: 10.0.0.4)

- Click: `Save` when done

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c0e0d75a-6a25-4fd1-9c76-d50539b68c97">

<br>
<br>
<br>

<ins>Now for the DNS Settings to sync in you must restart you Client-1's VM so</ins>:

- Resource Group: `Active-Directory-Lab` > VM: `Client-1`

- Restart: `Client-1` VM when done doing this

*If you do not restart your Client-1 VM after setting it's DNS Server to DC-1’s Private IP address then it will not successfully allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/b5c26b71-a822-46b3-a7ca-7fe04ead9877">

<br>
<br>
<br>

<ins>Log into Client-1 as the original local admin (labuser) and</ins>:

- Open: `PowerShell`

- Run: `ipconfig /all`

- The `DNS Server` should show DC-1’s Private IP Address as shown in the image below.

*If the DNS server on Client-1 is not set to DC-1's private IP (e.g., it shows 168.63.129.16), update the DNS settings manually to DC-1's private IP. Restarting Client-1's VM may also help apply the changes. If successful, the client may log you out, indicating it's trying to connect to the domain.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8641ddec-9a6c-4919-b3d6-cbd0f6bec77e">

<br>
<br>
<br>

<ins>While still in Client-1 join it to the domain</ins>:

- Right-Click: `Start`

- Select: `System`

*The Client-1 VM will restart when it joins the domain.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9cc81c81-20c3-417f-8f4a-9231cb209170">

<br>
<br>
<br>

<ins>Within System Settings</ins>:

- Select: `Rename this PC (advanced)`

- Select: `Change`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6e459538-9285-4016-8026-a3510456a931">

<br>
<br>
<br>

<ins>Within Computeer Name/Domain Changes</ins>:

- Select: `Domain`

- Type In: `mydomain.com`

- Click: `OK`

*Notice Computer Name is Client-1 not DC-1*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d2d79b1f-1cbb-4340-a5a1-de3e73524f98">

<br>
<br>
<br>

<ins>When the Windows Security Window Pops Up</ins>:

- Type In: `mydomain.com\jane_admin`

- Password: `WhateverYouCreated`

- Select: `OK`

*Client-1 VM will restart after this*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eadeed84-5714-431a-b95d-8e8cb841cf57">

<br>
<br>
<br>

<ins>When this pops up</ins>:

Select: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/614eb7b1-1cad-441a-8e16-65f15b690e7f">

<br>
<br>
<br>

<ins>When this pops up</ins>:

- Select: `Restart Now`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6bbc2ad2-a1a6-45ef-952f-1a19405cab2b">

</details>

<details>

<summary>

### 5. ) Setup Remote Desktop for Non-Administrative Users

</summary>

<ins>Log back into Client-1 as: mydomain.com\jane_admin then</ins>:

- Go To: `System Settings`

- Select: `About`

- Select: `Remote Desktop`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/0e2f3ecd-a2ed-4441-ac67-47a135955174">

<br>
<br>
<br>

<ins>Within Remote Desktop System Settings</ins>:

- Select: `Select users that can remotely access this PC`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/de0fa8ee-0fc9-46a3-9338-ac1611dd47c8">

<br>
<br>
<br>

<ins>Within Select Users or Groups</ins>:

- Type In: `Domain Users`

- Click: `Check Names`

- Click: `OK`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/45f64722-e8f2-4651-93d3-be3d2a5beae2">

<br>
<br>
<br>

<ins>Log back into the Domain Controller (DC-1) as</ins>: mydomain.com\jane_admin

- Search: `Powershell ISE`

- Right-Click: `Powershell ISE`

- Select: `Run as Administrator`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/5066bc80-075f-44cb-b81d-e999f764a6ef">

<br>
<br>
<br>

<ins>In the top right corner of PowerShell ISE</ins>:

Click: the `Script` drop down arrow

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4609810b-6178-45ba-8552-9eaadc3eafe5">

<br>
<br>
<br>

<ins>Within PowerShell ISE</ins>:

*Copy, Paste, then Run the script below into PowerShell ISE:*

https://github.com/vincentchachere/Generate-Names-Create-Users/blob/main/AD

*Observe the accounts being created*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/7eabe6e2-48ab-463c-80ad-0e900b95e504">

<br>
<br>
<br>

### 13. ) Attempt to log into Client-1 with one of the Created Users

When finished, open Active Directory Users and Computers (ADUC) and observe the accounts in the _EMPLOYEES Oganizational Unit (OU).

- Select: `A Created User`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/ecff73ad-7aa5-489f-a36c-f539dc699328">

<br>
<br>
<br>

<ins>Attempt to log into Client-1 with one of the accounts</ins>

*Take note of the password in the script*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2a0a8d88-09f1-43d6-8be6-69b43dc97a16">

<br>
<br>
<br>

<ins>Once logged in</ins>:

- Go To: `Command Prompt`

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/63b0c4aa-986d-42cc-888e-9fe67fd1a9b0">

<br>
<br>
<br>

<ins>Once inside Command Prompt</ins>:

*Notice the created user's name displayed in the path.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bbb2ff68-8c58-4936-a0ff-503cb2584124">

<br>
<br>
<br>

<ins>Navigate to File Explorer</ins>:

- Go to: `This PC` > `Windows (C:)` > `Users`

*Take notice that the created user's name is shown in here because you logged into the Client VM with that account.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/19c207bd-c074-4492-a880-a1c43276c21c">

</details>

<h2 align="center">Final Thoughts</h2>

With the Active Directory infrastructure now fully set up, deployed, and configured, we've established a solid foundation for centralized domain management. We covered installing and promoting a Domain Controller, creating users, groups, and organizational units, setting up DNS, and testing connectivity between client machines and the server. This infrastructure enables streamlined management, enhanced security, and scalability for future needs. Always document your setup for reference and maintain regular backups to ensure system reliability.

If you'd like to continue building off this lab the next tuorial within this Active Directories Series is [here](https://github.com/vincentchachere/Group-Policy-and-Managing-Accounts/edit/main/README.md). This is where we'll practice account lockout, account recovery, group policy creation, password resets, enabling/disabling accounts, and event log observation.

Thank you for following along with this project. Your time and effort in learning and implementing these concepts are greatly appreciated. 

☎️ For any questions or just to connect you can reach me at: www.linkedin.com/in/vincentchachere
