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
- 
---
## Log Analytics Workspace
In this query, we’re tapping into Azure's `AzureActivity` table, which logs control plane actions like creating, modifying, or deleting resources. This table captures detailed records of these activities, including who performed them and when, making it a valuable tool for monitoring and auditing what's happening in your Azure environment.

![Screenshot 2025-04-08 043233](https://github.com/user-attachments/assets/02cd7ef6-f626-41b3-bff4-1c9736f348b1)


## 📊 Query #1 Results: Detecting Successful DELETE Operations
The results reveal that several callers successfully deleted resources, with the number of deletion records surpassing the set threshold (1). To narrow down our investigation, we decided to focus on the specific caller (me): `9e3afd45a84b1308bbb838fd9c02c6d0b48a98d1174e99cdf13c93b73849528d@lognpacific.com`.

Based on the data, identified by their caller. The query filters for successful "DELETE" operations within the last 2 hours, grouping by the caller and resource group, and flags any activity exceeding the defined threshold.

**Query #1 Explanation** - [Click to View](https://github.com/cybererik/KQL-with-Azure-Activity-Logs/blob/main/KQL%20Query%3A%20Detecting%20Successful%20DELETE%20Operations)




