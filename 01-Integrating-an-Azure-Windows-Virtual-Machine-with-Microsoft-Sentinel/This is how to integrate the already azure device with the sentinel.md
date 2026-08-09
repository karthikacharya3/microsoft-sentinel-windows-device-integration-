# Integrating an Azure Windows Virtual Machine with Microsoft Sentinel

## Overview

This guide demonstrates how I integrated an Azure Windows Virtual Machine with Microsoft Sentinel to collect Windows Security Events for security monitoring and log analysis. By connecting the virtual machine to Microsoft Sentinel through a Log Analytics Workspace and the Azure Monitor Agent (AMA), security events can be centrally collected and analyzed using Kusto Query Language (KQL).

---

## Prerequisite

Before starting the integration, ensure you have:

* An active Microsoft Azure subscription.
* A Windows Virtual Machine already deployed in Azure.
* Microsoft Sentinel enabled.
* Appropriate permissions to create Azure resources.

---

## Step 1: Create a Log Analytics Workspace

The first step is to create a **Log Analytics Workspace**, which acts as the central repository for collecting and storing logs from Azure resources.

Navigate to:

```
Azure Portal
→ Create a Resource
→ Log Analytics Workspace
```

Provide the required details:

* **Workspace Name:** Log Analytics for Home Lab
* **Subscription:** Your Azure Subscription
* **Resource Group:** Your Resource Group
* **Region:** Same region as the virtual machine (recommended)

After providing the required information, click **Review + Create**, then **Create**.

---

## Step 2: Open Microsoft Sentinel

After the Log Analytics Workspace has been created:

1. Open **Microsoft Sentinel**.
2. Select the Log Analytics Workspace you created.
3. Microsoft Sentinel is now associated with that workspace and is ready to receive data.

---

## Step 3: Configure the Windows Security Events via AMA Data Connector

To collect Windows Security Events, configure the appropriate data connector.

Navigate to:

```
Microsoft Sentinel
→ Configuration
→ Data Connectors
→ Windows Security Events via AMA
```

Open the connector and click **Open Connector Page**.

---

## Step 4: Create a Data Collection Rule (DCR)

Click **Create Data Collection Rule** and configure the following settings:

* **Rule Name:** Enter a descriptive name for the Data Collection Rule.
* **Subscription:** Select your Azure subscription.
* **Resource Group:** Select the resource group containing your virtual machine.
* **Resources:** Choose the Azure Windows Virtual Machine you want to monitor.

Once the virtual machine is selected, continue to the next step.

---

## Step 5: Select the Windows Event Logs

Choose the Windows Event Logs you want Microsoft Sentinel to collect.

Common options include:

* Security Events
* Application Events
* System Events
* All Security Events

The logs you select here determine which Windows events will be collected and sent to the Log Analytics Workspace.

After selecting the required logs, complete the Data Collection Rule creation.

---

## Step 6: Verify Log Collection

Once the Data Collection Rule has been created successfully, the integration between the Azure Windows Virtual Machine and Microsoft Sentinel is complete.

Log ingestion may take a few minutes depending on the environment. In my lab, Windows Security Events started appearing in Microsoft Sentinel approximately **one minute** after completing the configuration.

You can verify successful ingestion by navigating to the **Logs** section in Microsoft Sentinel and running KQL queries to confirm that events are being received.

---

## Conclusion

In this project, I successfully integrated an Azure Windows Virtual Machine with Microsoft Sentinel by:

* Creating a Log Analytics Workspace.
* Connecting Microsoft Sentinel to the workspace.
* Configuring the **Windows Security Events via AMA** data connector.
* Creating a Data Collection Rule (DCR).
* Selecting the Windows event logs to collect.
* Verifying successful log ingestion into Microsoft Sentinel.

This integration enables centralized log collection, security monitoring, and threat investigation using Microsoft Sentinel and Kusto Query Language (KQL).
