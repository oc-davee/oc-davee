<p align="center"> <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/> </p> <h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1> This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br /> <h2>Environments and Technologies Used</h2>
Microsoft Azure (Virtual Machines/Compute)
Remote Desktop<img width="483" height="498" alt="Screenshot 2026-08-27 215730" src="https://github.com/user-attachments/assets/17f510d9-3d17-49e0-8a6e-fe4591e6673e" />

Active Directory Domain Services
PowerShell
<h2>Operating Systems Used</h2>
Windows Server 2022
Windows 10 (21H2)
<h2>High-Level Deployment and Configuration Steps</h2>
Step 1 - Deploy a Windows Server 2022 VM (DC-1) and a Windows 10 VM (Client-1) in the same Azure Virtual Network
Step 2 - Install Active Directory Domain Services on DC-1 and promote it to a new forest/domain
Step 3 - Join Client-1 to the domain and manage users/OUs from Active Directory Users and Computers (ADUC)
Step 4 - Simulate real-world AD administration: bulk user creation, account lockout policy, account enable/disable, and log review
<h2>Deployment and Configuration Steps</h2>
Part 1
Setup Domain Controller in Azure
Log into the Azure Portal
Create a Resource Group

Azure Portal — creating the Resource Group
<img width="846" height="729" alt="Screenshot 2026-08-27 210352" src="https://github.com/user-attachments/assets/a102446d-5eda-424d-a6fa-e73feb2344e4" />


Create the Domain Controller VM (Windows Server 2022) named "DC-1"
Username: labuser
Password: Cyberlab123!
Place it in the Resource Group and VNet created default

📸 Screenshot placeholder: Azure Portal — DC-1 VM creation blade
<img width="2889" height="1083" alt="Screenshot 2026-08-27 210558" src="https://github.com/user-attachments/assets/94ce7583-21a5-4837-9bbe-2fef859c75ad" />


After the VM is created, set DC-1's NIC Private IP address to Static

📸 Screenshot placeholder: Azure Portal — DC-1 NIC settings, Static private IP

<img width="2850" height="1629" alt="Screenshot 2026-08-27 210642" src="https://github.com/user-attachments/assets/2be8530f-d5de-474a-af7e-7e0e24625143" />

Log into the VM (via RDP) and disable the Windows Firewall on all profiles (Domain/Private/Public) — for testing connectivity only

Setup Client-1 in Azure
Create the Client VM (Windows 10) named "Client-1"
Username: labuser
Password: Cyberlab123!

Attach it to the same region and Virtual Network as DC-1
After the VM is created, set Client-1's DNS settings (on the NIC) to DC-1's Private IP address 

📸 Screenshot placeholder: Azure Portal — Client-1 NIC DNS servers set to DC-1's private IP
<img width="2424" height="1779" alt="Screenshot 2026-08-27 210725" src="https://github.com/user-attachments/assets/5ffe2f0d-ce40-4e1b-a647-7e08f1665238" />


From the Azure Portal, restart Client-1
Login to Client-1
Attempt to ping DC-1's private IP address
Ensure the ping succeeded

📸 Screenshot placeholder: ping <DC-1 private IP> succeeding from Client-1
<img width="849" height="261" alt="Screenshot 2026-08-27 231500" src="https://github.com/user-attachments/assets/51690fa9-ac34-46af-8f49-1c2fa91b6904" />


From Client-1, open PowerShell and run ipconfig /all
The output for the DNS settings should show DC-1's Private IP Address

📸 Screenshot placeholder: ipconfig /all output showing DC-1 as DNS Server
<img width="1422" height="363" alt="Screenshot 2026-08-27 231528" src="https://github.com/user-attachments/assets/0b73e705-e7a1-4fbf-8391-e7b0bfe17920" />
<img width="2460" height="210" alt="Screenshot 2026-08-27 231549" src="https://github.com/user-attachments/assets/49461d1f-9136-4b14-951c-29706166bb5d" />


Install Active Directory
Login to DC-1 and install the Active Directory Domain Services role
Promote the server to a Domain Controller: setup a new forest named mydomain.com (can be anything — just remember what it is)

