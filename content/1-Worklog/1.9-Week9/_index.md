---
title: "Week 9 Worklog"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Week 9 Objectives:

* Complete a new AWS account to prepare for the project proposal.
* Deeply understand S3 and related concepts.
* Design architecture for the proposal project.

### Tasks to be completed this week:
| Day | Task                                                                                                                                                                                                 | Start Date  | End Date    | Reference Material                         |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- | ----------- | ------------------------------------------ |
| 2   | - **Practice:** Strategy to receive $200 credit (Lab by mentor Thinh Nguyen) <br>&emsp; + Create EC2 <br>&emsp; + Amazon Bedrock Playground <br>&emsp; + Set up AWS Budgets <br>&emsp; + Create Lambda Web App <br>&emsp; + Create RDS Database | 03/11/2025  | 03/11/2025  | <https://thinhnguyen1211.github.io/fcj-first-lab/> |
| 3   | - Learn about Amazon S3 - Access Point - Storage Class <br> - Learn about AWS Security Hub                                                                   | 04/11/2025  | 04/11/2025  | <https://www.youtube.com/watch?v=_yunukwcAwc&t=2s> <https://www.youtube.com/watch?v=YnLo4MgOXyA> |
| 4   | - **Practice:** Serverless lab <br>&emsp; - Guide to writing Front-end calling API Gateway <br> - Work with Amazon DynamoDB                                  | 05/11/2025  | 05/11/2025  | <https://000079.awsstudygroup.com/vi/> |
| 5   | - Learn about Amazon DynamoDB <br> - Design architecture for proposal project                                           | 06/11/2025  | 06/11/2025  | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Design architecture for proposal project                                                                             | 07/11/2025  | 07/11/2025  | <https://cloudjourney.awsstudygroup.com/> |


### Week 9 Achievements:

* AWS account credit expired, so a new account was created to continue practicing.
  * Practice result: successfully received $200 credit.
  * Created Budgets Cost to monitor AWS service usage costs.
  * Daily Cost Monitoring
  * Identified "dangerous" services to avoid

* Understood S3 Access Point and Storage Class.
  * S3 Access Point helps manage data access permissions in S3 buckets more easily, especially in environments with many users or applications.
  * Storage Class in S3 divides storage into different classes to optimize cost and performance based on data access frequency.
    * S3 Standard: Frequently accessed data.
    * S3 Standard-IA: Infrequently accessed data. Storing frequently accessed data in this class will incur higher costs than S3 Standard.
    * S3 Intelligent-Tiering: Automatically moves objects between storage tiers based on days without access.
    * S3 One Zone-IA: Infrequently accessed data, stored in a single zone only, not replicated.
    * Amazon Glacier / Deep Archive: Long-term data storage. Data cannot be directly GET from this class; it must be restored to one of the four classes above.

* Understood AWS Backup, which sets up automatic data protection policies for AWS services and on-premises environments.

* Understood RESTful API and WebSocket API:
  * RESTful API creates a new connection for each client request to the server. After the server responds, the connection is closed.
  * WebSocket API maintains an open connection between client and server, allowing continuous two-way communication without re-establishing the connection for each request. -> Suitable for real-time applications like chat, games, etc.

Issues encountered:
  * During the lab, did not understand DynamoDB well enough to work with it and had to research basic concepts of DynamoDB.
