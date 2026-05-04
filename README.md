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

  <!-- IMAGE HERE -->
<b>Step 2:</b><br/>
Since we're using VirtualBox, we need to set the VMSVGA graphic controller. Setting another graphic controller freezes the VM window.<br/>
<br/>
  - Select the imported VM<br/>
  - Click <b>Settings</b> > <b>Display</b><br/>
  - In <b>Graphic Controller,</b> select the `VMSVGA` option

  <!-- IMAGE HERE -->

