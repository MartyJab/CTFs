# SIEM QRadar  
Sometimes I had to use hints or when I was completely stuck, I had to use the walkthrough solutions.
## Scenario
A financial company was compromised, and they are looking for a security analyst to help them investigate the incident. The company suspects that an insider helped the attacker get into the network, but they have no evidence.
The initial analysis performed by the company's team showed that many systems were compromised. Also, alerts indicate the use of well known malicious tools in the network. As a SOC analyst, you are assigned to investigate the incident using QRadar SIEM and reconstruct the events carried out by the attacker.
**Dataset:**
- Sysmon - swift on security configuration
- Powershell logging
- Windows Eventlog
- Suricata IDS
- Zeek logs (conn, HTTP)

## Setup
Creating a virtual Network between the Host System and the VM (QRadar)
![](./images/07_Threat_Hunting_SIEM_QRadar-1786723578335.webp)
![](./images/07_Threat_Hunting_SIEM_QRadar-1786723600168.webp)

Then, launching the VM and accessing it via 192.168.20.21:443 and logging into QRadar
![](./images/07_Threat_Hunting_SIEM_QRadar-1786723896300.webp)

## How many log sources available?
Admin --> Log Sources
![](./images/07_Threat_Hunting_SIEM_QRadar-1786724959689.webp)

## What is the IDS software to monitor the network
Suricata is a IDS for network monitoring
![](./images/07_Threat_Hunting_SIEM_QRadar-1786725521817.webp)

## What is the domain name used in the network?
Filtering by a local Source IP Address
![](./images/07_Threat_Hunting_SIEM_QRadar-1786729648480.webp)

Filtering by an successful logon as an event name
![](./images/07_Threat_Hunting_SIEM_QRadar-1786730398097.webp)

Inspecting the payload information of one. DC stands for Domain Controller
![](./images/07_Threat_Hunting_SIEM_QRadar-1786730468226.webp)

## Multiple IPs were communicating with the malicious server. One of them ends with "20". Provide the full IP.
The dashboard shows the top source addresses for offenses. You can find one that ends with .20
![](./images/07_Threat_Hunting_SIEM_QRadar-1786999027106.webp)

## What is the SID of the most frequent alert rule in the dataset?
Grouping the search results by the rule SID in the search edit menu
![](./images/07_Threat_Hunting_SIEM_QRadar-1787001581273.webp)
![](./images/07_Threat_Hunting_SIEM_QRadar-1787001649256.webp)

## What is the attacker's IP address?
Filter by rule SID is not N/A so you find the alert events
![](./images/07_Threat_Hunting_SIEM_QRadar-1787060375113.webp)

The most shown destination IP from the alters seems to be the attacker
![](./images/07_Threat_Hunting_SIEM_QRadar-1787060444077.webp)

## The attacker was searching for data belonging to one of the company's projects, can you find the name of the project?
Filtering the logs for the regualr expression "project"
![](./images/07_Threat_Hunting_SIEM_QRadar-1787062370927.webp)

In one log you can find the name of the project
![](./images/07_Threat_Hunting_SIEM_QRadar-1787062475502.webp)

## What is the IP address of the first infected machine?
Was stuck on this question so I used to walkthrough solutions:

Its the IP of the PC which was searching for the project data
![](./images/07_Threat_Hunting_SIEM_QRadar-1787062823646.webp)

## What is the username of the infected employee using 192.168.10.15?
Filter by the IP address
![](./images/07_Threat_Hunting_SIEM_QRadar-1787142551198.webp)

Look for a login/logoff related event
![](./images/07_Threat_Hunting_SIEM_QRadar-1787142585257.webp)

Inspect the payload information
![](./images/07_Threat_Hunting_SIEM_QRadar-1787142614511.webp)

## Hackers do not like logging, what logging was the attacker checking to see if enabled?
Filter by username
![](./images/07_Threat_Hunting_SIEM_QRadar-1787144430436.webp)

Logging invocation Event
![](./images/07_Threat_Hunting_SIEM_QRadar-1787144493610.webp)

Event Description shows checked logging
![](./images/07_Threat_Hunting_SIEM_QRadar-1787144672387.webp)

## Name of the second system the attacker targeted to cover up the employee?
Filtering for the "del" command in the cmd, because it commonly used for coverups
![](./images/07_Threat_Hunting_SIEM_QRadar-1787147845880.webp)

Two times a file was deleted
![](./images/07_Threat_Hunting_SIEM_QRadar-1787147873188.webp)

Only one system name fits the answer format
![](./images/07_Threat_Hunting_SIEM_QRadar-1787148002713.webp)

## When was the first malicious connection to the domain controller (log start time - hh:mm:ss)?
Filtering for a detected network connection
![](./images/07_Threat_Hunting_SIEM_QRadar-1788945624725.webp)

