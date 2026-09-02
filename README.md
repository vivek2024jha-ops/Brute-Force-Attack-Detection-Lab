# Brute-Force-Attack-Detection-Lab

## Project Overview

The Brute Force Attack Detection Lab is a hands-on cybersecurity project focused on simulating and investigating repeated authentication failures against a Windows workstation.

In this lab, I used a Kali Linux VM as the testing machine and a Windows 10 VM as the target system in an isolated virtual environment. The lab focused on understanding how authentication attempts are generated, how Windows records failed authentication activity, and how a SOC Analyst can investigate the related security events.

I used Hydra to perform controlled authentication testing and Windows Event Viewer to investigate the resulting security logs. Event ID 4624 was reviewed to understand successful authentication activity, while Event ID 4625 was analyzed to identify failed authentication attempts.

The investigation also included identifying the source IP address associated with the failed authentication activity and correlating the observed events with the controlled testing performed from the Kali Linux VM.

This project helped me understand the basic workflow of detecting authentication-based attacks through Windows security telemetry and performing initial security event investigation.

The lab provides practical experience relevant to Tier 1 SOC Analyst activities such as authentication monitoring, log analysis, source identification, event correlation, and basic incident investigation.

# Objectives

* Understand the basic concept of brute-force and password-guessing attacks
* Configure an isolated Kali Linux and Windows lab environment
* Establish network communication between the Kali Linux and Windows VMs
* Identify the RDP service running on the Windows target
* Perform controlled authentication testing using Hydra
* Monitor successful and failed authentication events
* Investigate Event ID 4624 (Successful Logon)
* Investigate Event ID 4625 (Failed Logon)
* Identify the source IP associated with failed authentication attempts
* Correlate authentication events with the controlled testing activity
* Develop basic threat detection and incident investigation skills relevant to a SOC Analyst

## Lab Environment

| **Component**           | **Details**          |
| ----------------------- | -------------------- |
| Operating System        | Windows 10 VM        |
| Testing Machine         | Kali Linux VM        |
| Virtualization Platform | Oracle VirtualBox    |
| Target IP Address       | 192.168.50.10        |
| Kali Linux IP Address   | 192.168.50.20        |
| Target Service          | RDP                  |
| Target Port             | 3389                 |
| Authentication Tool     | Hydra                |
| Network Scanning Tool   | Nmap                 |
| Log Investigation Tool  | Windows Event Viewer |
| Lab Account             | abc                  |

## Tools Used

* **Kali Linux** – Used as the testing machine for controlled authentication testing
* **Windows 10** – Used as the target workstation
* **Hydra** – Used to perform controlled authentication testing
* **Nmap** – Used to identify and verify the RDP service
* **Netcat** – Used to verify TCP port connectivity
* **Windows Event Viewer** – Used to investigate Windows Security Events
* **Windows Security Logs** – Used to monitor authentication activity
* **Oracle VirtualBox** – Used to provide an isolated virtual lab environment
* **xfreerdp** – Used to establish an RDP connection from Kali Linux to Windows

## Investigation Scenario

A Windows workstation was configured as a controlled cybersecurity lab environment. A Kali Linux VM was connected to the Windows workstation through an isolated virtual network.

The objective was to simulate authentication-related attack activity in a controlled environment and investigate how Windows records the resulting authentication events.

The investigation focused on successful and failed authentication events, identifying the source IP address, and correlating repeated failed authentication attempts with the activity performed from the Kali Linux VM.

# Investigation Process

## Step 1 - Configure the Lab Network

An isolated virtual network was configured in Oracle VirtualBox to allow communication between the Kali Linux and Windows VMs.

<img width="1917" height="955" alt="01-Lab Network Configuration" src="https://github.com/user-attachments/assets/0bcde614-3a46-4850-8060-814832f37462" />


The Windows target was assigned the IP address: **192.168.50.10**

The Kali Linux VM was assigned the IP address: **192.168.50.20**

This configuration provided a controlled environment for performing the security testing.

## Step 2 - Verify Network Connectivity

Network connectivity between the Kali Linux and Windows VMs was verified using the ping command.

<img width="1761" height="660" alt="02-Verify Network Connectivity" src="https://github.com/user-attachments/assets/be040006-8fb3-41b7-9603-739108ee953e" />


