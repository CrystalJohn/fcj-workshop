---
title: "Worklog Tuần 6"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---


### Mục tiêu tuần 6:

* Nắm được kiến thức liên quan đến Route 53 nằm trong pillar Reliability của AWS Well-Architected Framework.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Làm quen với Route 53 <br>&emsp; + Định nghĩa và chức năng                                                                                                                                | 13/10/2025   | 13/10/2025      |
| 3   | - **Thực hành** <br>&emsp; - Thiết lập Hybrid DNS với Route 53 Resolver                                                                                                                     | 14/10/2025   | 14/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Học về các khái niệm: <br> + Nguyên tắc Least Privilege (chỉ cấp quyền cần thiết) <br> + Cách hoạt động của KMS (Key Management Service) <br>                                             | 15/10/2025   | 15/10/2025      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu về Amazon DynamoDB và thực hành bài lab về DynamoDB <br> - Học về tạo Global secondary index                                                                                                                                                              | 16/10/2025   | 16/10/2025      | <https://000060.awsstudygroup.com/vi/1-introduce/> |
| 6   | - **Thực hành:** <br>&emsp; + Phát triển với Python và DynamoDB                                                                                         | 17/10/2025   | 17/10/2025      | <https://000060.awsstudygroup.com/vi/3-gettingstartedwithawssdk/3.2-pythonandynamodb/> |


### Kết quả đạt được tuần 6:
* Hiểu được cách  thiết kế hệ thống DNS với Route 53 Resolver 
  * Sử dụng AWS Quick Start để triển khai Hybrid DNS
  * Sử dụng  AWS Managed Microsoft Active Directory
Kết quả thực hành: https://drive.google.com/drive/folders/1-ZtnhA9PyAjANC9loPAZApB7TQHBCAox?usp=sharing 

* Biết được việc tạo một Global Secondary Index (GSI) trong DynamoDB có tác dụng chính là cho phép truy vấn dữ liệu nhanh chóng
dựa trên các thuộc tính chứ không phải là khóa chính (PK). Thay vì sử dụng PK bằng Partition key và Sort Key để truy vấn 
  * Partition Key: Thuộc tính dùng để phân phối dữ liệu trên các phân vùng lưu trữ vật lý.
  * Sort Key: Thuộc tính dùng để sắp xếp và lọc dữ liệu trong cùng một phân vùng.