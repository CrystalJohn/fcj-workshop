---
title: "Event 2"
date: "29/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “AWS Cloud Mastery Series #3 workshop”

### Mục Đích Của Sự Kiện

- Chia sẻ và củng cố kiến thức cốt lõi trong AWS Well-Architected Framework – Security Pillar.
- Trang bị tư duy và kỹ năng xây dựng hệ thống an toàn, bền vững, tuân thủ chuẩn AWS.

### Danh Sách Diễn Giả

- **FCJ Team** 
- **Kha Van** - Cloud Security Engineer

### Nội Dung Nổi Bật

#### Security Foundation – Nền tảng bảo mật AWS

- **Least Privilege – Zero Trust – Defense in Depth**: 3 nguyên tắc nền tảng cho mọi hệ thống bảo mật modern.
- **Shared Responsibility Model**: AWS bảo mật của cloud, khách hàng bảo mật trong cloud.
- **Top Cloud Threats tại Việt Nam**
    - Public S3 / DB / Redis
    - Lộ access key
    - IAM cấu hình sai
    - EC2 bị malware (mining)
    - Thiếu logging hoặc không bật GuardDuty

#### Pillar 1 — Identity & Access Management (IAM)

#### Pillar 1 — Identity & Access Management (IAM)

- **IAM Users**: gần như không còn phù hợp — ưu tiên sử dụng **Roles + SSO + OIDC**.
- **IAM Identity Center (SSO)**: dùng cho quản lý đa tài khoản (multi-account management).
- **Quyền tổ chức**: áp dụng **SCP** và **permission boundaries** để kiểm soát quyền ở mức tổ chức.
- **Bảo mật chứng thực & giám sát**: bắt buộc **credential rotation**, **MFA**; dùng **Access Analyzer** để phát hiện các policy quá rộng.

#### Pillar 2 — Detection & Monitoring

- **CloudTrail (organization level)**: lưu tất cả API activity để audit và điều tra sự cố.
- **GuardDuty**: phát hiện hành vi bất thường trên EC2, IAM, S3, EKS.
- **Security Hub**: tổng hợp findings từ nhiều dịch vụ vào một dashboard trung tâm.
- **Logging sources**: VPC Flow Logs, S3 access logs, ALB logs — dùng để truy vết network & access patterns.
- **Alerting & Automation**: sử dụng EventBridge để gửi cảnh báo và khởi chạy phản ứng tự động (remediation).
- **Detection-as-Code**: quản lý các rule detection như mã nguồn (IaC) để dễ review và triển khai.

> Case study: Nếu không có logs thì không có bằng chứng — không thể điều tra incident.

#### Pillar 3 — Infrastructure Protection

- **VPC segmentation**: giảm "blast radius" khi hệ thống bị tấn công bằng cách cô lập các vùng mạng.
- **Subnet placement (Public vs Private)**:
    - **Public**: ALB, NAT Gateway.
    - **Private**: EC2 (app), Databases, internal services.
- **Security Group (SG) vs Network ACL (NACL)**:
    - **SG** = stateful, áp dụng ở mức instance.
    - **NACL** = stateless, áp dụng ở mức subnet.
- **Edge protection**: WAF, Shield Advanced, Network Firewall cho lớp phòng thủ biên.
- **Workload hardening**: best practices cho EC2, ECS, EKS security (patching, minimal IAM, image scanning, runtime protections).

> Case study: Hầu hết lỗi bảo mật đến từ việc đặt sai tài nguyên vào `public subnet` — luôn kiểm tra placement trước khi deploy.

#### Pillar 4 — Data Protection

- **Encryption everywhere**: bật mã hóa cho `S3`, `EBS`, `RDS`, `DynamoDB` (at-rest).
- **AWS KMS**:
    - Quản lý `Key policies`, `Grants`, và `Rotation` cho key lifecycle.
- **Encryption layers**: tách biệt `encryption at-rest` và `in-transit` (TLS/HTTPS).
- **Secrets management**: dùng **Secrets Manager** / **Parameter Store**:
    - Secret rotation tự động.
    - Quản lý credential theo best-practice của AWS.
- **Data classification & guardrails**: phân loại dữ liệu theo mức độ nhạy cảm và áp dụng guardrails (IAM, encryption, access controls).

> Case study: Nếu encrypt từ đầu, rủi ro data breach có thể giảm ~50%.

#### Pillar 5 — Incident Response (IR)

- **AWS IR lifecycle**: Prepare → Detect → Investigate → Respond → Recover.

- **Playbooks (ví dụ thực tế):**
    - IAM key bị compromise
    - S3 bucket public
    - EC2 nhiễm malware / mining

