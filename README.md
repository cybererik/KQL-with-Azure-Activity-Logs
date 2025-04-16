# KQL-with-Azure-Activity-Logs

![Introduction to Microsoft Sentinel (SIEM)](https://github.com/user-attachments/assets/8038f7fc-9926-4997-91af-3302466cb45c)


## Overview
This project dives into Azure Log Analytics to create a KQL (Kusto Query Language) based solution for detecting suspicious **DELETE** operations in Azure environments. It's designed to help blue teams monitor unusual behavior patterns, such as mass resource deletions, which could indicate a potential compromise or insider threat.

We focus on summarizing and filtering logs from `AzureActivity`, looking specifically for **DELETE** operations marked as **"Success"** within a defined time window. Then, we drill down into a specific caller for deeper investigation.

---
## 🕵️‍♂️ Hypothesis
The security team suspects that the deletions took place within the last 1–2 hours, based on reports from multiple end users indicating that the file server (the VM) is currently inaccessible. This project simulates a security operations team responding to the disappearance of multiple resources from a subscription. The team needs to:
- Identify **who** deleted the resources,
- Verify **how many** deletions were made,
- And see **when** and **what** resources were deleted.

---
## Tools & Technologies
- **Microsoft Azure** (Virtual Machine Provisioning & Log Analytics Workspace)
- **Microsoft Sentinel (SIEM)** (Log Collection)

**Log Analytics Workspace:** In the following queries, we’re tapping into Azure's `AzureActivity` table, which logs management plane actions like creating, modifying, or deleting resources. This table captures detailed records of these activities, including who performed them and when, making it a valuable tool for monitoring and auditing what's happening in your Azure environment.

------
## Initial Investigation
![Screenshot 2025-04-08 043233](https://github.com/user-attachments/assets/02cd7ef6-f626-41b3-bff4-1c9736f348b1)


## 📊 Query #1 Results: Detecting Successful DELETE Operations
The results reveal that several callers successfully deleted resources, with the number of deletion records surpassing the set threshold (1). To narrow down our investigation, we decided to focus on the specific caller: `9e3afd45a84b1308bbb838fd9c02c6d0b48a98d1174e99cdf13c93b73849528d@lognpacific.com`.

Based on the data, identified by their caller. The query filters for successful "DELETE" operations within the last 2 hours, grouping by the caller and resource group, and flags any activity exceeding the defined threshold.

**Query #1 Explanation** - [Click to View](https://github.com/cybererik/KQL-with-Azure-Activity-Logs/blob/main/KQL%20Query%3A%20Detecting%20Successful%20DELETE%20Operations)

-------
## Filtering Activity Logs For Specific Caller
![Screenshot 2025-04-08 043533](https://github.com/user-attachments/assets/7b88c7ff-a5b0-49e1-a4d2-a2f33a416c3f)

## 📊 Query #2 Results: Successful Activities by Specific Caller
This query focuses on the activities of a specific caller within the last 2 hours, highlighting successful operations. It helps us spot patterns or suspicious behavior, such as deletions that may be part of a larger series of actions. The goal is to detect any abnormal actions that might indicate a compromised account or unauthorized behavior.

### Operations Performed by the Caller:
- **MICROSOFT.NETWORK/PUBLICIPADDRESS/DELETE**
- **MICROSOFT.NETWORK/NETWORKINTERFACES/DELETE**
- **MICROSOFT.COMPUTE/DISKS/DELETE**
- **MICROSOFT.COMPUTE/VIRTUALMACHINES/DELETE**

These actions were successfully executed on **4/8/2025**. The caller was responsible for deleting critical resources in the Azure environment, including network interfaces and virtual machines—an unusual activity that requires further investigation.

**Query #2 Explanation** - [Click to View](https://github.com/cybererik/KQL-with-Azure-Activity-Logs/blob/main/KQL%20Query%3A%20Successful%20Activities%20by%20Specific%20Caller)

-----

## 🧠 Conclusion
This project demonstrates how to use KQL within Azure Log Analytics to detect suspicious DELETE operations in an Azure environment. It simulates a security operations team responding to the disappearance of multiple resources from a subscription. The team’s objectives are to:

- **Identify who deleted the resources**
- **Determine how many deletions occurred**
- **Understand when and what resources were deleted**

The project filters logs from AzureActivity, specifically targeting successful DELETE actions performed by a defined caller within the last 2 hours. By analyzing these logs, we can spot unusual activities such as mass deletions, which could signal a potential security incident or insider threat.

The two main queries focus on:

1) **Detecting DELETE operations with a count threshold to flag unusual activity.**

2) **Investigating a specific caller to identify detailed operations and potential malicious behavior.**

By leveraging these techniques, security teams can efficiently detect abnormal actions, helping to mitigate risks and enhance security monitoring within Azure environments.












