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

To extract information we need for Wazuh, we install Mimikatz using Powershell on windows system with the following command.<br/>
<br/>
 `Invoke-WebRequest -Uri https://github.com/ParrotSec/mimikatz/archive/refs/heads/master.zip -OutFile C:/Users/m122/Downloads/mimikatz.zip`
<br/>
<b>NB</B>: Be sure to change the user from m122 in the -OutFile path.<br/>

