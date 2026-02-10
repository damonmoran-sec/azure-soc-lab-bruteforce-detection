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
![Heartbeat](screenshots/01-heartbeat.png)

### Data Collection Rule Configuration
![DCR](screenshots/02-data-collection-rule.png)

### Failed Logon Attempts (Event ID 4625)
![4625](screenshots/03-failed-logon-attempt-log.png)

### Account Lockout on VM (Event ID 4740)
![4740 VM](screenshots/04-account-lockout-vmlog.png)

### Account Lockout in Log Analytics (Event ID 4740)
![4740 LAW](screenshots/05-law-event4740-ingested.png)

### Alert Fired in Azure Monitor
![Alert](screenshots/06-alert-fired.png)

### Run Command Unlock Response
![Run Command](screenshots/07-run-command-unlock.png)

---

## Notes
This lab was completed using only built in Azure monitoring features and a free Azure subscription. It demonstrates a full detection and response workflow similar to what is performed in real SOC environments.