```bash
ping -c 4 192.168.50.10
```

The test successfully returned four replies with 0% packet loss.

Successful connectivity confirmed that the two virtual machines could communicate with each other before beginning the investigation.

## Step 3 - Identify the RDP Service

Nmap was used to identify the available RDP service on the Windows target.

The scan identified:

* Port: 3389/tcp
* Service: ms-wbt-server
* Service: Microsoft Terminal Services
* Target: Windows workstation

This confirmed that the RDP service was accessible on the Windows lab machine.

<img width="1841" height="1035" alt="03-Identify the RDP Service" src="https://github.com/user-attachments/assets/facdc693-62f9-490b-afc6-9fe8b15977b7" />


## Step 4 - Verify RDP Connectivity

The RDP service was accessed from Kali Linux using xfreerdp.

A successful RDP session was established with the Windows workstation.

This step helped establish normal authentication activity before performing the controlled authentication testing.

<img width="1917" height="991" alt="04 -Verify RDP Connectivity" src="https://github.com/user-attachments/assets/1083b97a-4d07-4a5e-af70-70de35ba220b" />


## Step 5 - Investigate Event ID 4624

### Event ID 4624 – Successful Logon

Event ID 4624 was investigated to identify successful authentication activity on the Windows system.

Additional event information was reviewed to understand the authentication details.

The logon type was reviewed to determine how the authentication occurred.

The source-related information was also reviewed where available.

<img width="1318" height="762" alt="05-Event ID 4624" src="https://github.com/user-attachments/assets/c5f6b5cb-8229-44a8-9225-b41ea80661bd" />


A successful RDP authentication event was observed with:

* Event ID: 4624
* Logon Type: 10
* Target: Windows workstation
* Account: abc

<img width="1062" height="742" alt="05-Event ID 4624 Logon Type" src="https://github.com/user-attachments/assets/8900a893-74ef-45d8-a898-933be4c26fec" />

<img width="1032" height="731" alt="05-Event ID 4624 IP Address" src="https://github.com/user-attachments/assets/0bb5c70a-97f6-4e43-9f79-478ac9c1db0c" />

Logon Type 10 represents a Remote Interactive Logon and is commonly associated with Remote Desktop Services.

Successful authentication events provide useful context when investigating later failed authentication activity.

## Step 6 - Perform Controlled Authentication Testing

Hydra was used from the Kali Linux VM to perform controlled authentication testing against the Windows lab environment.

<img width="1917" height="883" alt="06-Controlled Authentication Testing" src="https://github.com/user-attachments/assets/cb62d227-d46e-47a7-90bd-fc59c8e5a6a9" />


A small test password list containing incorrect passwords was used to generate failed authentication attempts.

The testing was performed only against the isolated Windows lab machine.

The purpose of this activity was to generate authentication telemetry that could be investigated through Windows Security Logs.

## Step 7 - Investigate Event ID 4625

### Event ID 4625 – Failed Logon

After the controlled authentication testing, the Windows Security Log was reviewed for failed authentication events.

<img width="1158" height="865" alt="07-Event ID 4625 Failed Logon" src="https://github.com/user-attachments/assets/5c5b086d-43ad-45b8-9d3c-b9ba2dffd031" />


Additional event details were reviewed to understand the authentication failure.

<img width="1186" height="867" alt="07-Event ID 4625 D1" src="https://github.com/user-attachments/assets/eb2a8d8f-ac4e-45b7-b136-352060b45226" />


Further information from the event was analyzed.

<img width="1167" height="863" alt="07-Event ID 4625 D2" src="https://github.com/user-attachments/assets/b553af7c-602e-4a3c-b687-54da06dcebb6" />


The investigation identified Event ID 4625 events associated with the test account.

Information reviewed included:

* Target username
* Logon type
* Authentication package
* Failure information
* Source IP address
* Workstation name
* Event timestamp

Multiple failed authentication attempts can be an indicator of password-guessing or brute-force activity and should be investigated by a SOC Analyst.

## Step 8 - Identify the Source IP Address

The source information within the Event ID 4625 logs was reviewed to identify the system generating the failed authentication activity.

