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
<br 

## Configuring Wazuh Server for Sysmon Events 
Enable the ability to ssh as root from our Windows VM:<br/>
<br/>
<b>Step 1:</b> Enable root SSH access:<br/>
In the Wazuh VM, edit the SSH configuration file with the command 
<br/>
<br/>
`sudo nano /etc/ssh/sshd_config:`  <br/>
<br />
<br />

<b>Step 2:</b> Change `##PermitRootLogin no` to `PermitRootLogin yes`  <br/>
<p align="center">
   <img alt="image" src="https://github.com/user-attachments/assets/3301a94e-be20-4010-a353-2d583e936344" height="80%" width="80%"/><br/>
Save and Exit
</p>
<br />
<br />

<b>Step 3:</b> Restart the SSH service: service sshd restart.<br/>
<br/>
`sudo service sshd restart`

## TESTING

## Creating Malware Rules on Wazuh

We are going to test it by writing some rules for detecting suspicious events related to the mimikatz.exe process. Mimikatz is a well-known tool for extracting Windows credentials.<br/>
<br/>
<b>Step 1:</b> Write a Security rule by editing the Wazuh rule file.<br/>
Add Security rules using the following command:<br/>
<br/>
`sudo nano /var/ossec/etc/rules/local_rules.xml`<br/>
<br/>
You can add rule using the following codes:<br/>
<p align="center">
  <img alt="image" src="https://github.com/user-attachments/assets/ceef69d2-4bb7-47ad-8664-dc66320124c5" height="80%" width="80%"/><br/>
In this case, we want to create a rule that detects mimikatz.exe process on the Agent's system. We have already install and executed the process just for demonstration.
</p>
<br />
<br />
<b>NB:</b> You can go ahead and execute the mimikatz.exe file we downloaded at the beginning of the lab. It was executed using Powershell as admin.<br/>
Executing `mimikatz.exe` will trigger and alert in the Wazuh dashboard as shown below.
<br/>
<br/>

<p align="center">
 Go to "Security Events":<br/>
   <img alt="image" src="https://github.com/user-attachments/assets/9887cf39-a943-4dd2-8e89-d5e982b52ec0" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
You can see a dashboard showing different security events:<br/>
   <img alt="image" src="https://github.com/user-attachments/assets/bd0c95ea-6b56-47df-91d7-ce09008dd061" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
Different security alerts:<br/>
   <img alt="image" src="https://github.com/user-attachments/assets/0b10ee78-b36e-452f-9713-0bdfd4c4080e" height="80%" width="80%"/><br/>
</p>
<br />
<br />

## Integrating VirusTotal with Wazuh Manager
This integration will automatically send the file hash of any new file downloaded, placed, or created to a specified folder, in our case the Downloads folder, to VirusTotal. The only reason we are limiting it to the Downloads folder is to ensure we do not exceed the daily allowance for lookups. 
<br/><br/>

<b>Step 1:</b> Log into VirusTotal and get it API KEY<br/>

<p align="center">
Getting VirusTotal API key:<br/>
  <img alt="image" src="https://github.com/user-attachments/assets/c54ee3d4-233d-4080-b927-79dd3bd59b9a" height="80%" width="80%"/><br/>
</p>
<br />
<br />

Paste the VirusTotal integration configuration into the  `/var/ossec/etc/ossec.conf` file. Be sure to place your own VirusTotal API key in the `<YOUR_VIRUS_TOTAL_API_KEY>` placeholder.<br/>
Use the following: <br/> <br/>

Wazuh Server CLI : `sudo nano /var/ossec/etc/ossec.conf`<br/><br/>

<p align="center">
Paste or type the following code in the "ossec.conf" file:<br/>
  <img alt="image" src="https://github.com/user-attachments/assets/1036a4e7-722f-4834-a230-3c5f9c19b9b1" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
Be sure to add your own VirusTotal API key. :<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/162a1e26-e5d0-4e94-a5f6-82daf6eb3099" height="80%" width="80%"/><br/>
</p>
<br />
<br />

Restart wazuh-manager: `sudo systemctl restart wazuh-manager` <br/>
<br/>


<b>Step 2:</b> Configure Wazuh Agent <br/>

On the Windows VM, use a text editor (notepad++ or notepad) and edit the `C:\\Program Files (x86)\\ossec-agent\\ossec.conf` file and add the following entries to track file changes in the Downloads folder.<br/>

<p align="center">
 Add this on the .conf file:<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/50d7ad33-950f-400e-bef0-e92c039d28ed" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
 Add this on the .conf file:<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/a4d9e063-e386-42f1-81aa-a8db7ec5a3c4" height="80%" width="80%"/><br/>
</p>
<br />
<br />

## TESTING
Let's download the EICAR file to test if the configuration work. <br/>
<b>NB:</B> EICAR is not a real malware, but was designed to test the response of computer antivirus programs. Instead of using real malware, which could cause real damage, this test file allows people to test anti-virus software without having to use a real computer virus.
<br/>
<br/>
Once i downloaded EICAR using powershell, i checked Wazuh dashboard to see if it was scanned.
<br/>

<p align="center">
Wazuh dashboard showing positive scan for <b>eicar</b>:<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/a526d337-eaf7-4aea-a13b-389d1d01d340" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
Further analysis of alert:<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/7df5e3a8-2dda-423c-8d84-06f77fe12161" height="80%" width="80%"/><br/>
</p>
<br />
<br />

<p align="center">
VirusTotal scan showing thesame number of red flags found on Wazuh:<br/>
 <img alt="image" src="https://github.com/user-attachments/assets/dfa18fc0-bbd8-403d-b752-745439d533d1" height="80%" width="80%"/><br/>
</p>
<br />
<br />





## Conclusion
This project provided hands-on experience in deploying and configuring a functional Wazuh environment for security monitoring and threat detection. Through this setup, I gained practical knowledge in log collection, SIEM integration, rule configuration, and basic incident detection within a controlled lab environment. The project strengthened my understanding of SOC operations and demonstrated the importance of centralized monitoring in improving system visibility and security analysis.


