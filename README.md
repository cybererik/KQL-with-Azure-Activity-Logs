# KQL-with-Azure-Activity-Logs

<div style="text-align: center;">
    <img src="https://github.com/user-attachments/assets/7eb1ee65-c753-46f6-ac05-9ba49c80ada3" alt="image" style="width: 80%; max-width: 800px;">
</div>

## Overview
This project dives into Azure Log Analytics to create a KQL (Kusto Query Language) based solution for detecting suspicious **DELETE** operations in Azure environments. It's designed to help blue teams monitor unusual behavior patterns, such as mass resource deletions, which could indicate a potential compromise or insider threat.

We focus on summarizing and filtering logs from `AzureActivity`, looking specifically for **DELETE** operations marked as **"Success"** within a defined time window. Then, we drill down into a specific caller for deeper investigation.

---
## 🕵️‍♂️ Scenario
The security team estimates that the deletions occurred about 1-2 hours ago, as multiple end users rely on the file server (in this case, the virtual machine) for their daily tasks. This project simulates a security operations team responding to the disappearance of multiple resources from a subscription. The team needs to:
- Identify **who** deleted the resources,
- Verify **how many** deletions were made,
- And see **when** and **what** resources were deleted.
---
## Log Analytics Workspace

![Screenshot 2025-04-08 043233](https://github.com/user-attachments/assets/02cd7ef6-f626-41b3-bff4-1c9736f348b1)

## 📊 Query 1: Detecting Successful DELETE Operations

We’re using Azure's `AzureActivity` table which logs control plane operations, like resource creations or deletions.

### Step 1: Detect Unusual DELETE Patterns

```kql
let resource_threshold = 1;
let time_threshold_in_hours = 2h;
AzureActivity
| where TimeGenerated > ago(time_threshold_in_hours)
| where OperationNameValue endswith "DELETE" and ActivityStatusValue == "Success"
| summarize number_of_records = count() by Caller, ActivityStatusValue, ResourceGroup
| where number_of_records > resource_threshold


