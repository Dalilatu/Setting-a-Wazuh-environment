# Setting-a-Wazuh-environment
## Description
This repository describes in detail how i configured the SIEM tool Wazuh i will be using for incident response and threat analysis.
<br />


<h2>Utilities Used</h2>

- <b>Wazuh</b>
- <b>Windows 11</b>


<h2>Environments Used </h2>

- <b>Virtual Box</b>

<h2>Program walk-through:</h2>

<h3>Installation Tools</h3>

| Tool Name | Why It Was Used | Download Link |
|-----------|---------------------------|---------------|
| Wazuh| Open-source security platform used for threat detection, incident response, and compliance management, combining SIEM (Security Information and Event Management) and XDR (Extended Detection and Response) capabilities | [Download](https://packages.wazuh.com/4.x/vm/wazuh-4.7.4.ova) |
| Windows 11 Enterprise | Will be used as our client machine | [Download](https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise) |

## Pre-Installation

We will install a malware Mimikatz using Powershell on windows system with the following command.This will enable us to harvest victim's credentials and device informations.<br/>
<br/>

 `Invoke-WebRequest -Uri https://github.com/ParrotSec/mimikatz/archive/refs/heads/master.zip -OutFile C:/Users/m122/Downloads/mimikatz.zip`
<br/>
<br/>

<b>NB</b>: Be sure to change the user from m122 to the system's User Profile name in the -OutFile path.<br/><br/>

## Download and Deploy Wazuh OVA
Wazuh has a pre-built virtual machine image in Open Virtual Appliance (OVA) format. This can be directly imported to VirtualBox or other OVA compatible virtualization systems.<br/>
<br/>
<h2>Steps To Follow:</h2>
<br/>
<b>Step 1:</b><br/>
Import the OVA file in Virtual Box. <br/>
<br/>
  <!-- IMAGE HERE -->
  <p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/738177ca-6aa5-4809-b0bc-f1f3ad94d37e" height="80%" width="80%"/>
<br />
<br />
  
<b>Step 2:</b><br/>
Since we're using VirtualBox, we need to set the VMSVGA graphic controller. Setting another graphic controller freezes the VM window.<br/>
  - Select the imported VM<br/>
  - Click <b>Settings</b> > <b>Display</b><br/>
  - In <b>Graphic Controller,</b> select the `VMSVGA` option

  <!-- IMAGE HERE -->
  <p align="center">
 <img alt="image" src="https://github.com/user-attachments/assets/91acc11d-7cbd-4c68-ad53-ace6962c70db" height="80%" width="80%"/>
<br />
<br />

  <b>Step 3:</b><br/>
Start the VM then access it using the VM and password.It is easiest to access via SSH using Putty so that copy/paste functions properly.
<br/>

<!-- user: wazuh-user
     password: wazuh
     
     SSH root user login has been deactivated; nevertheless, the wazuh-user retains sudo privileges. Root privilege escalation can be achieved by executing the following command:
     
     sudo -i  -->

<br/>

<b>Step 4:</b><br/>
Access the Wazuh web interface using the following credentials.<br/>
<br/>
`https://<wazuh_server_ip>`
<br/>
<br/>
user: admin
<br/>
password: admin
<br/>
<br/>
<b>NB:</b> It is to be noted that the .ova file is a Wazuh server and hence, only uses the CLI interface. Inorder to access the <strong>Web Interface</strong>, you need to use a windows system that is connected to the Wazuh server. Incase you choose to use your host machine to access the web interface like i did, you need to set the network of the server to <b>Bridged Adapter</b> so that it can communicate with your host.<br/>
<br/>
<br/>

<p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/2ef5101f-6d6b-4968-8622-44a2c5ee8da3" height="80%" width="80%"/>
<br />
<br />

<p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/112d756a-1746-4d28-afae-dfe72623f439" height="80%" width="80%"/>
<br />
<br />


