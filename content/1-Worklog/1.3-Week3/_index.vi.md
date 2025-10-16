---
title: "Worklog Tuần 3"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3:

- Triển khai và cấu hình EC2 instance trong VPC Public/Private Subnet
- Thực hành các bài lab về EC2 cơ bản như Site-to-Site VPN, CloudWatch Monitoring & Alerting
- Triển khai ứng dụng Node.js trên Amazon Linux

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - **Thực hành** <br>&emsp; + Triển khai Hạ tầng EC2 Production-Ready + <br>&emsp; + Cách đưa file aws-keypair.pem lên EC2    | 22/09/2025   | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Thực hành** <br>&emsp; + Triển khai Hạ tầng EC2 Production-Ready + <br>&emsp; + Cách kết nối với EC2 Private qua bostion | 23/09/2025   | 23/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Thiết lập AWS Systems Manager Session Manager và CloudWatch Monitoring & Alerting cho tài nguyên VPC                       | 24/09/2025   | 24/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành** <br>&emsp; + Cấu hình Site to Site VPN                                                                       | 25/09/2025   | 25/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - cấu hình Virtual Private Gateway (VGW) và Customer Gateway (CGW).                                                          | 26/09/2025   | 26/09/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 3:

- Triển khai được EC2 instance
- Thực hành các bài lab cơ bản về EC2
- Kết nối được với EC2 instance qua SSH
- Tạo Elastic IP Address và gán vào EC2 instance
- Sử dụng NAT Gateway để truy cập Internet từ EC2 instance trong Private Subnet
- Học được cách cấu hình Site-to-site VPN giữa VPC Cloud (AWS Cloud) và VPC ASG VPN (mô phỏng On-premises) thông qua cùng 10.10.0.0/16

_Bài học & Khó khăn_:
Học được cách phân biệt Public Subnet và Private Subnet.<br>
Route propagation tự động cập nhật bảng định tuyến khi có thay đổi.<br>

Khó khăn: Lúc đầu SSH qua VSCode không được, cần chỉnh lại cấu hình Security Group.
