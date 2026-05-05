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
<b>NB:</b> It is to be noted that the .ova file is a Wazuh server and hence, only uses the CLI interface. Inorder to access the <strong>Web Interface</strong>, you need to use a windows system that is connected to the Wazuh server. 
<br/>
Incase you choose to use your host machine to access the web interface like i did, you need to set the network of the server to <b>Bridged Adapter</b> so that it can communicate with your host.<br/>
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

<b>NB:</b> You need to give your server a Static Ip Address and it default gateway to ensure it IP address doesn't change in case it uses DHCP.<br/>

This can be done using the following commands.<br/>
Static ip addres: `sudo ifconfig eth0 192.168.1.x netmask 255.255.255.0`<br/>
default gateway: `sudo route add default gw 192.168.1.1 eth0`<br/>
<br/>

## Creating Agents to deploy to other devices

Creating agents in Wazuh is essential because they collect and send security data from endpoints to the central server, enabling real-time monitoring, threat detection, file integrity checks, and incident response; without agents, Wazuh has little visibility into systems and cannot effectively protect or monitor them.<br/>
Below are the steps to follow:<br/>
<br/>
<b>Step 1:</b> Click on create agents and select the windows system of the agent. <br/>

<b>Step 2:</b> Copy the powershell command and run it on the windows system you wish to install the agent.<br/>

<p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/88bd9db7-a57d-4e63-b67b-902ac84af0e0" height="80%" width="80%"/>
<br />
<br />

<b>Step 3:</b> Start the agent with the following command.<br/>
<br/>

 `NET START WazuhSvc`<br/>
 <br/>

 <p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/60dea72a-7880-4dc4-afce-3639cef798c1" height="80%" width="80%"/>
<br />
<br />

 <b>NB:</b> On the Wazuh dashboard, you should now see the agent's informations.<br/>

 <p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/d73c21f1-efa6-4b99-8152-1d7885be4cb9" height="80%" width="80%"/>
<br />
<br />


## Configure Wazuh to Monitor Sysmon Logs

Configuring Wazuh to monitor Sysmon logs is necessary because Sysmon provides deep, detailed visibility into system activities such as process creation, network connections, and file changes that standard Windows logs often miss; by ingesting these logs, Wazuh can detect advanced threats, suspicious behavior, and attack techniques more accurately, significantly improving overall security monitoring and incident response.<br/>
<br/>
To achieve this, let's follow the following steps.<br/>
<br/>
<b>Step 1:</b><br/>
**On the Windows VM**: Use a text editor (notepad++ or notepad) as an Administrator and edit the `C:\\Program Files (x86)\\ossec-agent\\ossec.conf` file and add the following entries: <br/>

<br/>
<br/>
   <p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/b9b87f1a-4f97-43ee-b168-0417403e9f7d" height="80%" width="80%"/><br/>
    <b>NB:</b> It should be under "Log Analysis"
   </p>
<br />
<br />
<b>Step 2:</b> Restart the Wazuh service:<br/>
<br/>
`NET STOP WazuhSvc`
<br/>
`NET START WazuhSvc`
<br/>
<br/>

<p align="center">
<img alt="image" src="https://github.com/user-attachments/assets/4a558e12-d5cb-4762-b9c6-14411502b601" height="80%" width="80%"/>
<br />
<br />
