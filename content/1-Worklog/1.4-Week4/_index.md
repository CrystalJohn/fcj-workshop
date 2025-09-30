---
title: "Week 4 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

- Deploy applications on Linux Instance

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                         | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Basic knowledge of virtual servers with Amazon Elastic Compute Cloud (EC2) + Clean resource                                                                                                                | 29/09/2025 | 29/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Go to office <br>&emsp; - **Practice:** <br>&emsp; + Basic Amazon EC2 <br>&emsp; + Create snapshot <br>&emsp; + Build AMI optional <br>&emsp; + Access when keypair is lost<br>&emsp; + Install phpMyAdmin | 30/09/2025 | 30/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create AWS Free Tier account <br> - Learn about AWS Console & AWS CLI <br> - **Practice:** <br>&emsp; + Create AWS account <br>&emsp; + Install & configure AWS CLI <br> &emsp; + How to use AWS CLI       | 13/08/2025 | 13/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic EC2: <br>&emsp; + Instance types <br>&emsp; + AMI <br>&emsp; + EBS <br>&emsp; + ... <br> - SSH connection methods to EC2 <br> - Learn about Elastic IP <br>                                    | 14/08/2025 | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Practice:** <br>&emsp; + Launch EC2 instance <br>&emsp; + Connect via SSH <br>&emsp; + Attach EBS volume                                                                                                 | 15/08/2025 | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Week 4 Achievements:

- Understood the types of EC2 instance types to choose suitable for workload, helping optimize development costs and performance

  - Use T-series instances for light workloads, low CPU demand.
  - Use M-series instances for balanced workloads between CPU, memory, and network.
  - Use R-series instances for workloads requiring lots of memory.

- Configure inbound rules in SG:

  - SSH, port 22 for connection via PuTTY or SSH client
  - All ICMP-IPv4 to allow ping and ICMP error messages
  - All ICMP-IPv6 to allow ping and ICMP error messages over IPv6
  - HTTP, port 80 for unsecured web access
  - HTTPS, port 443 for secured web access
  - MySQL/Aurora, port 3306 for MySQL Database
  - Custom TCP, port 5000 to run Node.js application

- Successfully created and configured VPC Linux and VPC Windows.
- Created and received Windows keypair to secure EC2 instance connection via RDP (Remote Desktop Protocol) on port 3389

- Connect to Amazon Linux 2 using MobaXterm:
- Change EC2 Instance Type configuration: from t2.micro to t3.micro
- Successfully performed EC2 snapshot. Backed up data before changing configuration. -> including data backup + EBS volume state
- Installed LAMP stack (Linux, Apache, MySQL, PHP) on Amazon Linux 2
  - Configured and used phpMyAdmin to manage database
  - Installed Node.js Runtime Environment
  - Deployed and ran AWS User Management application

- Link do-result lab: https://drive.google.com/drive/folders/1VMVLJbZfOHvjrRfTbkcXPtq55aznfbDZ?usp=sharing
