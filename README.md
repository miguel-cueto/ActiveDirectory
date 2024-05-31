<h1>Creating a Full Active Directory Lab</h1>


<h2>Description</h2>
This tutorial demonstrates how to set up a complete Active Directory lab environment on a personal computer using VirtualBox. It covers installing and configuring Windows Server 2019 as a domain controller, setting up Active Directory, configuring networking services, creating users, and joining a Windows 10 client to the domain. The goal is to help understand how Active Directory and Windows networking work in a corporate or educational setting.
<br />


<h2>Languages and Utilities Used</h2>

- <b>VirtualBox</b> 
- <b>Windows Server 2019</b>
- <b>PowerShell</b> 
- <b>Windows 10</b>

<h2>Environments Used </h2>

- <b>VirtualBox virtual machines (Windows Server 2019 and Windows 10)</b>

<h2>Program walk-through:</h2>

<p align="center">
Prerequisites:

Download and install Oracle VirtualBox 7.0.18:

- <b>Visit https://www.virtualbox.org/wiki/Downloads
- <b>Download the "VirtualBox 7.0.18 platform packages" for your operating system
- <b>If you encounter an error about a missing Visual C++ package during installation, follow these steps:
  - <b>Close the VirtualBox installer
  - <b>Download the Visual C++ redistributable from https://aka.ms/vs/17/release/vc_redist.x64.exe
  - <b>Run the downloaded executable to install the Visual C++ package
  - <b>After installation, restart the VirtualBox installer
- <b>Also, download the "VirtualBox 7.0.18 Oracle VM VirtualBox Extension Pack" by clicking on "All supported platforms"

<p align="center">
<img src="https://i.imgur.com/c1TC8mC.png" height="80%" width="80%" alt="VirtualBox"/>
<br />
<br />
Download the Windows 10 ISO:

- <b>Visit https://www.microsoft.com/en-us/software-download/windows10ISO
- <b>Follow the prompts to download the Windows 10 ISO file

<p align="center">
<img src="https://i.imgur.com/4oywnTk.png" height="80%" width="80%" alt="Window 10"/>
<br />
<br />
Download the Windows Server 2019 ISO:

- <b>Visit https://info.microsoft.com/ww-landing-windows-server-2019.html
- <b>Follow the prompts to download the Windows Server 2019 ISO file

<p align="center">
<img src="https://i.imgur.com/lAcQmoT.png" height="80%" width="80%" alt="VirtualBox"/>
<br />
<br />
<p align="center">
Setup Domain Controller VM:

- <b>Open Oracle VirtualBox and click the "Machine" tab, then click "New" to create a new virtual machine.
- <b>Name the virtual machine "DC" and select "Microsoft Windows" as the Type. Click "Next".
- <b>Assign at least 2048 MB (2 GB) of RAM to the virtual machine and click "Next".
- <b>Select "Create a virtual hard disk now" and click "Create".
- <b>Select "VDI (VirtualBox Disk Image)" as the hard disk file type and click "Next".
- <b>Select "Dynamically allocated" as the storage type and click "Next".
- <b>Leave the location and file size as default and click "Create".
- <b>With the VM selected, go to Settings > General > Advanced.  Change "Shared Clipboard" and Drag'n'Drop to Bidirectional.
- <b>In Settings > System > Processor and increase the number of processors if your host machine can support more.  Click on "Enable PAE/NX" under Extended Features.
- <b>In the Settings > Display, increase the video memory to the maximum allowed value.
In Settings > Network, enable two network adapters:

  - <b>Adapter 1: Attached to NAT (for internet connectivity)
  - <b>Adapter 2: Attached to Internal Network (for domain connectivity)


- <b>Start the VM

<p align="center">
<img src="https://i.imgur.com/eXwoicS.png" height="80%" width="80%" alt="VM"/>
<br />
<br />

- <b>Install Windows Server 2019.
- <b>In the "Windows Server Installer", select "Windows Server 2019 Standard Evaluation (Desktop Experience)".
- <b>After installation, log into the server as Administrator.
- <b>Speed the VM by installing VBoxWindowsAdditons-amd64
  - <b>Click Devices > Insert Guest Additions CD Images.
  - <b>Click the File Explore Icon > This PC > CD Drive (D:) VirtualBox Guest Additions > VBoxWindowsAdditons-amd64
  - <b>Click Start Icon > Shutdown

<p align="center">
<img src="https://i.imgur.com/oz1f2nc.png" height="80%" width="80%" alt="VBoxWindowsAdditons-amd64"/>
<br />
<br />

- <b>Setup Internal Internet
  - <b>Network Icon > Network > Change adopter options >
      - <b>Ethernet Network > right click Status > Detail... > Look at IP Address (10.x.x.x) > rename INTERNET
      - <b>Ethernet 2 Unidentified Network > right click Status > Detail... > Look at IP Address (169.254.x.x) > rename INTERNAL
         - <b>Right click "INTERNAL" > Properties > Internet Protocol Version 4 (TCP/IPv4) > Use the following IP Address:
           - <b>Change "IP address" to 172.16.0.1
           - <b>Change "Subnet mask" to 255.255.255.0
           - <b>Leave "Default gateway" blank
           - <b>Change "Preferred DNS server" to 172.16.0.1
           - <b>Leave "Alternet DNS server" blank


