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

<h2>Create a Domain Admin user within the domain</h2>

<p>Now in Active Directory Users and Computers (ADUC), we’re gonna create two Organizational Units (OUs), one called _EMPLOYEES and another called _ADMINS. These will help us stay organized by separating regular users from admin accounts.</p>

<p>Go to:
Start Menu ➝ Windows Administrative Tools ➝ Active Directory Users and Computers ➝ Click mydomain.com ➝ Right-click ➝ New ➝ Organizational Unit ➝ Create _EMPLOYEES and _ADMINS</p>


https://github.com/user-attachments/assets/03ba4904-d5fd-4fca-aff0-597f7922ee52

<p>Now we’re gonna create a new user named “Jane Doe” with the username jane_admin and the password Cyberlab123! — same one we’ve been using to keep it simple. This account is gonna go under the _ADMINS OU.</p>

<p>To create the user, go to:
_ADMINS ➝ Right-click ➝ New ➝ User

Then fill in the details:

Full Name: Jane Doe

Username: jane_admin

Password: Cyberlab123!
Make sure to uncheck the option that forces the user to change the password at next logon (since this is just a project setup).</p>


https://github.com/user-attachments/assets/303cc3e0-b701-4066-9b82-36e36d32f43b

<p>Now we’re gonna add jane_admin to the "Domain Admins" security group so she has full administrative privileges across the domain.

To do that:
_ADMINS ➝ Right-click jane_admin ➝ Properties ➝ Member Of tab ➝ Add ➝ Type Domain Admins ➝ Hit OK</p>


https://github.com/user-attachments/assets/9c32d21a-74fd-4ff5-a2ea-eae7c5f69962

<p>Now go ahead and log out or close the connection to dc-1, then log back in using mydomain.com\jane_admin. From this point on, we’ll be using jane_admin as our main admin account for the rest of the setup.</p>


https://github.com/user-attachments/assets/5ef3003d-08ce-4603-9d3d-710483c9ea90

<h2>Join Client-1 to your domain (mydomain.com)</h2>

<p>Now log in to client-1 using the original local admin account (labuser), and then join the machine to the domain. Once you do that, the computer will automatically restart to apply the changes.</p>

<img width="1167" alt="Screenshot 2025-07-01 at 11 23 01 AM" src="https://github.com/user-attachments/assets/43ee60bb-ded0-4f92-b0b8-31c50f35bd63" />

<p>To join client-1 to the domain:
Right-click the Start menu ➝ Click System ➝ On the right side, click “Rename this PC (advanced)” ➝ Under the Computer Name tab, click Change ➝ In the Member of section, select Domain, then enter mydomain.com ➝ Hit OK and enter domain credentials when prompted.</p>

<p>Since we set Client-1’s DNS settings to use dc-1’s private IP address, it’s able to locate the domain controller for mydomain.com. Without that, it wouldn’t know where to find the domain.</p>

https://github.com/user-attachments/assets/4dbaa1e4-19f4-4c20-b940-b43310d023b3

<p>After entering the domain and confirming, you’ll see a Welcome tab pop up, that means it successfully found the domain. It’ll then prompt you to restart, so just go ahead and let it reboot to finish joining the domain.</p>

<img width="1167" alt="client-1 mydomain" src="https://github.com/user-attachments/assets/5c4cfe2e-2895-4325-bb15-e962f390c024" />

<p>Now log back into the Domain Controller and open Active Directory Users and Computers (ADUC). In there, you should see Client-1 listed under "Computers", which means it successfully joined the domain.

Next, go ahead and create a new Organizational Unit (OU) called _CLIENTS, then just drag Client-1 into that OU to keep things organized.</p>


https://github.com/user-attachments/assets/b0d42afe-7cd1-4a36-a65a-13ed2c79b7ac

<h2>Setup Remote Desktop for non-administrative users on Client-1</h2>

<p>Now log into Client-1 using mydomain.com\jane_admin. Once you're in, open System Properties ➝ click on the “Remote Desktop” tab ➝ and then allow “Domain Users” access to Remote Desktop.</p>


https://github.com/user-attachments/assets/d1ad9717-5351-40ee-b05f-24cdf680cc88

<h2>Create additional users and attempt to log into client-1 with one of the users</h2>

<p>While logged in as jane_admin, open PowerShell ISE as an administrator. Create a new file, then paste the contents of the user creation script into it.

This script will create 10,000 employee user accounts and store them in the _EMPLOYEES Organizational Unit (OU).

### [User Creation Script](https://github.com/reimon08/Pwrshell/blob/main/Generate-Names-Create-Users.ps1)


Once it’s in, run the script and you’ll see the accounts being created in real time. When it’s done, open up Active Directory Users and Computers (ADUC) and you should see all the new accounts inside the _EMPLOYEES OU.</p>



https://github.com/user-attachments/assets/d1430af8-21c7-417c-bc64-cf7f015d038f

<p> If you take a look at ADUC and go to the _EMPLOYEES OU, you’ll see the users being added in real time as the script runs.</p>

<p>You can also stop the script whenever you feel like you’ve added enough users, just hit the Stop button in PowerShell ISE and it’ll pause right there.</p>



https://github.com/user-attachments/assets/a079d2d3-e047-4979-8c70-50b3426c8580

<h2>🧠 Foundation Set: AD Ready for GPO and More</h2>

<p>That concludes this project! Next, I’ll be using this setup for another project focused on Group Policy and account management. Thanks for following along, I hope this helped you get comfortable with setting up Active Directory in Azure.</p>


























