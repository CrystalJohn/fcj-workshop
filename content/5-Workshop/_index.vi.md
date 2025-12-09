---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Tổng quan

Trong workshop này, chúng ta sẽ xây dựng **ARC (Academic Research Chatbot)** - một hệ thống chatbot thông minh hoạt động trên nền tảng **AWS Serverless**. Giải pháp này ứng dụng **Generative AI** và **RAG (Retrieval-Augmented Generation)** để hỗ trợ nghiên cứu học thuật, truy vấn tài liệu và trả lời câu hỏi một cách linh hoạt.

Thay vì trả lời các câu hỏi dựa trên kịch bản cố định (rule-based), hệ thống sử dụng mô hình **Claude 3.5 Sonnet** để hiểu ngôn ngữ tự nhiên, truy vấn dữ liệu từ cơ sở vector database và phản hồi người dùng một cách chính xác.

#### Mục tiêu Workshop

Sau khi hoàn thành workshop, bạn sẽ:

- Hiểu kiến trúc RAG và cách áp dụng vào thực tế  
- Triển khai hệ thống chatbot hoàn chỉnh trên AWS  
- Sử dụng Amazon Bedrock (Claude 3.5 Sonnet + Cohere Embed)  
- Xây dựng IDP pipeline với Amazon Textract  
- Implement vector search với Qdrant  
- Deploy infrastructure với Terraform  
- Tích hợp authentication với Amazon Cognito  

#### Nội dung

1. [Giới thiệu về workshop](5.1-workshop-overview)
2. [Các bước chuẩn bị](5.2-prerequiste)
3. [Tạo S3 VPC Endpoint](5.3-s3-vpc)
4. [Tạo S3 On-Prem Access](5.4-s3-onprem)
5. [Cấu hình Policy](5.5-policy)
6. [Dọn dẹp tài nguyên](5.6-cleanup)
