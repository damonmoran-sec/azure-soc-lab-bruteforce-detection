Azure SOC Lab — Brute Force Detection and Account Lockout Response








Overview

This lab simulates a brute force login attack against a Windows Server virtual machine and demonstrates how Azure Monitor, Log Analytics, and KQL can be used to detect failed logon attempts, identify account lockouts, trigger an alert, and remotely respond by unlocking the account using Azure Run Command.

This mirrors a real SOC workflow from detection to response.

Architecture
Windows VM
   |
Azure Monitor Agent
   |
Data Collection Rule
   |
Log Analytics Workspace
   |
KQL Detection Queries
   |
Azure Alert Rule
   |
Run Command Response

Lab Requirements

Azure Windows Server VM

Azure Monitor Agent installed

Log Analytics Workspace

Data Collection Rule collecting Security Events

RDP access to generate failed logons

Step 1 — Verify Agent Communication (Heartbeat)

KQL Query:

Heartbeat
| take 10


This confirms the VM is successfully sending telemetry to Log Analytics.

Step 2 — Configure Data Collection Rule for Security Logs

The Data Collection Rule is configured to collect Windows Security Event logs and send them to the Log Analytics Workspace.

Step 3 — Generate Failed Logon Attempts (Event ID 4625)

Multiple failed RDP logins are performed to simulate a brute force attack.

KQL Query:

Event
| where EventLog == "Security"
| where EventID == 4625
| take 20


Step 4 — Account Lockout Occurs (Event ID 4740)

After enough failed attempts, the account becomes locked.

KQL Query:

Event
| where EventID == 4740


Step 5 — Log Analytics Confirms Lockout Event

The lockout event is successfully ingested into the workspace from the VM.

Step 6 — Alert Rule Triggers

An Azure alert rule monitors for Event ID 4740 and fires when the account lockout occurs.

Step 7 — SOC Response Using Run Command

Instead of logging into the VM, Azure Run Command is used to unlock the account remotely.

PowerShell used:

$user = "dpmoran11"
$u = [ADSI]"WinNT://./$user,user"
$u.IsAccountLocked = $false
$u.SetInfo()
"Unlocked user: $user"


KQL Queries Used
Heartbeat | take 10

Event | where EventLog == "Security" | where EventID == 4625

Event | where EventID == 4740

Lessons Learned

How Windows security events flow into Azure Monitor

How to detect brute force patterns using KQL

How to detect account lockouts in real time

How to create alert rules tied to specific security events

How to perform remote incident response without RDP access

How this process mirrors real SOC detection and response workflows

Screenshots Reference

All screenshots for this lab are stored in the screenshots folder and referenced throughout each step.
