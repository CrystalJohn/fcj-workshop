---
title: "Worklog Tuần 11"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Hoàn thiện setup base infrastructure (M0) — S3, DynamoDB, Cognito, ALB
* Bắt đầu triển khai IDP Pipeline (M1) — SQS, PDF Detection, PyPDF2 extraction

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Setup S3 bucket `arc-chatbot-documents-427995028618` <br> - Tạo DynamoDB tables (`metadata`, `chat-history`) <br> - Cấu hình table schema và indexes                                     | 17/11/2025   | 17/11/2025      | <https://docs.aws.amazon.com/dynamodb/>   |
| 3   | - Configure Cognito User Pool `ap-southeast-1_8KB4JYvsX` <br> - Setup user authentication flow <br> - Test sign-up/sign-in process                                                         | 18/11/2025   | 18/11/2025      | <https://docs.aws.amazon.com/cognito/>    |
| 4   | - Setup ALB `arc-chatbot-dev-alb` với health checks <br> - Cấu hình target groups và listeners <br> - Test load balancer routing                                                           | 19/11/2025   | 19/11/2025      | <https://docs.aws.amazon.com/elasticloadbalancing/> |
| 5   | - Tạo SQS queue `arc-chatbot-dev-document-processing` <br> - Implement PDF detection service (digital vs scanned) <br> - Viết unit tests cho PDF detector                                  | 20/11/2025   | 20/11/2025      | <https://docs.aws.amazon.com/sqs/>        |
| 6   | - Implement PyPDF2 extraction cho digital PDFs <br> - Xử lý các edge cases (encrypted, corrupted PDFs) <br> - Viết tests và đạt 30 tests passed                                            | 21/11/2025   | 21/11/2025      | <https://pypdf2.readthedocs.io/>          |


### Kết quả đạt được tuần 11:

* **Hoàn thiện Base Infrastructure (M0):**
  * S3 bucket `arc-chatbot-documents-427995028618` — lưu trữ documents
  * DynamoDB tables: `metadata` (document info), `chat-history` (conversation logs)
  * Cognito User Pool `ap-southeast-1_8KB4JYvsX` — xác thực người dùng
  * ALB `arc-chatbot-dev-alb` với health checks — load balancing cho API

* **Bắt đầu IDP Pipeline (M1):**
  * SQS Queue `arc-chatbot-dev-document-processing` — queue xử lý documents
  * PDF Detector service — phân biệt digital vs scanned PDFs (17 tests passed)
  * PDF Extractor service với PyPDF2 — extract text từ digital PDFs (30 tests passed)

* **Học được:**
  * Cách thiết kế DynamoDB schema cho chatbot application
  * Cognito authentication flow và JWT tokens
  * SQS message processing patterns
  * Xử lý các loại PDF khác nhau trong Python


