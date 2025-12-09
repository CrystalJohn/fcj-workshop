---
title: "Week 11 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Complete base infrastructure setup (M0) — S3, DynamoDB, Cognito, ALB
* Start implementing IDP Pipeline (M1) — SQS, PDF Detection, PyPDF2 extraction

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Setup S3 bucket `arc-chatbot-documents-427995028618` <br> - Create DynamoDB tables (`metadata`, `chat-history`) <br> - Configure table schema and indexes                                           | 11/17/2025 | 11/17/2025      | <https://docs.aws.amazon.com/dynamodb/>   |
| 3   | - Configure Cognito User Pool `ap-southeast-1_8KB4JYvsX` <br> - Setup user authentication flow <br> - Test sign-up/sign-in process                                                                    | 11/18/2025 | 11/18/2025      | <https://docs.aws.amazon.com/cognito/>    |
| 4   | - Setup ALB `arc-chatbot-dev-alb` with health checks <br> - Configure target groups and listeners <br> - Test load balancer routing                                                                   | 11/19/2025 | 11/19/2025      | <https://docs.aws.amazon.com/elasticloadbalancing/> |
| 5   | - Create SQS queue `arc-chatbot-dev-document-processing` <br> - Implement PDF detection service (digital vs scanned) <br> - Write unit tests for PDF detector                                         | 11/20/2025 | 11/20/2025      | <https://docs.aws.amazon.com/sqs/>        |
| 6   | - Implement PyPDF2 extraction for digital PDFs <br> - Handle edge cases (encrypted, corrupted PDFs) <br> - Write tests and achieve 30 tests passed                                                    | 11/21/2025 | 11/21/2025      | <https://pypdf2.readthedocs.io/>          |


### Week 11 Achievements:

* **Completed Base Infrastructure (M0):**
  * S3 bucket `arc-chatbot-documents-427995028618` — document storage
  * DynamoDB tables: `metadata` (document info), `chat-history` (conversation logs)
  * Cognito User Pool `ap-southeast-1_8KB4JYvsX` — user authentication
  * ALB `arc-chatbot-dev-alb` with health checks — load balancing for API

* **Started IDP Pipeline (M1):**
  * SQS Queue `arc-chatbot-dev-document-processing` — document processing queue
  * PDF Detector service — distinguish digital vs scanned PDFs (17 tests passed)
  * PDF Extractor service with PyPDF2 — extract text from digital PDFs (30 tests passed)

* **Learned:**
  * How to design DynamoDB schema for chatbot application
  * Cognito authentication flow and JWT tokens
  * SQS message processing patterns
  * Handling different types of PDFs in Python
