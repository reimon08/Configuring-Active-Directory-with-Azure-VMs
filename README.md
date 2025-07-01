<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure)</h1>
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

- 🏗️ Install Active Directory Domain Services (AD DS) on dc-1
- 👤 Create a Domain Admin user within your new domain
- 🌐 Join client-1 to your domain (e.g., mydomain.com)
- 🖥️ Set up Remote Desktop access for non-administrative users on client-1
- 👥 Create multiple additional users within the domain
- 🔐 Log into client-1 using one of the new user accounts to test access


<h2>Install Active Directory</h2>

<p>First off, we’re gonna install Active Directory Domain Services (AD DS) on dc-1. This will let us turn our server into a domain controller so we can manage users, devices, and the whole domain environment.</p>

<p>To install it, go to the Start menu and open Server Manager. From there, click “Add roles and features”, then scroll down and select Active Directory Domain Services. When prompted, click “Add Features”, then just keep clicking Next through the rest of the steps and hit Install.</p>


https://github.com/user-attachments/assets/378223ce-0b95-475f-b16c-a46eef2f081b

<h2>Promote as a Domain Controller</h2>

<p>Now we’re gonna promote dc-1 to an actual domain controller by setting up a new forest with the domain name mydomain.com. This basically makes our server the boss of the domain, managing all the users and devices under it.</p>

<p>When you get to the part where it asks for the Directory Services Restore Mode (DSRM) password, enter Password1. We’re not really gonna use this for this project, but it’s required to move forward, so just set it and continue.</p>


https://github.com/user-attachments/assets/054cccf5-8a20-4607-9f9d-0f0cc0e51ccb

<p>Once the promotion and installation are done, dc-1 will automatically restart. That’s normal, it’s just finalizing the setup and locking in the domain controller role.</p>

<img width="1710" alt="Screenshot 2025-07-01 at 10 18 40 AM" src="https://github.com/user-attachments/assets/49f908d9-9aae-4fbc-a4e4-b4cbb915fb93" />

<p>Now that this virtual machine is a domain controller, everything is part of the domain. So when we log in, we need to include the domain name like this: mydomain.com\labuser.</p>

<img width="1171" alt="Screenshot 2025-07-01 at 10 33 25 AM" src="https://github.com/user-attachments/assets/228d913a-95ef-4e75-9e30-f4a630af21df" />









