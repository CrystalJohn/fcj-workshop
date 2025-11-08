---
title: "Bản đề xuất"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Academic Research Chatbot
## Giải pháp AWS RAG-based hỗ trợ học thuật và nghiên cứu học tập thông minh  

### 1. Tóm tắt điều hành  
Academic Research Chatbot được thiết kế dành cho sinh viên, giảng viên, và nhóm nghiên cứu học thuật tại các trường đại học như FPT University, với mục tiêu hỗ trợ việc học, ôn tập và nghiên cứu khoa học bằng hội thoại thông minh. Nền tảng cho phép người dùng tải lên tài liệu học thuật (PDF, DOCX, giáo trình, bài báo nghiên cứu, bài giảng, v.v.) và sử dụng trí tuệ nhân tạo (AI) để tìm kiếm, tóm tắt, so sánh, và trích dẫn nội dung theo ngữ cảnh học thuật.

Hệ thống ứng dụng mô hình Retrieval-Augmented Generation (RAG) kết hợp giữa Amazon Bedrock, Amazon Kendra, và Amazon Textract để phân tích và hiểu tài liệu. Giao diện người dùng được xây dựng bằng React (Next.js), lưu trữ trên Amazon S3 và phân phối qua Amazon CloudFront, đảm bảo trải nghiệm mượt mà, nhanh chóng, và bảo mật với Amazon Cognito.

Nền tảng hoạt động hoàn toàn theo kiến trúc Serverless trên AWS, dễ mở rộng, chi phí thấp, phù hợp với quy mô sử dụng nội bộ tại các lab học thuật. Trong tương lai, hệ thống có thể mở rộng thành trợ lý học tập thông minh đa ngôn ngữ, hỗ trợ tiếng Anh học thuật (Academic English) hoặc các môn chuyên ngành như Data Structures & Algorithms, Software Engineering, hoặc Database Systems.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Các nền tảng hỗ trợ học tập hiện nay (như ChatGPT, Perplexity, hoặc NotebookLM) tuy mang lại trải nghiệm học tập hiện đại và dễ sử dụng, nhưng đa phần hoạt động trong môi trường đóng, không thể tùy chỉnh theo chương trình học hoặc tích hợp sâu với hệ thống nội bộ của trường.
Điều này khiến các tổ chức giáo dục khó quản lý dữ liệu, kiểm soát nội dung học thuật, hoặc mở rộng tính năng theo nhu cầu riêng.

*Giải pháp*  
Academic Research Chatbot sử dụng công nghệ AWS Serverless kết hợp Generative AI để tạo ra một hệ thống hội thoại học thuật thông minh.
Người dùng chỉ cần tải lên tài liệu học thuật, hệ thống sẽ:

1. Tự động trích xuất nội dung (Amazon Textract).

2. Lưu trữ và lập chỉ mục ngữ nghĩa (Amazon Kendra / OpenSearch).

3. Tìm kiếm đoạn nội dung liên quan (Retrieval) và sinh phản hồi học thuật (Generation) bằng mô hình Claude 3.5 Sonnet trên Amazon Bedrock.

Giải pháp cho phép sinh viên đặt câu hỏi trực tiếp liên quan đến tài liệu học, nhận được câu trả lời có trích dẫn, tóm tắt hoặc ví dụ minh họa. Giảng viên có thể theo dõi dashboard phân tích (Amazon QuickSight) để nắm bắt các chủ đề được hỏi nhiều nhất, giúp cải thiện nội dung giảng dạy.

*Lợi ích và hoàn vốn đầu tư (ROI)*  
Giải pháp giúp sinh viên tiết kiệm 40–60% thời gian ôn tập và nghiên cứu, đồng thời cung cấp công cụ tự học hiện đại cho nhà trường mà chi phí vận hành dưới 1 USD/tháng nhờ kiến trúc serverless.  
Ngoài ra, nền tảng tạo tiền đề cho việc mở rộng sang các lĩnh vực nghiên cứu khác như phân tích ngôn ngữ học, tổng hợp báo cáo khoa học, hoặc xây dựng học liệu số (digital learning repository).  
Thời gian hoàn vốn dự kiến 3–6 tháng thông qua giảm chi phí nhân lực hướng dẫn và tăng hiệu quả tự học của sinh viên.  

### 3. Kiến trúc giải pháp

Academic Research Chatbot áp dụng mô hình AWS RAG-based Serverless Architecture, cho phép hệ thống mở rộng linh hoạt, đảm bảo hiệu suất cao và chi phí thấp.

Luồng xử lý dữ liệu và hội thoại
<br>[Sơ đồ kiến trúc] <br>


 *Dịch vụ AWS sử dụng*
- Amazon Textract: Tách text từ file PDF/DOCX.
- Amazon Kendra / OpenSearch: Tạo chỉ mục ngữ nghĩa để tìm đoạn liên quan.
- Amazon Bedrock (Claude 3.5 Sonnet): Sinh phản hồi học thuật theo ngữ cảnh.
- AWS Lambda: Xử lý backend, orchestrate RAG pipeline.
- Amazon S3: Lưu trữ file người dùng và dữ liệu trích xuất.
- Amazon API Gateway: Cổng kết nối giữa UI và backend.
- Amazon Cognito: Xác thực người dùng theo vai trò.
- Amazon QuickSight: Phân tích dữ liệu và tạo dashboard cho giảng viên.
- Amazon CloudWatch: Theo dõi hiệu suất, log và cảnh báo lỗi.

