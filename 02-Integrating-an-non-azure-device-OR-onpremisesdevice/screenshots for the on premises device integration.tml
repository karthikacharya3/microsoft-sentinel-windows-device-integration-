# Onboarding an On-Premises Windows Device to Microsoft Sentinel Using Azure Arc

## Overview

In this project, I integrated an **on-premises Windows device** with **Microsoft Sentinel** using **Azure Arc-enabled Servers**. After onboarding the machine through Azure Arc, I configured **Microsoft Monitoring Agent (AMA)** and connected the device to Sentinel using a **Data Collection Rule (DCR)** to collect Windows Security Events.

This setup allows Microsoft Sentinel to monitor security events from machines that are not hosted directly in Azure.

---

# Architecture Flow

```
On-Premises Windows Device
          |
          |
 Azure Connected Machine Agent
          |
          |
       Azure Arc
          |
          |
 Azure Monitor Agent (AMA)
          |
          |
 Data Collection Rule (DCR)
          |
          |
 Microsoft Sentinel
          |
          |
 Security Logs / KQL Queries
```

---

# Step 1: Open Azure Arc Machines

1. Login to the Azure Portal.
2. Search for:

```
Azure Arc
```

3. Navigate to:

```
Infrastructure → Machines
```

4. Initially, no machines were available because no on-premises devices were onboarded.

5. Click:

```
Create / Onboard
```

---

# Step 2: Select Onboarding Option

Azure provides two options:

### Option 1:

**Onboard existing machines**

> Onboard existing servers or virtual machines from any of your environments.

### Option 2:

**Create a machine in a connected host environment**

> Create a virtual machine in your connected host environments.

For this integration, select:

```
Onboard existing machines
```

---

# Step 3: Configure Machine Details

Under the **Basics** page:

1. Select the Azure subscription.
2. Select the resource group.
3. Enter the server details.
4. Select the operating system:

```
Windows
```

5. Complete all required fields.
6. Click through the validation process.

After successful validation, Azure generates a PowerShell script.

This script needs to be executed on the Windows device that we want to onboard.

---

# Step 4: Execute Azure Arc Script on Windows Device

When running the generated script in PowerShell, I encountered an issue.

## Error:

```
This script needs administrator permission to install the Azure Connected Machine Agent.
Please run this script as administrator.
```

Even after running PowerShell as Administrator, another error appeared:

```
Cannot be loaded because running scripts is disabled on this system.
```

---

# Step 5: Enable PowerShell Script Execution

To resolve this issue, I enabled PowerShell script execution.

Run PowerShell as Administrator and execute:

```powershell
Set-ExecutionPolicy RemoteSigned
```

PowerShell asks for confirmation:

```
Do you want to change the execution policy?
```

Options:

```
Y - Yes
N - No
A - Yes to All
L - No to All
```

Select:

```
Y
```

Now script execution is enabled.

---

# Step 6: Run Azure Arc Installation Script

1. Go back to Azure Portal.
2. Copy the Azure Arc onboarding script again.
3. Paste it into Administrator PowerShell.
4. Wait for the installation process.

The execution progress will appear:

```
10%
20%
30%
...
100%
```

After completion, you will receive a message similar to:

```
Connected machine to Azure successfully
```

---

# Network Requirement

The onboarded machine requires outbound communication to Azure.

The required port:

```
TCP 443
```

should be allowed.

The required Azure URLs must also be permitted through the firewall because Azure Arc communication is URL-based.

---

# Step 7: Verify Azure Arc Machine

After script execution:

1. Login again to Azure Portal.
2. Navigate to:

```
Azure Arc → Machines
```

3. Refresh the page.

The onboarded device should appear.

Click on the machine name to view:

* Hostname
* Operating System details
* Machine information
* Connection status

---

# Monitoring Configuration

Inside the Azure Arc machine page, there is an option called:

```
Monitor
```

This feature provides monitoring for:

* CPU utilization
* Memory utilization
* Performance metrics

By default, this is not configured and requires additional configuration/cost.

I did not configure performance monitoring in this implementation.

---

# Step 8: Configure Microsoft Sentinel Data Connector

Navigate to:

```
Microsoft Sentinel
→ Configuration
→ Data Connectors
```

Previously, I installed:

```
Windows Security Events via AMA
```

Open the data connector and refresh the page.

---

# Step 9: Create / Update Data Collection Rule (DCR)

The Data Collection Rule controls what logs are collected from devices.

Since both devices are Windows systems:

* Azure VM Windows machine
* On-premises Windows machine

I reused the existing Data Collection Rule.

Open:

```
Windows Security Events via AMA
→ Edit
```

Navigate through:

```
Basics
Resources
Collect
Review + Create
```

---

# Resources Configuration

Under the Resources section:

I selected:

```
Subscription
   |
   Resource Group
        |
        Windows VM
        |
        On-Premises Azure Arc Machine
```

Select the newly onboarded Azure Arc machine.

Click:

```
Next
```

---

# Data Collection Settings

Select the required security events:

Options include:

* All Security Events
* Common
* Minimal
* Custom

Choose based on your monitoring requirements.

After configuration:

```
Review + Create
```

Click:

```
Create
```

---

# Step 10: Verify Agent Installation

After creating the rule, Azure displays:

```
Successfully installed extension
```

and:

```
Data Collection Rule associated successfully
```

Now verify the agent installation.

For Azure VM:

```
Virtual Machine
→ Extensions
```

For on-premises machine:

```
Azure Arc
→ Machines
→ Select Device
→ Extensions
```

Check:

* Monitoring Agent status
* Installation status
* Extension enabled status

Expected result:

```
Status: Success
Automatic Upgrade: Enabled
```

---

# Step 11: Verify Logs in Microsoft Sentinel

After completing the configuration, verify that logs are reaching Sentinel.

Run the KQL query:

```kql
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count() by Computer
```

The onboarded on-premises device should appear in the results.

This confirms successful communication between:

```
On-Premises Device
        |
        |
Azure Arc
        |
        |
Azure Monitor Agent
        |
        |
Microsoft Sentinel
```

---

# Conclusion

Successfully integrated an on-premises Windows device with Microsoft Sentinel using Azure Arc.

The complete workflow included:

✅ Azure Arc machine onboarding
✅ Azure Connected Machine Agent installation
✅ PowerShell execution policy troubleshooting
✅ Azure Monitor Agent configuration
✅ Data Collection Rule association
✅ Windows Security Event collection
✅ Sentinel log verification using KQL

This implementation allows Microsoft Sentinel to monitor and investigate security events from machines outside Azure environments.
