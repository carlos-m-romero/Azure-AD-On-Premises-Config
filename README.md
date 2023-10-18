# On-premises Active Directory Deployment in Azure Virtual Machines

![Microsoft Active Directory Logo](https://i.imgur.com/pU5A58S.png)

This tutorial provides a guide on how to deploy on-premises Active Directory within Azure Virtual Machines. The aim is to simplify the process and make it as easy to use as possible. We will focus on setting up Active Directory Domain Services (AD DS) on a Windows Server 2022 machine and joining a Windows 10 client to the domain. Additionally, we will cover creating user accounts, enabling remote desktop access, and other related tasks.  

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)


## High-Level Deployment and Configuration Steps

1. **Setup Resources in Azure**

   - Create a Resource Group and Virtual Network (VNet) in Azure.
   - Create a Windows Server 2022 Virtual Machine (VM) named "DC-1" in the Resource Group and VNet.
   - Set the NIC Private IP address of the DC-1 VM to static.
   - Create a Windows 10 VM named "Client-1" in the same Resource Group and VNet.


![1](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/49877388-55e4-4de8-bc75-18a787e0852b)


2. **Ensure Connectivity between the Client and Domain Controller**

   - Connect to Client-1 using Remote Desktop and ping DC-1's private IP address.
   - If the ping fails, login to DC-1 and enable ICMPv4 in the local Windows Firewall.
![2](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/c950f42d-d2a6-41c9-bda4-7babd363f7dc)
![3](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/10adf0f3-4a4f-45e1-b141-f53bafd66dbc)


   - Verify that the ping from Client-1 to DC-1 succeeds.

![4](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/d03a552b-1b29-48d9-9a04-783cb7eb811d)

3. **Install Active Directory**

   - Log in to DC-1 and install Active Directory Domain Services.
   - Promote DC-1 as a domain controller and set up a new forest with a domain name (e.g., mydomain.com).

![5](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/9babefe2-e163-48f4-8b32-1cc61dee236f)

![6](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/8bcae04b-b0d1-453e-95bd-75dcc9e4f8ed)


![7](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/837f1059-21e1-4abf-9c13-19223ad5b063)



4. **Create an Admin and Normal User Account in AD**

   - Open Active Directory Users and Computers (ADUC) on DC-1.
   - Create an Organizational Unit (OU) called "_EMPLOYEES" and "_ADMINS" within ADUC.
   - Create a new employee account named "Jane Doe" with the username "jane_admin" and add her to the "Domain Admins" Security Group.
   - Log out and log back into DC-1 as "mydomain.com\jane_admin".


![8](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/2bee3017-8a84-42e6-a0d5-a847ab242b74)


5. **Join Client-1 to the Domain**

   - Set Client-1's DNS settings to the private IP address of DC-1.

![9](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/d6fc69c1-3411-47cd-9516-9fc0096dddb9)


   - Restart Client-1 and log in as the local admin user.
   - Join Client-1 to the domain and wait for the machine to restart.


![10](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/06e929bc-4f09-4804-a686-681971d1e159)

     
   - Log in to DC-1 and verify that Client-1 appears in ADUC under the "Computers" container.
   - Create a new OU named "_CLIENTS" and move Client-1 into it.


![11](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/797bf930-299e-4281-ade3-45e717abfbc5)


5. **Setup Remote Desktop for Non-Administrative Users on Client-1**

   - Log in to Client-1 as "mydomain.com\jane_admin" and open system properties.
   - Click on "Remote Desktop" and allow "domain users" access to Remote Desktop.
     

![12](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/3fe7e040-edb6-4a0a-aff8-2b7795a3188f)

  - Non-administrative users can now log in to Client-1 using Remote Desktop.

5. **Create Additional User Accounts and Test Login**

   - Log in to DC-1 as "mydomain.com\jane_admin".
   - Open PowerShell ISE as an administrator and run the provided PowerShell script to create additional user accounts.

   ![13](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/b7af223e-f8f2-407e-9664-aad06f31a2a4)

   - Verify that the accounts are created in the appropriate OU within ADUC.


![14](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/11bc5f95-3285-479d-84c5-3b292e2aa7b7)

     
   - Attempt to log in to Client-1 using one of the newly created user accounts.

![15](https://github.com/carlos-m-romero/Azure-AD-On-Premises-Config/assets/148396073/83961d2d-1480-4ee5-b7d3-185726d46cb9)



By following these steps, you will successfully configure on-premises Active Directory within Azure Virtual Machines and establish a domain environment for user and resource management.

## Summary

This project involved the setup and configuration of on-premises Active Directory within Azure Virtual Machines. We created a Windows Server 2022 VM as a domain controller and a Windows 10 VM as a client. The deployment process included configuring network settings, ensuring connectivity, installing Active Directory, creating user accounts, joining the client to the domain, enabling Remote Desktop access, and testing user logins.
