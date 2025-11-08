---
title: "Worklog Tuần 9"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9:

* Hoàn thành tài khoản mới AWS để chuẩn bị làm project proposal.
* Tìm hiểu kỹ về S3 và các khái niệm liên quan.
* Thiết kế kiến trúc cho project nộp proposal.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Thực hành** Chiến lược nhận đủ $200 credit (bài lab mentor Thịnh Nguyễn) <br>&emsp; + Tạo EC2 <br>&emsp; + Amazon Bedrock Playground <br>&emsp; + Set up AWS Budgets <br>&emsp; + Tạo Lambda Web App <br>&emsp; + Tạo RDS Database                     | 03/11/2025   | 03/11/2025      | <https://thinhnguyen1211.github.io/fcj-first-lab/> |
| 3   | - Tìm hiểu về Amazon S3 - Access Point - Storage Class <br> - Tìm hiểu về AWS Security Hub                                                                                                  | 04/11/2025   | 04/11/2025      | <https://www.youtube.com/watch?v=_yunukwcAwc&t=2s> <https://www.youtube.com/watch?v=YnLo4MgOXyA> |
| 4   | - **Thực hành** lab Serverless <br>&emsp; - Hướng dẫn viết Front-end gọi API Gateway  <br> - Làm việc với Amazon DynamoDB                                                                                                                     | 05/11/2025   | 05/11/2025      | <https://000079.awsstudygroup.com/vi/> |
| 5   | - Tìm hiểu Amazon DynamoDB  - Thiết kế kiến trúc cho project nộp proposal               | 06/11/2025   | 06/11/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Thiết kế kiến trúc cho project nộp proposal                                                                                        | 07/11/2025   | 07/11/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 9:

* Account AWS với credit đã bị expire dẫn đến việc tạo account mới để tiếp tục thực hành.
  * Kết quả thực hành: đã nhận được $200 credit.
  * Tạo Budgets Cost để theo dõi chi phí sử dụng dịch vụ AWS.
  * Daily Cost Monitoring
  * Nắm được các services “nguy hiểm” cần tránh

* Hiểu được S3 Access Point và Storage Class.
  * S3 Access Point giúp quản lý quyền truy cập dữ liệu trong S3 bucket dễ dàng hơn, đặc biệt trong môi trường có nhiều người dùng hoặc ứng dụng.
  * Storage Class trong S3 chia vùng lưu trữ ra nhiều lớp lưu trữ khác nhau để tối ưu hóa chi phí và hiệu suất dựa trên tần suất truy cập dữ liệu.
    * S3 Standard: Dữ liệu truy cập thường xuyên.
    * S3 Standard-IA: Dữ liệu truy cập không thường xuyên. Nếu đưa dữ liệu truy cập thường xuyên'
    vào lớp này sẽ bị tính phí cao hơn cả S3 Standard.
    * S3 Intelligent-Tiering: Tự động chuyển đổi đối tượng giữa cấp lưu trữ theo số ngày đối với object không được truy cập.
    * S3 One Zone-IA: Dữ liệu truy cập không thường xuyên, chỉ lưu trữ trong một vùng duy nhất, không replicate.
    * Amazon Glacier /Deep Archive: Lưu trữ dữ liệu dài hạn. Không thể GET dữ liệu trực tiếp ngay trong class này mà phải chuyển về 1 trong 4 class trên.

* Hiểu được AWS Backup thiết lập chính sách bảo vệ dữ liệu tự động cho các dịch vụ AWS và on-premises.

* Hiểu được API RESTful và API WebSocket
  * API RESTful sẽ tạo ra 1 kết nối mới mỗi khi có yêu cầu từ client đến server. Sau khi server phản hồi xong, kết nối sẽ đóng lại.
  * API WebSocket sẽ duy trì kết nối mở giữa client và server, cho phép giao tiếp hai chiều liên tục mà không cần thiết lập lại kết nối cho mỗi yêu cầu.
    -> phù hợp cho các ứng dụng real-time như chat, game,...

Vấn đề gặp phải:
  * Khi làm lab chưa hiểu về DynamoDB nên chưa thể làm và tìm hiểu về các khái niệm cơ bản của DynamoDB.    