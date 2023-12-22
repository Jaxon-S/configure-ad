<img width="692" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/1623a8dd-6086-4e84-86fe-ac672c1b3975">


<h1>Configuring Active Directory with Azure VMs</h1>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory
- File Permissions

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>Setup Resources in Azure</h2>

- Create the Domain Controller VM(named "DC-1")
- Set the Domain Controller's NIC Private IP adress to be static instead of dynaamic
- Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group
 and Vnet that was created in Step 1.a
- Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)


<h2>Ensure Connectivity between the client and Domain Controller</h2>

<p>
<img width="684" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/6ecc082d-a05c-46f7-b311-bcdec82b50fe">

</p>
<p>
<h2>Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). Then, login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. Check back at Client-1 to see the ping succeed
 
</p>
<br />

<h2>Install Active Directory
 
<p>
<img width="529" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/c78b557b-3b2e-4337-8a11-486a1488954d">
<img width="312" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/a0aa1d4e-d689-4061-87d3-7c8dc22c083c">

</p>
<p>
Login to DC-1 and install Active Directory Domain Services, then promote into a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser


</p>
<br />
Create an Admin and Normal User Account in AD

<p>
<img width="304" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/2f9e6846-20df-4d94-89ac-f599efceff22">
<img width="548" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/05e6183c-0392-4fe6-9b80-e909104af553">
<img width="316" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/9f8a8b9d-525f-4ec3-bbc0-81a037759314">


</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. Create a new OU named “_ADMINS”. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”. Add jane_admin to the “Domain Admins” Security Group. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”. Now User jane_admin is the admin account

</p>
<br />
Join Client-1 to your domain (mydomain.com)

<p>
<img width="507" alt="image" src="https://github.com/Jaxon-S/configure-ad/assets/154096378/8f11b3a3-25ee-4434-a475-a157011b91af">

</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. Again, from the Azure Portal, restart Client-1. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.

</p>
<br />

Setup Remote Desktop for non-administrative users on Client-1
<p></p>


Log into Client-1 as mydomain.com\jane_admin and open system properties. Click “Remote Desktop”, Allow “domain users” access to remote desktop. You can now log into Client-1 as a normal, non-administrative user.