<img width="1217" height="885" alt="08-Identify the Source IP Address" src="https://github.com/user-attachments/assets/053ef162-a2ac-4474-9cd6-1a41bf8cc3e2" />


The source IP address observed during the investigation was: **192.168.50.20**

This address belonged to the Kali Linux VM used for the controlled authentication testing.

Identifying the source IP helped correlate the Windows Security Events with the testing activity performed from the Kali Linux machine.

## Step 9 - Analyze Multiple Failed Authentication Attempts

Multiple failed authentication attempts were generated during the controlled lab exercise.

The Windows Security Log recorded the authentication failures using Event ID 4625.

<img width="1167" height="863" alt="09-Multiple Failed Authentication Attempts" src="https://github.com/user-attachments/assets/b39debae-e80d-4828-85c3-eec976cdd663" />


Additional failed authentication information was reviewed to understand the repeated activity.

<img width="1217" height="885" alt="010-Multiple Failed Information" src="https://github.com/user-attachments/assets/251ee349-50d8-4908-a30a-c2979754e547" />


The repeated events were reviewed to understand:

* Number of authentication failures
* Target account
* Source IP address
* Logon type
* Authentication details
* Event timestamps

Repeated failed authentication activity from the same source can help SOC Analysts identify potential password-guessing or brute-force behavior.

## Step 10 - Correlate Attack Activity With Windows Logs

The authentication testing activity from Kali Linux was correlated with the Windows Security Events.

The investigation connected:

* Kali Linux source IP: 192.168.50.20
* Windows target IP: 192.168.50.10
* Target account: abc
* Failed authentication Event ID: 4625
* Successful RDP authentication Event ID: 4624
* RDP service: TCP 3389

This correlation demonstrated how SOC Analysts can combine network information and endpoint logs during an initial investigation.

# Key Windows Event IDs

| **Event ID** | **Description**  | **Investigation Purpose**               |
| ------------ | ---------------- | --------------------------------------- |
| **4624**     | Successful Logon | Monitor successful authentication       |
| **4625**     | Failed Logon     | Identify failed authentication attempts |

# Skills Demonstrated

* Windows Event Viewer navigation
* Windows Security Log analysis
* Authentication event analysis
* Event ID identification
* Failed authentication investigation
* Successful RDP authentication analysis
* Source IP identification
* Basic log correlation
* Network connectivity testing
* RDP service identification
* Nmap service scanning
* Hydra authentication testing
* Basic brute-force detection
* Security event investigation
* SOC Analyst log analysis
* Security documentation

# Key Findings

During the lab, the following activities were successfully observed and investigated:

* Successful RDP authentication was observed using **Event ID 4624 / Logon Type 10**
* Controlled authentication testing was performed from the Kali Linux VM
* Multiple failed authentication attempts were observed using **Event ID 4625**
* The failed authentication activity was associated with the Kali Linux source IP **192.168.50.20**
* The Windows target machine was identified as **192.168.50.10**
* RDP was confirmed to be available on **TCP port 3389**
* Authentication activity was correlated with the controlled testing performed in the isolated lab

These observations demonstrated how Windows Security Logs can provide useful telemetry for detecting and investigating authentication-related security activity.

# Lessons Learned

Through this hands-on project, I gained practical experience in simulating and investigating authentication-related security activity in a controlled Windows environment.

I learned how to identify exposed services, establish an RDP connection, perform controlled authentication testing, and analyze the Windows Security Logs generated by authentication activity.

The project also helped me understand how Event ID 4624 and Event ID 4625 can be used to investigate successful and failed authentication attempts.

By correlating the source IP address, target account, logon information, and event timestamps, I developed a better understanding of how SOC Analysts perform initial investigation and detection of suspicious authentication behavior.

This project provided a practical foundation for further learning in areas such as SIEM, threat detection, incident response, and threat hunting.


# Conclusion

This project provided hands-on experience with network connectivity testing, RDP service identification, controlled authentication testing, and Windows Security Log analysis.

By correlating Kali Linux activity with Windows authentication events, I developed a practical understanding of how SOC Analysts can detect and investigate repeated authentication failures.

The skills gained from this lab provide a foundation for further learning in areas such as SIEM, threat detection, incident response, and threat hunting.