📸 Screenshot placeholder: AD DS installation wizard / "Deployment Configuration" screen
<img width="3303" height="1794" alt="Screenshot 2026-08-27 213655" src="https://github.com/user-attachments/assets/5651bcb8-b50f-45bb-b147-2fac98e0f7c6" />
<img width="1440" height="1038" alt="Screenshot 2026-08-27 213718" src="https://github.com/user-attachments/assets/c24dc9f5-40f3-49f6-ae34-91adb39b4f7c" />
<img width="3258" height="1896" alt="Screenshot 2026-08-27 213810" src="https://github.com/user-attachments/assets/c781f3d3-8c77-48b1-97bc-8437c91c8649" />
<img width="1632" height="1053" alt="Screenshot 2026-08-27 213835" src="https://github.com/user-attachments/assets/91b24eab-7435-4189-996d-7f2e7004ae46" />
<img width="2457" height="1398" alt="Screenshot 2026-08-27 213929" src="https://github.com/user-attachments/assets/93970887-2b9e-408c-8764-a839374f7245" />
<img width="1512" height="1083" alt="Screenshot 2026-08-27 213947" src="https://github.com/user-attachments/assets/16ef66ba-9067-4ba2-af40-3872d81495ad" />
<img width="1386" height="1029" alt="Screenshot 2026-08-27 214033" src="https://github.com/user-attachments/assets/b1526278-04a6-4d04-8e69-f24682dc7855" />



Restart, then log back into DC-1 as mydomain.com\labuser
Create a Domain Admin user within the domain
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called _EMPLOYEES
Create a new OU named _ADMINS

📸 Screenshot placeholder: ADUC showing the _EMPLOYEES and _ADMINS OUs
<img width="828" height="438" alt="Screenshot 2026-08-27 214209" src="https://github.com/user-attachments/assets/e7f5aed4-97d4-402a-8dbe-df95ff976084" />
<img width="1329" height="1155" alt="Screenshot 2026-08-27 214251" src="https://github.com/user-attachments/assets/dddf9318-83ca-42f2-a420-36f860115249" />
<img width="3141" height="1893" alt="Screenshot 2026-08-27 214325" src="https://github.com/user-attachments/assets/9a8eba39-49ed-46ac-849f-d4536c6cdc4a" />
<img width="2199" height="1461" alt="Screenshot 2026-08-27 214349" src="https://github.com/user-attachments/assets/414fe60b-40b1-47e3-b465-5455ba5b45f1" />
<img width="2370" height="1575" alt="Screenshot 2026-08-27 214403" src="https://github.com/user-attachments/assets/4a3d6aeb-4138-4b09-bc66-0c063672bb16" />


Create a new employee named "Jane Doe" (same password) with the username jane_admin / Cyberlab123!, placed in _ADMINS
Add jane_admin to the Domain Admins security group

📸 Screenshot placeholder: "Member Of" tab showing jane_admin in Domain Admins
<img width="1293" height="1107" alt="Screenshot 2026-08-27 214414" src="https://github.com/user-attachments/assets/354d21ae-5a56-4e9d-b649-f7bc52d47f1f" />
<img width="1896" height="1485" alt="Screenshot 2026-08-27 214451" src="https://github.com/user-attachments/assets/e61dfee9-b70b-4b6a-94d3-b4e3c07d4594" />
<img width="2580" height="1605" alt="Screenshot 2026-08-27 214516" src="https://github.com/user-attachments/assets/f76a8f46-29f7-4af8-a745-c7c7420a62fb" />
<img width="1086" height="1242" alt="Screenshot 2026-08-27 214616" src="https://github.com/user-attachments/assets/63837c66-f452-4c43-aae9-87e44dd7d434" />
<img width="1338" height="933" alt="Screenshot 2026-08-27 214630" src="https://github.com/user-attachments/assets/3f617aba-8dce-40dc-80cc-f2d4c589b9ee" />


Log out / close the RDP connection to DC-1 and log back in as mydomain.com\jane_admin
Use jane_admin as your admin account from now on
Join Client-1 to your domain (mydomain.com)
From the Azure Portal, confirm Client-1's DNS settings still point to the DC's Private IP address (already done)
From the Azure Portal, restart Client-1 (already done)
Login to Client-1 as the original local admin (labuser) and join it to the domain mydomain.com
The computer will restart