- **Công cụ & kỹ thuật xử lý:**
    - **Snapshot** (EBS/instance snapshot) để lưu evidence.
    - **Isolation** bằng thay đổi Security Group để ngắt kết nối instance.
    - **Evidence collection** cho forensic và audit.

- **Tự động hóa Incident Response:**
    - **Lambda** – chạy remediation nhỏ, cleanup.
    - **Step Functions** – điều phối quy trình IR phức tạp.
    - **EventBridge** – trigger playbooks khi phát hiện sự kiện.

> Case study: Incident response không thể phụ thuộc hoàn toàn vào con người — cần tự động hóa càng nhiều càng tốt.

### Những Gì Học Được

#### Tư Duy Bảo Mật Hiện Đại

- **Zero Trust & Least Privilege**: ưu tiên mô hình không tin tưởng mặc định và hạn chế quyền ở mức tối thiểu.
- **Defense in Depth**: xây dựng nhiều lớp phòng thủ để giảm rủi ro khi một lớp bị vượt qua.
- **Traceability & IaC**: mọi thay đổi phải traceable và reproducible — triển khai hạ tầng bằng **Infrastructure as Code (IaC)**.
- **Không tin vào cấu hình tay**: tránh cấu hình thủ công, ưu tiên mã hoá và kiểm thử.

#### Kiến Trúc Kỹ Thuật Quan Trọng

- **IAM**
    - **Roles > Users**
    - **SSO > local credentials**
    - **OIDC > Access Keys**

- **Network**
    - **Private-first**: luôn ưu tiên đặt tài nguyên trong private placement.
    - **Security Group (SG)** là "wall chính" (stateful); **NACL** là lớp bổ trợ (stateless).
    - **Outbound filtering** quan trọng tương đương inbound filtering.

- **Data**
    - **Encrypt by default** (S3, EBS, RDS, DynamoDB).
    - **Secret rotation** theo chính sách.
    - **Zero exposure** cho S3/DB — hạn chế public access.

- **Detection**
    - **CloudTrail ON**
    - **GuardDuty ON**
    - **Logging đầy đủ** để phục vụ điều tra sự cố.

- **Incident Response (IR)**
    - Có **playbook** rõ ràng.
    - Có **automation** (remediation, rollback).
    - Có **rollback / snapshot** cho phục hồi nhanh.

#### Chiến Lược Hiện Đại Hóa

Không làm ồ ạt → Phased approach.

Dùng multi-account architecture để giảm rủi ro.

### Ứng Dụng Vào Công Việc

Thiết kế IAM của dự án theo Roles + Permission Sets.

Áp dụng VPC segmentation và “private-first design”.

Thêm GuardDuty + CloudTrail org-level cho toàn môi trường dev/prod.

Setup Alerting & IR automation (EventBridge → Lambda).

Áp dụng Secret Manager + rotation → không hard-code secret.

Triển khai IaC (Terraform/CDK) để giảm drift.

### Trải nghiệm trong event

- Tham gia workshop **“AWS Cloud Mastery Series #3”** là một trải nghiệm rất bổ ích, giúp tôi có cái nhìn toàn diện về cách hiện đại hóa ứng dụng và cơ sở dữ liệu bằng các phương pháp và công cụ hiện đại. Một số trải nghiệm nổi bật:

#### Học hỏi từ các diễn giả có chuyên môn cao

- Hiểu chi tiết cách AWS xử lý incident & detection thực tế.

#### Trải nghiệm kỹ thuật thực tế

- Học cách phân tích policy bằng IAM Simulator.
- Nhìn thấy flow thực của S3 public exposure → auto-remediation.
- Quan sát IR automation xử lý compromised EC2.

#### Ứng dụng công cụ hiện đại

- Thấy rõ vai trò của security automation & đa tầng.
- Học OIDC, cross-account guardrails, detection as code.

#### Kết nối và tư duy

- Nhận ra cách doanh nghiệp lớn tổ chức bảo mật theo multi-account.
- Tư duy “secure by design”, không chờ đến khi bị tấn công mới sửa.

#### Bài học rút ra

- Security = culture, not a feature.
- Cấu hình sai là nguyên nhân lớn nhất → IaC là cứu cánh.
- Không thể vận hành cloud an toàn nếu thiếu IAM + Logging + IR

#### Một số hình ảnh khi tham gia sự kiện
![Sự kiện](/images/4-EventParticipated/29.11(1).jpg)
> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi hiểu rõ hơn về tư duy bảo mật hiện đại, tự động hóa quy trình bảo mật và cải thiện khả năng ứng phó sự cố trong môi trường cloud.
