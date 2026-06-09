# 🛡️ Sysmon & Splunk Monitoring Lab | Centralised Windows Security Monitoring

## Overview

This lab demonstrates how to deploy Splunk Enterprise on Ubuntu Desktop, install Sysmon on Windows Server 2025, and configure Splunk Universal Forwarder to forward Windows Event Logs and Sysmon telemetry to a centralised SIEM platform.

The objective was to build a security monitoring environment capable of collecting, centralising, and analysing endpoint activity from a Windows host.

This project demonstrates core SOC Analyst skills including:

* SIEM deployment
* Endpoint monitoring
* Log collection
* Event analysis
* Security monitoring
* Troubleshooting log ingestion issues

---

# Lab Architecture

![Lab Architecture](screenshots/01-lab-architecture.png)

**Figure 1.** Lab topology showing Windows Server 2025 forwarding Windows Event Logs and Sysmon telemetry to Splunk Enterprise hosted on Ubuntu Desktop.

# Lab Objectives

* Deploy Splunk Enterprise on Ubuntu
* Configure Splunk as a receiving indexer
* Install Sysmon on Windows Server
* Install Splunk Universal Forwarder
* Forward Windows Event Logs
* Forward Sysmon Operational Logs
* Create a dedicated Splunk index
* Validate event ingestion
* Monitor endpoint activity

---

# Environment

| Component                | IP Address  | Purpose                      |
| ------------------------ | ----------- | ---------------------------- |
| Ubuntu Desktop 24.04 LTS | 10.0.70.104 | Splunk Enterprise            |
| Windows Server 2025      | 10.0.70.2   | Sysmon + Universal Forwarder |
| Proxmox VE               | ---         | Virtualisation Platform      |

---

# Phase 1 – Ubuntu Installation

## Download Ubuntu

Official Website:

https://ubuntu.com/download/desktop

Version Used:

```text
Ubuntu Desktop 24.04 LTS
```

---

## Create VM in Proxmox

Configuration:

```text
CPU: 2 vCPU
RAM: 8 GB
Disk: 80 GB
Network: VLAN70
```

![Ubuntu VM](screenshots/02-ubuntu-vm.png)

**Figure 2.** Ubuntu Desktop virtual machine configuration deployed within Proxmox.

---

## Install Ubuntu

Selected:

```text
✓ Install third-party software for graphics and Wi-Fi hardware

✓ Download and install support for additional media formats
```

---

## Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Phase 2 – Splunk Enterprise Installation

## Download Splunk Enterprise

Downloaded the Linux `.tgz` package from Splunk.

Example:

```text
splunk-10.4.0-f798d4d49089-linux-amd64.tgz
```

---

## Extract Splunk

Navigate to Downloads:

```bash
cd ~/Downloads
```

Extract archive:

```bash
tar -xvzf /home/cyber/Downloads/splunk-10.4.0-f798d4d49089-linux-amd64.tgz
```

Verify extraction:

```bash
ls
```

Expected:

```text
splunk
```

---

## Move Splunk to /opt

```bash
sudo mv splunk /opt/
```

Verify:

```bash
ls /opt
```

Expected:

```text
splunk
```

---

## Verify Installation Files

```bash
ls /opt/splunk/bin
```

Expected:

```text
splunk
splunkd
```

---

# Phase 3 – First Startup

Start Splunk:

```bash
sudo /opt/splunk/bin/splunk start --accept-license --run-as-root
```

Accept the licence agreement and create the administrator account.

---

## Enable Startup

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

---

## Verify Splunk Status

```bash
sudo /opt/splunk/bin/splunk status
```

Expected:

```text
splunkd is running
```

![Splunk Running](screenshots/03-splunk-running.png)

**Figure 3.** Verification showing Splunk services successfully running on Ubuntu.

---

# Phase 4 – Access Splunk Web

Open:

```text
http://10.0.70.104:8000
```

or

```text
http://cyber:8000
```

Login using the administrator credentials created during installation.

![Splunk Web Login](screenshots/04-splunk-web-login.png)

**Figure 4.** Splunk Enterprise web interface accessible from the browser.

---

# Phase 5 – Configure Splunk Receiver

Navigate:

```text
Settings
→ Forwarding and Receiving
→ Configure Receiving
→ New Receiving Port
```

Port:

```text
9997
```

Save configuration.

![Receiving Port Configuration](screenshots/05-receiving-port-9997.png)

**Figure 5.** Splunk configured to receive forwarded events on TCP port 9997.

---

## Verify Receiver

Ubuntu:

```bash
sudo ss -tulpn | grep 9997
```

Expected:

```text
LISTEN *:9997
```

---

# Phase 6 – Install Sysmon

## Download Sysmon

Downloaded Sysmon from Microsoft Sysinternals.

https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon

---

## Extract Files

Created:

```text
C:\Tools\Sysmon
```

Extracted Sysmon files into the folder.

---

## Install Sysmon

Open Command Prompt as Administrator:

```cmd
cd C:\Tools\Sysmon

sysmon64.exe -accepteula -i
```

![Sysmon Installation](screenshots/06-sysmon-installation.png)

**Figure 6.** Successful Sysmon installation on Windows Server 2025.

---

## Verify Sysmon

Open:

```text
Event Viewer
```

Navigate:

```text
Applications and Services Logs
→ Microsoft
→ Windows
→ Sysmon
→ Operational
```

Generate activity:

```cmd
notepad.exe
```

Verify events appear.

![Sysmon Operational Log](screenshots/07-sysmon-operational-log.png)

# Phase 7 – Install Splunk Universal Forwarder

Download:

https://www.splunk.com/en_us/download/universal-forwarder.html

Install on Windows Server.

---

## Universal Forwarder Configuration

Installation Type:

```text
On-Premises Splunk Enterprise Instance
```

Service Account:

```text
Local System
```

Receiving Indexer:

```text
10.0.70.104
```

Receiving Port:

```text
9997
```

![Universal Forwarder Installation](screenshots/08-universal-forwarder-installation.png)

**Figure 8.** Splunk Universal Forwarder installation and configuration on Windows Server 2025.

---

## Verify Forwarding

After installation, the forwarding configuration was verified from PowerShell.

Command:

```powershell
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" list forward-server
```

Output observed during testing:

```text
Active forwards:
none

Configured but inactive forwards:
10.0.70.104:9997
```

![Forward Server Configuration](screenshots/09-forward-server-config.png)

**Figure 9.** Universal Forwarder forwarding configuration showing the configured Splunk Enterprise receiver.

### Validation

Although the command reported the receiver as *configured but inactive*, Splunk Enterprise continuously received Windows Event Logs and Sysmon telemetry.

The following validation steps confirmed successful communication:

1. Verified Splunk was listening on TCP port 9997.
2. Confirmed Windows Event Logs were arriving in Splunk.
3. Confirmed Sysmon events were arriving in Splunk.
4. Generated new endpoint activity and verified events appeared in near real-time.

Search used for validation:

```spl
index=wineventlog
```

![Events Arriving in Splunk](screenshots/10-events-arriving.png)

**Figure 10.** Windows Event Logs and Sysmon telemetry successfully received and indexed by Splunk Enterprise.

Example activities generated during testing:

```cmd

notepad.exe

nslookup google.com
```

These events were visible within the `wineventlog` index shortly after execution.

### Lessons Learned

During testing, the Universal Forwarder command-line utility reported the receiver as inactive despite successful event ingestion.

This demonstrated that service status outputs should always be validated against actual data flow. From a SOC Analyst perspective, the most important verification is whether logs are successfully reaching the SIEM and are available for investigation and detection activities.

---

## Verify Inputs

Command:

```powershell
& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" btool inputs list --debug | findstr WinEventLog
```

Expected:

```text
[WinEventLog://Security]
[WinEventLog://System]
[WinEventLog://Application]
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
```

![inputs.conf Configuration](screenshots/11-inputs-conf.png)

**Figure 11.** inputs.conf configuration used to collect Windows Event Logs and Sysmon telemetry.

---

# Troubleshooting Notes

## Issue 1 – Splunk Required Root Privileges

Error:

```text
Use the --run-as-root option
```

Resolution:

```bash
sudo /opt/splunk/bin/splunk start --accept-license --run-as-root
```

---

## Issue 2 – inputs.conf Not Loading

Issue:

```text
inputs.conf.txt
```

instead of:

```text
inputs.conf
```

Resolution:

Enabled Windows file extensions and renamed the file correctly.

---

## Issue 3 – Events Not Appearing in Splunk

Splunk Notification:

```text
Received event for unconfigured/deleted index=wineventlog
Dropping them
```

Cause:

```text
wineventlog index did not exist
```

Resolution:

Created the wineventlog index.

---

# Phase 10 – Verify Event Ingestion

Search:

```spl
index=wineventlog
```

Successfully observed:

* Security Events
* System Events
* Application Events
* Sysmon Events

![Events Arriving in Splunk](screenshots/12-events-arriving.png)

**Figure 12.** Windows Event Logs and Sysmon telemetry successfully ingested into the wineventlog index.

---

# Phase 11 – Detection Validation

## Process Creation Monitoring

Generate activity:

```cmd
notepad.exe
```

Search:

```spl
index=wineventlog EventCode=1 notepad.exe
```

Expected:

```text
Sysmon Event ID 1
Process Creation
```

![Process Creation Detection](screenshots/13-process-creation-detection.png)

**Figure 13.** Sysmon Event ID 1 displaying process creation activity detected within Splunk.

---

## Additional Process Creation Validation

Generate activity:

```cmd
nslookup google.com
```

Search:

```spl
index=wineventlog EventCode=1 nslookup
```

Expected:

```text
DNS Query Events
```

![DNS Query Detection](screenshots/14-process-creation-detection.png)

**Figure 14.** Sysmon Event ID 1 displaying the execution of nslookup.exe detected and searchable within Splunk.

---

# Useful Splunk Searches

## All Sysmon Events

```spl
index=wineventlog sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational"
```

## Process Creation Events

```spl
index=wineventlog EventCode=1
```

## Network Connections

```spl
index=wineventlog EventCode=3
```

## DNS Queries

```spl
index=wineventlog EventCode=22
```

## PowerShell Activity

```spl
index=wineventlog EventCode=1 Image="*powershell.exe"
```

---

# Skills Demonstrated

* Ubuntu Administration
* Splunk Enterprise Deployment
* Splunk Administration
* Splunk Receiver Configuration
* Splunk Universal Forwarder
* Windows Event Collection
* Sysmon Deployment
* Endpoint Monitoring
* Log Ingestion Troubleshooting
* Security Event Analysis
* SIEM Operations
* Detection Engineering Fundamentals
* SOC Analyst Skills

---

# Outcome

Successfully deployed a centralised logging and monitoring environment using Splunk Enterprise, Sysmon, and Splunk Universal Forwarder.

Windows Event Logs and Sysmon telemetry were collected, forwarded, indexed, and analysed through Splunk Search & Reporting.

This project provides a foundation for future threat detection, threat hunting, and SOC investigation labs.