📸 Screenshot placeholder: System Properties → "Join a Domain" dialog on Client-1
<img width="2211" height="1728" alt="Screenshot 2026-08-27 214725" src="https://github.com/user-attachments/assets/664985ba-068f-4d75-b469-4fe8ac5d6fc1" />
<img width="777" height="882" alt="Screenshot 2026-08-27 214744" src="https://github.com/user-attachments/assets/14fe499e-951b-45aa-b960-9cfbd4b1700b" />
<img width="663" height="834" alt="Screenshot 2026-08-27 214758" src="https://github.com/user-attachments/assets/4a0a44e1-2ab9-40b1-8d82-e85c5ae7403d" />
<img width="1836" height="1668" alt="Screenshot 2026-08-27 214823" src="https://github.com/user-attachments/assets/bc31ce3e-c491-48c1-97b4-ca08fcbfde5c" />
<img width="843" height="570" alt="Screenshot 2026-08-27 214845" src="https://github.com/user-attachments/assets/2a4a115e-609e-4b5c-b44e-a71fa0485193" />
<img width="546" height="276" alt="Screenshot 2026-08-27 214859" src="https://github.com/user-attachments/assets/2dae1297-4eaf-40c2-96ff-8f3575b08b4b" />


Log into the Domain Controller and verify Client-1 shows up in ADUC (under the default "Computers" container)

📸 Screenshot placeholder: ADUC showing Client-1's computer object
<img width="2784" height="900" alt="Screenshot 2026-08-27 214924" src="https://github.com/user-attachments/assets/7bb106d8-2980-41a5-99bc-4371ec931adc" />


Part 2

Turn on the DC-1 and Client-1 VMs in the Azure Portal if they are off.

Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin
Open System Properties
Click the Remote Desktop tab
Allow "Domain Users" access to Remote Desktop

📸 Screenshot placeholder: Remote Desktop tab — "Select Users" showing Domain Users added
<img width="3195" height="1578" alt="Screenshot 2026-08-27 215046" src="https://github.com/user-attachments/assets/b1496877-8201-4a9e-a00d-2fbafe9abe11" />
<img width="2154" height="1662" alt="Screenshot 2026-08-27 215107" src="https://github.com/user-attachments/assets/5238990b-e09a-44ce-b3b7-b991e890ad89" />
<img width="1533" height="1626" alt="Screenshot 2026-08-27 215129" src="https://github.com/user-attachments/assets/4fae1500-8ed5-4608-9e9c-bd8a7f2215e6" />
<img width="1074" height="654" alt="Screenshot 2026-08-27 215155" src="https://github.com/user-attachments/assets/7bdb97a1-d768-4eab-b03f-75876f142bac" />


You can now log into Client-1 as a normal, non-administrative domain user

💡 Normally you'd want to do this via Group Policy, which lets you change this setting on many systems at once.
<img width="2163" height="855" alt="Screenshot 2026-08-27 215213" src="https://github.com/user-attachments/assets/bcb9d66f-20e6-4503-a374-902ae5eceb33" />


Create a bunch of additional users and log into Client-1 with one of them
Login to DC-1 as jane_admin
Open PowerShell ISE as an administrator
Create a new file and paste in the contents of Generate-Names-Create-Users.ps1
Run the script and observe the accounts being created in real time

📸 Screenshot placeholder: PowerShell ISE console output — "Creating user: ..." lines scrolling
<img width="1803" height="1038" alt="Screenshot 2026-08-27 215240" src="https://github.com/user-attachments/assets/4aaa6e09-69e5-4efb-8711-a22236c8f09a" />
<img width="1596" height="432" alt="Screenshot 2026-08-27 215307" src="https://github.com/user-attachments/assets/2d7308bb-7598-4535-9386-e2d4b1cce36f" />


powershell
   # ----- Edit these Variables for your own Use Case ----- #
   $PASSWORD_FOR_USERS           = "Password1"
   $NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
   # ------------------------------------------------------ #

⚠️ Tip: for a lab, start with a much smaller number (e.g. 20–50) — 10,000 accounts will take a while and clutter ADUC.

When finished, open ADUC and observe the accounts in the _EMPLOYEES OU

📸 Screenshot placeholder: ADUC _EMPLOYEES OU populated with generated user accounts
<img width="2619" height="1401" alt="Screenshot 2026-08-27 215336" src="https://github.com/user-attachments/assets/a39393a5-2cf0-49a1-b283-36f292c26dc4" />


Attempt to log into Client-1 with one of the accounts (take note of the password used in the script — Password1 by default)