This connection seems suspicious because notepad.exe initiated the connection
![](./images/07_Threat_Hunting_SIEM_QRadar-1788945856461.webp)

![](./images/07_Threat_Hunting_SIEM_QRadar-1788945795451.webp)

## What is the md5 hash of the malicious file?
Filter for MD5 Expression in the Log
![](./images/07_Threat_Hunting_SIEM_QRadar-1788947068289.webp)

A hash created by the infected machine seems to be the hash of a malicious file
![](./images/07_Threat_Hunting_SIEM_QRadar-1788947283309.webp)

The payload shows the md5 hash
![](./images/07_Threat_Hunting_SIEM_QRadar-1788947194580.webp)

## What is the MITRE persistence technique ID used by the attacker?
I was stuck at this question so I used the walkthrough as help:

Filtering by a infecterd systems IP and grouping by Event Name to find possible persistence related event names
![](./images/07_Threat_Hunting_SIEM_QRadar-1788949207990.webp)

Regestry Key manipulation can be used for persistence initiation
![](./images/07_Threat_Hunting_SIEM_QRadar-1788949272458.webp)

One Payload shows that a script was added to the Run Registry Key which is for auto start.
![](./images/07_Threat_Hunting_SIEM_QRadar-1788949771404.webp)

The MITRE ID for that is T1547.001

## What protocol is used to perform host discovery?
ICMP is typically used for network scanning, so I filtered by ICMP Protocol
![](./images/07_Threat_Hunting_SIEM_QRadar-1788952037027.webp)

The infected system 192.168.10.15 used icmp packets 37 times
![](./images/07_Threat_Hunting_SIEM_QRadar-1788952026042.webp)

It used it on several local systems
![](./images/07_Threat_Hunting_SIEM_QRadar-1788952079935.webp)

ICMP Type 8 stands for Echo Request
![](./images/07_Threat_Hunting_SIEM_QRadar-1788952198516.webp)

## What is the email service used by the company?
There are no standard smtp port traffic logs.
I was stuck so I checked the solutions:
Checking for the origin of outgoing IP addresses which is microsoft a lot of times, thats why the email service is supposed to be office365.

## What is the name of the malicious file used for the initial infection?
You can find the name in the payload inforamtion picture of the md5 question
![](./images/07_Threat_Hunting_SIEM_QRadar-1789029878839.webp)

## What is the name of the new account added by the attacker?
Grouping all logs by event name and searching for events related to account creation
![](./images/07_Threat_Hunting_SIEM_QRadar-1789030588542.webp)

There is one log which creates an account
![](./images/07_Threat_Hunting_SIEM_QRadar-1789030658620.webp)

## What is the PID of the process that performed injection?
Was kind of stuck at this one as well so I used the solution as help as well
CreateRemoteThread is an event name of injection logs
![](./images/07_Threat_Hunting_SIEM_QRadar-1789033326645.webp)

The payload of a log indicates Injection due to a new process starting at a memory address without a starting module
![](./images/07_Threat_Hunting_SIEM_QRadar-1789033666833.webp)

## What is the name of the tool used for lateral movement?
I didn't find anything in the logs and the the hints didnt help so I asked AI which tools are commonly used for lateral movement and so thats how I solved this question: wmiexec.py
After reviewing the solution I just learned how to view all CLI commands at once which would have been very useful.
## Attacker exfiltrated one file, what is the name of the tool used for exfiltration?
Searching through all executed commands to find one that is used for exfiltration
![](./images/07_Threat_Hunting_SIEM_QRadar-1789123356373.webp)

![](./images/07_Threat_Hunting_SIEM_QRadar-1789123380147.webp)

## Who is the other legitimate domain admin other than the administrator?
Filtering by "special" for the event "Success Audit: Successful logon with administrative or special privileges" which is common for admin behaviour and grouping by username
![](./images/07_Threat_Hunting_SIEM_QRadar-1789123795774.webp)
![](./images/07_Threat_Hunting_SIEM_QRadar-1789123835509.webp)
Rambo was added by the attacker
## The attacker used the host discovery technique to know how many hosts available in a certain network, what is the network the hacker scanned from the host IP 1 to 30?
Filtering for icmp protocol and the infected system 192.168.10.15
![](./images/07_Threat_Hunting_SIEM_QRadar-1789128708501.webp)

The destination IPs are in the subnet 192.168.20.0/24
![](./images/07_Threat_Hunting_SIEM_QRadar-1789128751152.webp)

## What is the name of the employee who hired the attacker?
The exfiltrated file was named sami.xlsl. Thats why the employee name is supposed to be sami. It seems like sami was intrested in the data the organisation is saving about him 

