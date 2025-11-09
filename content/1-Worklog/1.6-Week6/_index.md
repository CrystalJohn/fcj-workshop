---
title: "Week 6 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice warning %}} 
⚠️ **Note:** The following information is for reference purposes only. Please **do not copy verbatim** for your own report, including this warning.
{{% /notice %}}


### Week 6 Objectives:

* Understand knowledge related to Route 53 in the Reliability pillar of the AWS Well-Architected Framework.

### Tasks to be completed this week:
| Day | Task                                                                                                                                                                                                 | Start Date  | End Date    | Reference Material                         |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ----------- | ------------------------------------------ |
| 2   | - Get familiar with Route 53 <br>&emsp; + Definition and functions                                                                                             | 13/10/2025  | 13/10/2025  |
| 3   | - **Practice:** <br>&emsp; - Set up Hybrid DNS with Route 53 Resolver                                                                                        | 14/10/2025  | 14/10/2025  | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn about concepts: <br> + Principle of Least Privilege (grant only necessary permissions) <br> + How KMS (Key Management Service) works                  | 15/10/2025  | 15/10/2025  | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn about Amazon DynamoDB and practice lab on DynamoDB <br> - Learn about creating Global Secondary Index                                                | 16/10/2025  | 16/10/2025  | <https://000060.awsstudygroup.com/vi/1-introduce/> |
| 6   | - **Practice:** <br>&emsp; + Develop with Python and DynamoDB                                                                                                | 17/10/2025  | 17/10/2025  | <https://000060.awsstudygroup.com/vi/3-gettingstartedwithawssdk/3.2-pythonandynamodb/> |

### Week 6 Achievements:
* Understood how to design DNS systems with Route 53 Resolver
  * Used AWS Quick Start to deploy Hybrid DNS
  * Used AWS Managed Microsoft Active Directory
Practice result: https://drive.google.com/drive/folders/1-ZtnhA9PyAjANC9loPAZApB7TQHBCAox?usp=sharing

* Learned that creating a Global Secondary Index (GSI) in DynamoDB allows for fast queries based on attributes other than the primary key (PK). Instead of using PK with Partition Key and Sort Key for queries:
  * Partition Key: Attribute used to distribute data across physical storage partitions.
  * Sort Key: Attribute used to sort and filter data within the same partition.