📸 Screenshot placeholder: Client-1 login screen using a generated user account
<img width="2343" height="1014" alt="Screenshot 2026-08-27 215457" src="https://github.com/user-attachments/assets/039700f5-f543-4f03-942a-f7a85ef912fa" />


Allow the generated users to actually log into Client-1

By default, only Domain Admins and members of the local Administrators group can log into a domain-joined machine. Since the "Allow Domain Users access to Remote Desktop" step above already covers this for RDP, the accounts created by the script should work — but if you run into a "The sign-in method you're trying to use isn't allowed" or similar access-denied error, walk through the following:

In ADUC, open one of the generated user accounts and note its exact username (e.g. bod.tacilu) — the script uses firstname.lastname

📸 Screenshot placeholder: ADUC — generated user account properties showing the logon name
<img width="756" height="1026" alt="Screenshot 2026-08-27 215527" src="https://github.com/user-attachments/assets/efd09b44-cd15-4736-8a56-c7e694cc4f82" />


Confirm the account isn't already locked/disabled (right-click the account → Properties → Account tab)
On Client-1, open System Properties → Remote Desktop and verify "Domain Users" (or a group the generated accounts belong to) is still listed under "Select Users that can remotely access this PC"

From your local machine (or wherever you RDP from), open the Remote Desktop Connection client (mstsc) and connect to Client-1's public IP or hostname
At the credential prompt, sign in as mydomain.com\<username> (or <username>@mydomain.com) with the password from the script (Password1 by default)

Confirm the logon succeeds and you land on the Client-1 desktop as the generated (non-admin) user
💡 If the connection is refused outright, double-check the Client-1 Network Security Group (NSG) in Azure still allows inbound RDP (port 3389), and that the Windows Firewall isn't blocking Remote Desktop.

Dealing with Account Lockouts
Get logged into DC-1
Pick a random user account you created previously
Attempt to log in with it 10 times using a bad password

Configure Group Policy to lock out the account after 5 attempts 

📸 Screenshot placeholder: Group Policy Management Editor — Account Lockout Policy settings
<img width="2529" height="999" alt="Screenshot 2026-08-27 215624" src="https://github.com/user-attachments/assets/966b0ddc-18d2-4623-ab0e-e20741c7c783" />
<img width="2757" height="1272" alt="Screenshot 2026-08-27 215603" src="https://github.com/user-attachments/assets/a0314baa-b353-429e-ac6f-1cff78f577ac" />


Attempt to log in with it 6 times using a bad password
Observe that the account has been locked out within Active Directory

📸 Screenshot placeholder: ADUC — user account properties showing disabled
<img width="477" height="306" alt="Screenshot 2026-08-27 215659" src="https://github.com/user-attachments/assets/a7ea61c5-70e7-4076-8bb9-4c746b2d5f77" />




Unlock the account
Reset the password
Attempt to log in with the new password

Enabling and Disabling Accounts
Disable the same account in Active Directory
Attempt to log in with it and observe the error message

📸 Screenshot placeholder: Logon error — "The referenced account is currently disabled..."
<img width="483" height="498" alt="Screenshot 2026-08-27 215730" src="https://github.com/user-attachments/assets/b1e5eb38-ba9c-4930-b757-2248dcc73726" />

Re-enable the account and attempt to log in with it again


Observing Logs
Observe the logs on the Domain Controller (Event Viewer → Windows Logs → Security — look for Event IDs 4625 [failed logon], 4740 [account lockout], 4724 [password reset], 4725/4722 [account disabled/enabled])

📸 Screenshot placeholder: Event Viewer on DC-1 filtered to Security log
<img width="3519" height="1683" alt="Screenshot 2026-08-27 215757" src="https://github.com/user-attachments/assets/b9adece7-a0b8-484e-8042-2387b2cafe4d" />


Observe the logs on the client machine (Event Viewer → Windows Logs → Security on Client-1)

📸 Screenshot placeholder: Event Viewer on Client-1 showing logon events
<img width="3786" height="1776" alt="Screenshot 2026-08-27 215905" src="https://github.com/user-attachments/assets/c34d8920-5760-461c-ab77-7e06f7ba3c41" />


This lab is a precursor to cybersecurity and security operations — these are the same event types a SOC analyst would triage in a SIEM.
