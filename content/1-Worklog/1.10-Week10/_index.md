---
title: "Week 10 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Week 10 Objectives:

* Setup AWS Account, IAM Users & Policies
* Research Bedrock Claude 3.5 & Cohere Embed APIs
* Research Textract AnalyzeDocument API
* Design architecture

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Setup AWS Account <br> - Create IAM Users & Policies <br> - Configure access permissions for AI/ML services                                                                                         | 11/10/2025 | 11/10/2025      | <https://docs.aws.amazon.com/IAM/>        |
| 3   | - Research Amazon Bedrock <br> - Learn about Claude 3.5 model <br> - Learn about Cohere Embed APIs                                                                                                    | 11/11/2025 | 11/11/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 4   | - Write sample code `test_bedrock.py` <br> - Test Claude 3.5 API calls <br> - Test Cohere Embedding                                                                                                    | 11/12/2025 | 11/12/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 5   | - Research Amazon Textract <br> - Learn about AnalyzeDocument API <br> - Write sample code `test_textract.py`                                                                                         | 11/13/2025 | 11/13/2025      | <https://docs.aws.amazon.com/textract/>   |
| 6   | - Design Architecture diagram <br> - Estimate costs (~$65/month) <br> - Review and finalize documentation                                                                                             | 11/14/2025 | 11/14/2025      |                                           |


### Week 10 Achievements:

* **AWS Account Setup:**
  * Created AWS Account 427995028618 with IAM users
  * Configured IAM Policies for Bedrock, Textract

* **Sample Code:**
  * `test_bedrock.py` — test Claude 3.5 & Cohere Embed APIs
  * `test_textract.py` — test AnalyzeDocument API

* **Architecture:**
  * Completed Architecture diagram for ARC-Chatbot
  * Identified required services: Bedrock, Textract, S3, Lambda, ...

* **Cost Estimation:**
  * Estimated costs approximately ~$65/month for dev/test environment
