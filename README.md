<br>

<p align="center">
<img width="700" src="https://github.com/user-attachments/assets/9b6b0a51-6411-4e01-96c5-1bb31e6fd986" alt="Microsoft Active Directory Logo"/>
<br>

<br>

<br>

<h1 align="center">Active Directory Deployment and Configuration</h1> 

This is the second project in the [comprehensive series of tutorials](https://github.com/vincentchachere/azure-on-prem-ad) on Azure and Active Directory implementation. It simulates an enterprise Active Directory setup in Azure, where we'll deploy and configure Active Directory, create groups and user accounts, then verify the credentials, authentication, and permissions by logging into a client VM with manually generated users. Key topics include AD installation, forest creation, user management, domain integration, and custom Remote Desktop access, providing a strong foundation for future projects.

<p align="center">
<img src="https://github.com/user-attachments/assets/5d4b8c68-d1a6-4a4e-93e3-d8b8602d9123" height="85%" width="85%" alt="9"/><br />
</p>
<br />

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
- Create an Admin in Active Directory
- Join Client-1 to Domain
- Setup Remote Desktop for Non-Administrative Users

## Configuration Steps

<details>

<summary>

### 1. ) Install Active Directory

</summary>

Go to the **Domain Controller (DC-1)** and in the Server Manager Dashboard click **Add roles and features**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4109a1e0-694c-4404-9109-4c69f23ca2ce">

<br>
<br>
<br>

Click **Next** until reaching the Server Roles section then select **Active Directory Domain Services**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/612caea1-964c-4197-892d-fd3aa833c562">

<br>
<br>
<br>

<ins>Within this portion</ins>:

Click **Add Features**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3fa6a381-f5b0-4c94-bb48-794bea14b10b">

<br>
<br>
<br>

<ins>Click Next until reaching the Confirmation tab then</ins>:

Check the **Restart the destination server automatically if required** box, click **Yes**, then **Install**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/81812bf1-e8b9-48f7-9507-fa4074af53c4">

<br>
<br>
<br>

<ins>When that's done installing</ins>:

Click **Close**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/7c73b736-cf0e-4545-a6d9-683b9ecf90ea">

</details>

<details>

<summary>

### 2. ) Promote DC-1 to Domain Controller

</summary>

Towards the top-right corner of the Server Manager window, there will be a flag and a yellow triangle ⚠️ symbol. Click the **Flag with Triangle** and **Promote this server to a domain controller**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/377ca48e-b056-46a5-8504-9afa07a31297">

<br>
<br>
<br>

<ins>Within the Deployment Configuration tab</ins>

Select **Add a new forest**, name the Root domain **mydomain.com**, and click **Next**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/47cd8d56-987c-4a4e-b2b8-4f586e2f85e8">

<br>
<br>
<br>

<ins>Within the Domain Controller Options tab</ins>:

Give it a DSRM password (*This is required but it will not be used in this tutorial*).

Click **Next**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/fedd52a0-48d0-4c63-8adb-0c824671e1ae">

<br>
<br>
<br>

<ins>Within the DNS Options tab</ins>:

Uncheck **Create DNS delegation** and click **Next**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/35e80bf5-5348-4c1e-94c9-68cfe5f88de2">

<br>
<br>
<br>

<ins>Click Next until you reach the Prerequisites Check tab then</ins>:

Click **Install**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8d940c8e-4b2a-49be-a7af-3c8ad8a840f3">

<br>
<br>
<br>

The DC-1 will now restart to complete its promotion to a Domain Controller.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/126fa108-df23-44f3-87ef-b1f58cfe7aea">

<br>
<br>
<br>

Now that DC-1 is officially a domain controller, we'll need to decide how to log in: as a **local user** on Client-1 or as a **domain user** on the domain controller. This means clarifying two things: **which domain** to use and **which user account** to use.

For this lab, we'll log back into DC-1 as a local user with the username **mydomain.com\labuser** (or whatever you made when creating your Domain Controller (DC-1)) and enter the password you created.

*Make sure to use a backslash ( \ ) NOT a forward slash ( / ) or you will not be able to login.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8489277b-b15e-4ebc-947d-11c5a1988046">

</details>

<details>

<summary>

### 3. ) Create an Admin in Active Directory

</summary>

Now that we're logged back into DC-1 as local user: **domain.com\labuser**, create **two organizational units** called **_EMPLOYEES** and **_ADMINS**, then **add** a **new domain admin user**. Mine will be named Jane Doe (You can name yours the same or something different, just remember it).

After all that, go to **Active Directory Users and Computers (ADUC)**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/190b5ddf-a472-4fc6-8195-819cab80ada6">

<br>
<br>
<br>

Once inside Active Directory Users and Computers (ADUC) create the first Organizational Unit (OU) called **_EMPLOYEES**. So to do this right-click **domain.com**, click **New**, and select **Organizational Unit**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eb3f4120-b273-4901-b111-75081f98ea10">

<br>
<br>
<br>

Within the New Object - Organzational Unit window name organizational unit **_EMPLOYEES**.

*Make sure to spell it exactly as you see it or the scripts and policies referencing may fail.*

Create the second organizational unit the same way and name it **_ADMINS**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/20ee7889-bf7a-4925-8d71-23711ed14f00">

<br>
<br>
<br>

Back in Active Directory Users and Computers (ADUC) create your domain user. To do this click the **_ADMINS folder**, within the _ADMINS folder: right-click the **empty space**, select **New**, then select **User**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bb799a7b-360b-455b-a480-f1b35549135f">

<br>
<br>
<br>

Within the New Object - User window name your domain user's first name **Jane**, last name **Doe**, change user logon name to: **jane_admin** (*The first name of your created domain user then underscore admin.*), and click **Next**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3a4ba425-6ea3-4585-99a6-f9d296bf33aa">

<br>
<br>
<br>

Within the New Object - User window create a password you can remember (*You will use this to login with your domain admin user account - mydomain.com\jane_admin - from now on.*), check the **Password never expires** (*Normally you do not want to do this, but for the simplicity of the lab we will.*), and click **Next**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f22c31e6-776a-4ad0-8c55-5a9185b45e4b">

<br>
<br>
<br>

Click **Finish**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/dcb94011-6685-4c85-b501-7a0834d0b1d1">

<br>
<br>
<br>

Back in Active Directory Users and Computers (ADUC) click **_ADMINS**, right-click **Jane Doe** (your domain admin user account), and select **Properties**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d4b8977b-e96e-4da4-b683-4a8115ea2fb7">

<br>
<br>
<br>

Within the Jane Doe (domain admin user account) Properties go to the **Member Of** tab, then select **Add**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/082a8038-dd74-407b-99f1-ba4c43210405">

<br>
<br>
<br>

Within the Select Groups window input **Domain Admins**, click **Check Names**, and click **OK**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c3fd5780-1fba-4847-9f73-89502b69ac53">

<br>
<br>
<br>

Back in the Jane Doe (domain admin user account) Properties click **Apply** and click **OK**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/06db3f2f-a19e-4baf-adc4-22aa750bef4a">

<br>
<br>
<br>

Log out / close the connection to DC-1 and **log back** in **with** the **new domain admin user account** (mydomain.com\jane_admin) you just created.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/3c0508eb-56f6-4de5-9938-89324722b547">

</details>

<details>

<summary>

### 4. ) Join Client-1 to Domain

</summary>

To join Client-1 to the Domain Controller we will first **set Client-1’s DNS servers** settings to **DC-1’s Private IP address**, which will allow the client-1 VM to resolve domain-related DNS queries through the Domain Dontroller (DC-1).

To do this go to your resourse group **Active-Directory-Lab** > **Client-1** > **Network Settings** > **Network Interface** (client-1408_z1) > **DNS servers**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2be62c15-ec58-4afe-8579-2e0bd3929243">

<br>
<br>
<br>

<ins>Setting Client's DNS servers to DC-1's Private IP Address</ins>:

Within your DNS Servers dashboard select **Custom**, input **DC-1's Private IP Address** (10.0.0.4), click **save**, and wait for it to update.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/c0e0d75a-6a25-4fd1-9c76-d50539b68c97">

<br>
<br>
<br>

For the DNS settings to sync we'll need to **restart Client-1** in the Azure portal. So go to your resource group, select **Client-1**, and click **restart**.

*If you do not restart your Client-1 VM after setting it's DNS Server to DC-1’s Private IP address then it will not successfully allow the client-1 VM to resolve domain-related DNS queries through the domain controller (DC-1).*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/b5c26b71-a822-46b3-a7ca-7fe04ead9877">

<br>
<br>
<br>

**Login to Client-1** as the original local admin user (**labuser**), open **PowerShell**, and run the command **ipconfig /all** to verify DC-1's Private IP Address got synced to Client-1.

The **DNS Server** should show **DC-1’s Private IP Address** as shown in the image below.

*If the DNS server on Client-1 is not set to DC-1's private IP (e.g., it shows 168.63.129.16), update the DNS settings manually to DC-1's private IP. Restarting Client-1's VM may also help apply the changes. If successful, Client-1 may log you out, indicating it's trying to connect to the domain.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/8641ddec-9a6c-4919-b3d6-cbd0f6bec77e">

<br>
<br>
<br>

While still in Client-1 join it to the domain by right-clicking **Start** and selecting **System**.

*Client-1 will restart when it joins the domain.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9cc81c81-20c3-417f-8f4a-9231cb209170">

<br>
<br>
<br>

Within System settings select **Rename this PC (advanced)** and select **Change**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6e459538-9285-4016-8026-a3510456a931">

<br>
<br>
<br>

Select **Domain**, input **mydomain.com**, and click **OK**.

*Notice Computer Name is Client-1 not DC-1*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/d2d79b1f-1cbb-4340-a5a1-de3e73524f98">

<br>
<br>
<br>

Input the domain admin user accouunt you created: **mydomain.com\jane_admin**, input **Password**, and select **OK**.

*Client-1 VM will restart after this*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/eadeed84-5714-431a-b95d-8e8cb841cf57">

<br>
<br>
<br>

Select **OK**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/614eb7b1-1cad-441a-8e16-65f15b690e7f">

<br>
<br>
<br>

Select **Restart Now**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/6bbc2ad2-a1a6-45ef-952f-1a19405cab2b">

<br>
<br>
<br>

Log back into DC-1 as your domain admin user (mydomain.com\jane_admin) and verify Client-1 made it into ADUC. So open **ADUC**, expand **mydomain.com**, and select **Computers**.

*If you do not see Client-1 in there **Refresh** ADUC.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/a8f3ab63-8819-4bee-952e-a59362c0a329">

<br>
<br>
<br>

Create another Organizational Unit called **_CLIENTS**.

*We will place Client-1 inside this folder for organizational purposes.*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/9738fa42-b995-4983-b302-d9a869bc0711">

<br>
<br>
<br>

Within **ADUC** drap and drop **Client-1** into the **_CLIENTS** folder or open the **Computers** folder, right-click **Client-1**, select **Move**, select **_CLIENTS**, and click **OK**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/a1b031ec-9d4d-4fee-b5ad-f524b2b8e4e5">

<br>
<br>
<br>

Open the **_CLIENTS** folder and verify **Client-1** is in there.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/f06306b1-27cd-4a4e-878e-fd77c3c1cfb7">

</details>

<details>

<summary>

### 5. ) Setup Remote Desktop for Non-Administrative Users

</summary>

Log back into Client-1 as your domain admin user (mydomain.com\jane_admin) and open **System Settings**, select **About**, then select **Remote Desktop**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/0e2f3ecd-a2ed-4441-ac67-47a135955174">

<br>
<br>
<br>

Within the Remote Desktop System Settings select **Select users that can remotely access this PC**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/de0fa8ee-0fc9-46a3-9338-ac1611dd47c8">

<br>
<br>
<br>

Input **Domain Users**, click **Check Names**, and click **OK**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/45f64722-e8f2-4651-93d3-be3d2a5beae2">

<br>
<br>
<br>

Log back into DC-1 as your domain admin user (mydomain.com\jane_admin) and open **PowerShell ISE as an administrator**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/5066bc80-075f-44cb-b81d-e999f764a6ef">

<br>
<br>
<br>

In the top right corner of PowerShell ISE click the **Script** drop down arrow.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/4609810b-6178-45ba-8552-9eaadc3eafe5">

<br>
<br>
<br>

**Copy**, **Paste**, and **Run the script** that's in the link below into PowerShell ISE.

https://github.com/vincentchachere/Generate-Names-Create-Users/blob/main/AD

*Observe the accounts being created*

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/7eabe6e2-48ab-463c-80ad-0e900b95e504">

<br>
<br>
<br>

### 13. ) Attempt to log into Client-1 with one of the Created Users

When finished, open **ADUC** and **observe the generated accounts** in the **_EMPLOYEES** Oganizational Unit (OU).

Select **A Random Created User** (Example: bat.pac)

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/ecff73ad-7aa5-489f-a36c-f539dc699328">

<br>
<br>
<br>

**Login to Client-1** with one of the **generated user accounts** (mydomain.com\bat.pac).

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/2a0a8d88-09f1-43d6-8be6-69b43dc97a16">

<br>
<br>
<br>

Once logged in to Client-1 open **Command Prompt**.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/63b0c4aa-986d-42cc-888e-9fe67fd1a9b0">

<br>
<br>
<br>

Notice the created user's name (*bat.pac*) displayed in the path.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/bbb2ff68-8c58-4936-a0ff-503cb2584124">

<br>
<br>
<br>

Open **File Explorer** and follow the path: **This PC** > **Windows (C:)** > **Users**

You'll see the created user's name (*bat.pac*) shown in here since you logged into Client-1 with that account.

<img width="800" alt="isolated" src="https://github.com/user-attachments/assets/19c207bd-c074-4492-a880-a1c43276c21c">

</details>

<h2 align="center">Final Thoughts</h2>

With the Active Directory infrastructure now fully set up, deployed, and configured, we've established a solid foundation for centralized domain management. We covered installing and promoting a Domain Controller, creating users, groups, and organizational units, setting up DNS, and testing connectivity between client machines and the server. This infrastructure enables streamlined management, enhanced security, and scalability for future needs. Always document your setup for reference and maintain regular backups to ensure system reliability.

To continue the Active Directory series, explore the [Group Policy and Account Management](https://github.com/vincentchachere/Group-Policy-and-Managing-Accounts) or [Network File Shares and Permissions](https://github.com/vincentchachere/Network-File-Shares-and-Permissions) lab.

Thank you for following along with this project. Your time and effort in learning and implementing these concepts are greatly appreciated. 

☎️ For questions or to connect you can reach me at: www.linkedin.com/in/vincentchachere
