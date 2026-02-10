Azure SOC Lab: Detecting and Responding to Account Lockout Attacks

This lab simulates a very common real world SOC scenario. A user account gets locked out after multiple failed logon attempts. Instead of just fixing it, I built monitoring around it so Azure could detect it, alert on it, and let me respond to it like a SOC analyst would.

The goal was not just to unlock an account. The goal was to build the visibility pipeline that shows how security events move from a VM into Log Analytics, trigger alerts, and then be investigated and remediated.

Step 1: Verify the VM is sending telemetry (Heartbeat)

Before doing anything security related, I verified the Azure Monitor Agent was properly communicating with Log Analytics.

KQL used:

Heartbeat
| take 10


This confirmed the VM was actively reporting to the workspace.

Step 2: Configure the Data Collection Rule

I created a Data Collection Rule to send Windows Security Event Logs into the Log Analytics workspace. This is what allows Azure to see authentication activity from the VM.

Without this step, there is no visibility into failed logons or account lockouts.

Step 3: Generate Failed Logon Attempts (Event ID 4625)

I intentionally entered the wrong password multiple times over RDP to simulate a brute force style login attempt.

KQL used:

Event
| where EventLog == "Security"
| where EventID == 4625
| take 20


This shows multiple failed authentication attempts.

Step 4: Confirm Account Lockout on the VM (Event ID 4740)

After enough failed attempts, the account locked. I verified this directly on the VM logs.

KQL used:

Event
| where EventID == 4740
| sort by TimeGenerated desc


This proves the account lockout occurred on the system itself.

Step 5: Verify the Lockout Event Reached Log Analytics

Next, I confirmed that the same Event ID 4740 was ingested into the Log Analytics workspace. This proves the monitoring pipeline is working end to end.

Step 6: Create an Alert for Multiple Failed Logons

I created an Azure Monitor alert rule that triggers when multiple failed logons are detected. Shortly after the failed attempts, the alert fired.

This shows how a SOC team would be notified of suspicious authentication behavior.

Step 7: Respond Using Run Command to Unlock the Account

Instead of logging into the VM, I used Azure Run Command to remotely execute PowerShell to unlock the account.

PowerShell used:

$user = "dpmoran11"

$u = [ADSI]"WinNT://./$user,user"
$u.IsAccountLocked = $false
$u.SetInfo()

"Unlocked user: $user"


This simulates remote incident response without direct RDP access.

What This Lab Demonstrates

This lab shows the full security monitoring lifecycle:

VM telemetry collection

Security log ingestion into Log Analytics

Detection of failed logon attempts

Detection of account lockout events

Alert creation and triggering

Remote response using Azure tools

This is exactly how a SOC analyst investigates authentication based alerts in a real environment.

Key Skills Demonstrated

Azure Monitor Agent

Data Collection Rules

Log Analytics and KQL

Security Event Monitoring

Azure Alert Rules

Incident style response using Run Command

Understanding Windows Security Event IDs 4625 and 4740

This lab helped me understand not just how to configure monitoring, but how to think like a defender when authentication abuse happens inside an environment.