*Thiết kế thành phần*

- Người dùng: Sinh viên, giảng viên, và nhà nghiên cứu.
- Xử lý tài liệu: PDF/DOCX được upload lên S3 và xử lý bởi Textract.
- Lập chỉ mục: Kendra/OpenSearch phân tích và tạo embedding vector.
- Hội thoại AI: Bedrock (Claude 3.5 Sonnet) sử dụng RAG để sinh câu trả lời.
- Quản lý người dùng: Cognito xác thực và phân quyền truy cập.
- Phân tích dữ liệu: QuickSight tổng hợp hành vi học tập và tài liệu phổ biến.  

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Dự án gồm 2 phần — thiết lập trạm thời tiết biên và xây dựng nền tảng thời tiết — mỗi phần trải qua 4 giai đoạn:  
1. *Nghiên cứu và vẽ kiến trúc*: Nghiên cứu Raspberry Pi với cảm biến ESP32 và thiết kế kiến trúc AWS Serverless (1 tháng trước kỳ thực tập).  
2. *Tính toán chi phí và kiểm tra tính khả thi*: Sử dụng AWS Pricing Calculator để ước tính và điều chỉnh (Tháng 1).  
3. *Điều chỉnh kiến trúc để tối ưu chi phí/giải pháp*: Tinh chỉnh (ví dụ tối ưu Lambda với Next.js) để đảm bảo hiệu quả (Tháng 2).  
4. *Phát triển, kiểm thử, triển khai*: Lập trình Raspberry Pi, AWS services với CDK/SDK và ứng dụng Next.js, sau đó kiểm thử và đưa vào vận hành (Tháng 2–3).  

*Yêu cầu kỹ thuật*  
- *Trạm thời tiết biên*: Cảm biến (nhiệt độ, độ ẩm, lượng mưa, tốc độ gió), vi điều khiển ESP32, Raspberry Pi làm thiết bị biên. Raspberry Pi chạy Raspbian, sử dụng Docker để lọc dữ liệu và gửi 1 MB/ngày/trạm qua MQTT qua Wi-Fi.  
- *Nền tảng thời tiết*: Kiến thức thực tế về AWS Amplify (lưu trữ Next.js), Lambda (giảm thiểu do Next.js xử lý), AWS Glue (ETL), S3 (2 bucket), IoT Core (gateway và rules), và Cognito (5 người dùng). Sử dụng AWS CDK/SDK để lập trình (ví dụ IoT Core rules tới S3). Next.js giúp giảm tải Lambda cho ứng dụng web fullstack.  

### 5. Lộ trình & Mốc triển khai  
- *Trước thực tập (Tháng 0)*: 1 tháng lên kế hoạch và đánh giá trạm cũ.  
- *Thực tập (Tháng 1–3)*:  
    - Tháng 1: Học AWS và nâng cấp phần cứng.  
    - Tháng 2: Thiết kế và điều chỉnh kiến trúc.  
    - Tháng 3: Triển khai, kiểm thử, đưa vào sử dụng.  
- *Sau triển khai*: Nghiên cứu thêm trong vòng 1 năm.  

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)  
Hoặc tải [tệp ước tính ngân sách](../attachments/budget_estimation.pdf).  

*Chi phí hạ tầng*  
- AWS Lambda: 0,00 USD/tháng (1.000 request, 512 MB lưu trữ).  
- S3 Standard: 0,15 USD/tháng (6 GB, 2.100 request, 1 GB quét).  
- Truyền dữ liệu: 0,02 USD/tháng (1 GB vào, 1 GB ra).  
- AWS Amplify: 0,35 USD/tháng (256 MB, request 500 ms).  
- Amazon API Gateway: 0,01 USD/tháng (2.000 request).  
- AWS Glue ETL Jobs: 0,02 USD/tháng (2 DPU).  
- AWS Glue Crawlers: 0,07 USD/tháng (1 crawler).  
- MQTT (IoT Core): 0,08 USD/tháng (5 thiết bị, 45.000 tin nhắn).  

*Tổng*: 0,7 USD/tháng, 8,40 USD/12 tháng  
- *Phần cứng*: 265 USD một lần (Raspberry Pi 5 và cảm biến).  

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
- Mất mạng: Ảnh hưởng trung bình, xác suất trung bình.  
- Hỏng cảm biến: Ảnh hưởng cao, xác suất thấp.  
- Vượt ngân sách: Ảnh hưởng trung bình, xác suất thấp.  

*Chiến lược giảm thiểu*  
- Mạng: Lưu trữ cục bộ trên Raspberry Pi với Docker.  
- Cảm biến: Kiểm tra định kỳ, dự phòng linh kiện.  
- Chi phí: Cảnh báo ngân sách AWS, tối ưu dịch vụ.  

*Kế hoạch dự phòng*  
- Quay lại thu thập thủ công nếu AWS gặp sự cố.  
- Sử dụng CloudFormation để khôi phục cấu hình liên quan đến chi phí.  

### 8. Kết quả kỳ vọng  
*Cải tiến kỹ thuật*: Dữ liệu và phân tích thời gian thực thay thế quy trình thủ công. Có thể mở rộng tới 10–15 trạm.  
*Giá trị dài hạn*: Nền tảng dữ liệu 1 năm cho nghiên cứu AI, có thể tái sử dụng cho các dự án tương lai.