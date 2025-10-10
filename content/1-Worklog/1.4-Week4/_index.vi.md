---
title: "Worklog Tuần 4"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

- Triển khai ứng dụng trên Linux Instance

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc                                                                                                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Kiến thức cơ bản về máy chủ ảo với Amazon Elastic Compute Cloud (EC2) + Clean resource                                                                                                                     | 29/09/2025   | 29/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Lên văn phòng <br>&emsp; - **Thực hành:** <br>&emsp; + Amazon EC2 cơ bản <br>&emsp; + Tạo snapshot <br>&emsp; + Xây dựng AMI optional <br>&emsp; + Truy cập khi mất keypair<br>&emsp; + Cài đặt phpMyAdmin | 30/09/2025   | 30/09/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu về AWS Cloud9 <br> - Chuẩn bị với Amazon S3                                                                                                                                                       | 01/10/2025   | 01/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành** <br>&emsp; + Tạo S3 và thiết lập hosting static web                                                                                                                                          | 02/10/2025   | 02/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành:** <br>&emsp; + Tạo EC2 instance <br>&emsp; + Kết nối SSH <br>&emsp; + Gắn EBS volume                                                                                                          | 15/08/2025   | 15/08/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 4:

- Hiểu được các loại EC2 instance types để lựa chọn phù hợp với workload, giúp tôi tối ưu chi phí phát triển và hiệu suất

  - Sử dụng instance dòng T cho workload nhẹ, như cầu CPU thấp.
  - Sử dụng instance dòng M cho workload cân bằng giữa CPU, bộ nhớ và mạng.
  - Sử dụng instance dòng R cho workload cần nhiều bộ nhớ.

- Cấu hình inbound rules trong SG:

  - SSH, cổng 22 để kết nối qua PuTTY hoặc SSH client
  - All ICMP-IPv4 cho phép ping và các thông báo lỗi ICMP
  - All ICMP-IPv6 cho phép ping và các thông báo lỗi qua IPv6
  - HTTP, cổng 80 cho truy cập web không bảo mật
  - HTTPS, cổng 443 cho truy cập web bảo mật
  - MySQL/Aurora, cổng 3306 sử dụng cho Database MySQL
  - Custom TCP, cổng 5000 để chạy ứng dụng Node.js

- Đã tạo và cấu hình VPC Linux và VPC Windows thành công.
- Tạo và nhận keypair windows để bảo mật kết nối EC2 instance thông qua RDP (Remote Desktop Protocol) qua port 3389

1gqFSE53bGQJJKeE@)u;Hsi7xOphbTqv
mật khẩu root để sử dụng cấu hình Database cho Nodejs app: 123Admin

- Kết nối Amazon Linux 2 bằng MobaXterm:
- Thay đổi cấu hình EC2 Instance Type: từ t2.micro sang t3.micro
- Thực hiện EC2 snapshot thành công. Đã snapshot(backup) lại dữ liệu trước khi thay đổi cấu hình. -> bao gồm backup dữ liệu + trạng thái EBS volume
- Cài đặt LAMP stack (Linux, Apache, MySQL, PHP) trên Amazon Linux 2
  - Cấu hình và sử dụng phpMyAdmin để quản lý cơ sở dữ liệu
  - Cài đặt Node.js Runtime Environment
  - Triển khai và chạy ứng dụng AWS User Management
