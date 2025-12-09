---
title: "Worklog Tuần 10"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10:

* Setup AWS Account, IAM Users & Policies
* Nghiên cứu Bedrock Claude 3.5 & Cohere Embed APIs
* Nghiên cứu Textract AnalyzeDocument API
* Thiết kế architecture

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Setup AWS Account <br> - Tạo IAM Users & Policies <br> - Cấu hình quyền truy cập cho các dịch vụ AI/ML                                                                                    | 10/11/2025   | 10/11/2025      | <https://docs.aws.amazon.com/IAM/>        |
| 3   | - Nghiên cứu Amazon Bedrock <br> - Tìm hiểu Claude 3.5 model <br> - Tìm hiểu Cohere Embed APIs                                                                                              | 11/11/2025   | 11/11/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 4   | - Viết sample code `test_bedrock.py` <br> - Test Claude 3.5 API calls <br> - Test Cohere Embedding                                                                                          | 12/11/2025   | 12/11/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 5   | - Nghiên cứu Amazon Textract <br> - Tìm hiểu AnalyzeDocument API <br> - Viết sample code `test_textract.py`                                                                                 | 13/11/2025   | 13/11/2025      | <https://docs.aws.amazon.com/textract/>   |
| 6   | - Thiết kế Architecture diagram <br> - Ước tính chi phí (~$65/month) <br> - Review và hoàn thiện tài liệu                                                                                   | 14/11/2025   | 14/11/2025      |                                           |


### Kết quả đạt được tuần 10:

* **AWS Account Setup:**
  * Tạo AWS Account 427995028618 với IAM users
  * Cấu hình IAM Policies cho Bedrock, Textract

* **Sample Code:**
  * `test_bedrock.py` — test Claude 3.5 & Cohere Embed APIs
  * `test_textract.py` — test AnalyzeDocument API

* **Architecture:**
  * Hoàn thành Architecture diagram cho ARC-Chatbot
  * Xác định các dịch vụ cần sử dụng: Bedrock, Textract, S3, Lambda, ...

* **Cost Estimation:**
  * Ước tính chi phí khoảng ~$65/month cho môi trường dev/test


