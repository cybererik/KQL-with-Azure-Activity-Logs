# KQL-with-Azure-Activity-Logs

## Overview
This project dives into Azure Log Analytics to create a KQL (Kusto Query Language) based solution for detecting suspicious **DELETE** operations in Azure environments. It's designed to help blue teams monitor unusual behavior patterns, such as mass resource deletions, which could indicate a potential compromise or insider threat.

We focus on summarizing and filtering logs from `AzureActivity`, looking specifically for **DELETE** operations marked as **"Success"** within a defined time window. Then, we drill down into a specific caller for deeper investigation.

---
## Tools & Technologies
- Microsoft Azure (Virtual Machine Provisioning)
- Microsoft Azure (Log Analysis Workspace)

## 🧠 Problem Statement
Imagine you're on a security operations team and suddenly multiple resources vanish from a subscription. You need to:
- Identify **who** deleted the resources,
- Verify **how many** deletions were made,
- And see **when** and **what** resources were deleted.

That’s exactly what this project tackles.

---

## 🛠️ Query Breakdown

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