<p align="center">
<img src="https://i.imgur.com/38bOH2D.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />

- <b>Rename the PC
  - <b>Right click Start Icon > System > Rename This PC > rename it "DC"

<p align="center">
<img src="https://i.imgur.com/9TYESVB.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />

- <b>In Server Manager, click "Add roles and features".
- <b>Install the "Active Directory Domain Services" role.

<p align="center">
<img src="https://i.imgur.com/dLP8T8f.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />
  
- <b>Promote the server as a Domain Controller:

  - <b>Launch the Active Directory Domain Services Configuration Wizard
  - <b>Select "Add a new forest" and enter a root domain name (e.g., mydomain.com)
  - <b>Provide a password for Directory Services Restore Mode (DSRM)

<p align="center">
<img src="https://i.imgur.com/vCIOuT2.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />
  
- <b>After installation, restart the VM when prompted.


- <b>Create a new Organizational Unit (OU) named "Admins":

  - <b>Go to Start Icon > Administrative Tools > Active Directory Users and Computers
  - <b>Right-click the domain name and select New > Organizational Unit
  - <b>Name the OU "Admins"

<p align="center">
<img src="https://i.imgur.com/3XgxiCA.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />

- <b>Create a domain admin account in the Admins OU (e.g., a-"first initial"/"last name"):

  - <b>In Active Directory Users and Computers, right-click the Admins OU
  - <b>Select New > User
  - <b>Enter the first name, last name, and username (e.g., a-(First Initial, Last Name)
  - <b>Click Next, enter a secure password, and only check "Password never expires."
  - <b>Click Finish, then right-click the new user and select "Add to a group"
  - <b>Type "domain admins", click OK, then OK again to add the user to the Domain Admins group

<p align="center">
<img src="https://i.imgur.com/9kEB81d.png" height="80%" width="80%" alt="Internal Internet"/>
<br />
<br />

<p align="center">
Configure Routing and NAT:

- <b>In the DC VM, launch Server Manager.
- <b>Click "Add roles and features" and install the "Remote Access" role.
- <b>In the Tools menu, open "Routing and Remote Access".
- <b>Right-click on the server name and select "Configure and Enable Routing and Remote Access".
- <b>In the wizard, select "Network Address Translation (NAT)".
- <b>For the NAT Internet Connection, select the "NAT" adapter (Adapter 1).
- <b>Complete the wizard to enable NAT.

<p align="center">
Configure DHCP Server:

- <b>In the DC VM, launch Server Manager.
- <b>Click "Add roles and features" and install the "DHCP Server" role.
- <b>After installation, open the "DHCP" tool from Administrative Tools.
- <b>Create a new Scope:

  - <b>Right-click "Scope" and select "New Scope"
  - <b>Scope name: Internal Network
  - <b>Network: 172.16.0.0/24 (matches the internal adapter IP)
  - <b>Add an exclusion range if needed


- <b>Configure Scope Options:

  - <b>Router: 172.16.0.1 (IP of the DC internal adapter)
  - <b>Parent/Root Domain: mydomain.com
  - <b>Activate the new scope


<p align="center">
Create Active Directory Users:

- <b>On the DC VM desktop, create a new folder named "PDscripts".
- <b>Download the New-CustomUsers.ps1 PowerShell script (link provided in the notes).
- <b>Extract the script and names.txt file into the PDscripts folder.
- <b>Open the names.txt file and add your name at the top.
- <b>Open PowerShell as Administrator.
- <b>Run the command: Set-ExecutionPolicy Unrestricted
- <b>Change the directory to the script location: cd C:\Users\Administrator\Desktop\PDscripts
- <b>Run the script: .\New-CustomUsers.ps1

  - <b>This will create 1000 users in the _Users OU


<p align="center">
Setup Client VM:

- <b>In VirtualBox, create a new VM named "Client1" for Windows 10.
- <b>Allocate reasonable RAM (e.g., 4096 MB).
- <b>Select "Use an existing virtual hard disk" and choose the Windows 10 ISO.
- <b>In Settings > Network, attach the VM to the "Internal Network".
- <b>Start the VM and install Windows 10 (no product key needed).
- <b>In the Windows 10 setup, create a local admin account.
- <b>After setup, join the machine to the mydomain.com domain:

  - <b>Right-click Start > System > Rename this PC (advanced)
  - <b>Click "Change" under the "Member of" section
  - <b>Enter the domain name (e.g., mydomain.com)
  - <b>Provide the domain admin credentials when prompted


- <b>Restart the VM after joining the domain.
- <b>Sign into the Client1 VM using a domain user account created earlier.
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
