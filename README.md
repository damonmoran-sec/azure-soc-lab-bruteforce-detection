# Azure SOC Brute Force Detection and Account Lockout Lab

## Objective
This lab demonstrates how to monitor Windows Security Event logs from a virtual machine in Microsoft Azure using Azure Monitor, Data Collection Rules, Log Analytics, and alert rules. It also shows how to detect failed logon attempts, identify an account lockout, trigger an alert, and respond to the incident using Azure Run Command.

## Environment
- Microsoft Azure Free Subscription
- Windows Server Virtual Machine (SOCVM)
- Azure Monitor Agent
- Log Analytics Workspace (SOC-LAW)
- Data Collection Rule (DCR)

## Lab Steps

### 1. Verify Azure Monitor Agent Communication
Used the Heartbeat query to confirm the VM was actively sending telemetry to Log Analytics.

### 2. Configure Data Collection Rule
Configured a Data Collection Rule to collect Windows Security Event logs and send them to the Log Analytics workspace.

### 3. Generate Failed Logon Attempts (Event ID 4625)
Performed multiple failed RDP login attempts to simulate a brute force attack and verified Event ID 4625 in Log Analytics.

### 4. Identify Account Lockout (Event ID 4740)
Confirmed the account lockout event on the VM and verified the same event was ingested into Log Analytics.

### 5. Create Azure Monitor Alert Rule
Created an alert rule that triggers when multiple failed logon attempts occur within a short time window.

### 6. Trigger the Alert
Intentionally locked the account by repeated failed logons and observed the alert fire in Azure Monitor.

### 7. Respond Using Azure Run Command
Used Azure Run Command with PowerShell to unlock the account remotely without using RDP.

## Skills Demonstrated
- Azure Monitor Agent configuration
- Data Collection Rules
- Log Analytics and KQL queries
- Windows Security Event monitoring
- Alert rule creation and testing
- Incident response using Azure Run Command
- Understanding Event IDs 4625 and 4740

## Screenshots

### Agent Heartbeat (VM sending logs)
<img width="1906" height="950" alt="01-heartbeat png" src="https://github.com/user-attachments/assets/df8ef5a2-8ee4-40e5-a95d-40230dafe60b" />


### Data Collection Rule Configuration
<img width="1913" height="907" alt="02-data-collection-rule png" src="https://github.com/user-attachments/assets/90cf53eb-188d-47af-8197-4be8be6fde57" />

### Failed Logon Attempts (Event ID 4625)
<img width="1911" height="902" alt="03-failed-logon-attempt-log png" src="https://github.com/user-attachments/assets/91f66526-d304-4d3c-8995-5e06319ae85a" />


### Account Lockout on VM (Event ID 4740)
<img width="1909" height="899" alt="04-account-lockout-vmlog png" src="https://github.com/user-attachments/assets/17722b3b-3de4-41f6-84c9-bda36c005162" />


### Account Lockout in Log Analytics (Event ID 4740)
<img width="1904" height="901" alt="05-law-event4740-ingested png" src="https://github.com/user-attachments/assets/386fc077-18cb-4750-87a9-4fc4408cd850" />


### Alert Fired in Azure Monitor
<img width="1912" height="952" alt="06-alert-fired png" src="https://github.com/user-attachments/assets/c00ae451-53c8-4972-b3a3-9977875ead2b" />


### Run Command Unlock Response
<img width="1911" height="907" alt="07-run-command-unlock png" src="https://github.com/user-attachments/assets/00297470-d8d7-4e4e-8a9d-c5c1a294ea49" />


---

## Notes
This lab was completed using only built in Azure monitoring features and a free Azure subscription. It demonstrates a full detection and response workflow similar to what is performed in real SOC environments.
