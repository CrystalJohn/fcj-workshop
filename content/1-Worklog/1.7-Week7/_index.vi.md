---
title: "Worklog Tuần 7"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7:

* Làm quen với SageMaker Studio và thực hành workshop

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Học về Qdrant EC2                                                                                             | 20/10/2025   | 20/10/2025      |  <https://aws.amazon.com/marketplace/pp/prodview-rtphb42tydtzg>
| 3   | - Học về cách setup với Amazon Bedrock APIs                                                                                                                                                 | 21/10/2025   | 21/10/2025      | <https://github.com/aws-samples/amazon-bedrock-samples/tree/main/introduction-to-bedrock/bedrock_apis> |
| 4   | - Tạo domain SageMaker để test Amazon Bedrock trong SageMaker <br> - Setup Bedrock và prompt với model Claude 3.5 Sonet <br> - Cách làm việc với Amazon Bedrock APIs                        | 22/10/2025   | 22/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành:** <br>&emsp; Cài đặt Bedrock <br>&emsp; Claude 3.5 Sonnet <br>&emsp; Gọi API cơ bản <br>&emsp; Xử lý hàng loạt <br>&emsp; Kỹ thuật viết prompt <br>&emsp; Xử lý giới hạn tốc độ | 23/10/2025   | 23/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Học các khái niệm:** <br>&emsp; Amazon Simple Queue Service (Amazon SQS) <br>&emsp; Amazon Simple Notification Service (SNS) <br>&emsp; AWS Organizations <br>&emsp; Amazon Macie <br>&emsp; AWS Direct Connect | 24/10/2025   | 24/10/2025      | <https://skillbuilder.aws/learn/94T2BEN85A/aws-cloud-practitioner-essentials/> |


### Kết quả đạt được tuần 7:

- Biết được Qdrant trên EC2 chính là vector database. Với EC2 tức là bạn tự host Qdrant trên EC2.
- Qdrant trên EC2 là giải pháp vector DB tự quản lý, dùng để lưu và tìm kiếm embedding (vector) trong hệ thống RAG.
Nó thay thế các dịch vụ đắt tiền như OpenSearch Serverless (~$90–150/tháng).

- Qdrant được dùng để 
  - Lưu embedding từ Titan Text Embeddings v2, sau khi bạn chunk tài liệu PDF/OCR
  - Tìm top-K vectors cho mỗi truy vấn → feed cho Claude Sonnet để RAG

- Thực hành tạo domain SageMaker và kiểm thử kết nối tới Amazon Bedrock
  - Đã tạo được domain đơn giản trên SageMaker để thử nghiệm gọi Bedrock APIs từ môi trường notebook.
  - Xác nhận flow: gửi prompt → nhận response từ Claude 3.5 Sonnet.

- Triển khai và gọi thử Amazon Bedrock APIs
  - Viết script mẫu để gọi API (sync/async), kiểm tra giới hạn tần suất và xử lý lỗi (retry/backoff).
  - Thử nghiệm batch processing: gửi nhiều prompt/embedding jobs và quan sát quota/rate limits.

- Kiến thức bảo mật & hạ tầng
  - Tìm hiểu nhanh AWS Organizations và IAM role để quản lý tài khoản và quyền truy cập dịch vụ Bedrock/Textract.
  - Làm quen với Amazon Macie (data classification) và ý nghĩa của Direct Connect trong kịch bản kết nối on-prem.

- Bài học thực tế & bước tiếp theo
  - Nắm rõ flow end-to-end: Upload PDF → SQS message → EC2 Worker (Textract → chunk → Titan Embeddings) → index vào Qdrant → RAG với Claude Sonnet.
  - Kế hoạch tiếp theo: triển khai worker mẫu trên EC2 để tự động hoá ingest, test performance và tối ưu batch size/throughput.




