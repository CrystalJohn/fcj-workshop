---
title: "Event 3"
date: "17/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “AWS Cloud Mastery Series #2 workshop”

### Mục Đích Của Sự Kiện

- Trang bị tư duy DevOps hiện đại và cách áp dụng trên hệ sinh thái AWS.
- Hướng dẫn triển khai CI/CD, IaC, container orchestration, và monitoring đúng chuẩn AWS.
- Tối ưu tốc độ phát triển, chất lượng release và độ tin cậy của hệ thống.
### Danh Sách Diễn Giả

- **FCJ Team** 
- **Kha Van** - Cloud Security Engineer
- **AWS Developer Advocate Team**

### Nội Dung Nổi Bật

#### AWS DevOps Services – CI/CD Pipeline

- **Build**: tối ưu `buildspec.yml` (build caching & parallel test).
- **Demo**: pipeline end-to-end — Commit → Build → Test → Deploy → Monitor.
- **Deployment**: phải tự động hóa 100% — không click-ops (no manual deployments).

#### Infrastructure as Code

- **Tools:** `CloudFormation` / `Terraform` / `CDK` — dùng để triển khai hạ tầng (CloudFormation là dịch vụ quản lý của AWS).
- **Template anatomy (YAML):** `Resources` → `Parameters` → `Outputs` → `Mappings`.
- **CDK model:** Constructs → Stacks → Apps. Hỗ trợ nhiều ngôn ngữ (TypeScript, Python, Java...).
- **Benefits:** dễ reuse, dễ modular hóa, dễ test.
- **When to choose:**
    - **CloudFormation:** declarative, phù hợp teams enterprise.
    - **CDK:** developer-friendly, phù hợp Agile teams.

> “Không IaC = không DevOps.”
#### Container Services on AWS

- **Docker fundamentals**
    - `Dockerfile` → Build → Image → Registry → Run container
    - Registry: Docker Hub hoặc **Amazon ECR**
- **Amazon ECR**
    - Image scanning, lifecycle policies, permissions per repository
- **Amazon ECS**
    - Chạy container với **Fargate** hoặc **EC2**
    - Application Load Balancer, auto-scaling theo CPU, memory, queue length
- **Amazon EKS**
    - Managed Kubernetes service
    - Use cases: large-scale, multi-team, portable workloads
- **AWS App Runner**
    - Dành cho teams muốn “deploy container như deploy Vercel”
>Case study: So sánh `ECS`, `EKS`, `App Runner` cho microservices, “Containers = scalability + portability. ECS/EKS giúp production hoạt động trơn tru.”

#### Monitoring & Observability

- **CloudWatch**
    - Metrics, Logs, Dashboards
    - Composite alarms
    - Custom metrics cho business KPIs

- **AWS X-Ray**
    - Distributed tracing
    - Debug latency, bottlenecks, service maps

- **Best practices**
    - Thiết kế alerting để tránh noise
    - On-call workflow và runbook rõ ràng
    - Giám sát theo Golden Signals: Latency, Traffic, Errors, Saturation

> “Không observability = không biết hệ thống đang chết như thế nào.”

### Những Gì Học Được

#### Tư Duy DevOps Hiện Đại

- Tập trung vào **outcome**, không phải tools.
- **DORA metrics**: tiêu chuẩn đo hiệu suất phần mềm.
- Continuous Integration → Continuous Delivery → Continuous Learning.

#### Kiến Trúc Kỹ Thuật Quan Trọng

- **CI/CD (AWS)**: CodePipeline, CodeBuild, CodeDeploy — thiết kế pipeline theo chuẩn.
- **IaC** là xương sống của automation (CloudFormation / Terraform / CDK).
- **Containerization** giúp microservices scale và di động.
- **Observability**: phát hiện lỗi nhanh, giảm MTTR bằng metrics, logs và tracing.

#### Chiến Lược DevOps

- Bắt đầu từ small pipeline → mở rộng dần.
- Dùng **blue/green** và **canary** để giảm rủi ro khi deploy.
- Áp dụng **IaC + GitOps** cho môi trường multi-team.

### Ứng Dụng Vào Công Việc

- Xây dựng CI/CD cho các dự án backend/web (CodePipeline / CodeBuild / CodeDeploy).
- Đóng gói service bằng Docker và deploy lên `ECS` (Fargate) hoặc `App Runner`.
- Sử dụng `CDK` để build hạ tầng AWS thay vì thao tác thủ công trên console.
- Quan sát hệ thống bằng CloudWatch (dashboards, alarms, custom metrics).
- Thiết lập incident workflow cho dự án: alert → investigate → fix → postmortem (runbooks & on-call).

### Trải nghiệm trong event

- Tham gia workshop **“AWS Cloud Mastery Series #2”** là một trải nghiệm rất bổ ích, giúp tôi có thêm tư duy của 1 DevOps cần có.

#### Học hỏi từ các diễn giả có chuyên môn cao

- Hiểu chi tiết cách AWS xử lý incident & detection thực tế.

#### Trải nghiệm kỹ thuật thực tế

- Demo pipeline từ commit → deploy live.
- Demo drift detection, CDK synth/deploy.
- Triển khai container real-time trên ECS/ECR.

#### Ứng dụng công cụ hiện đại

- CloudFormation + CDK giúp IaC rõ ràng, lặp lại được.
- ECR scanning tăng bảo mật.
- X-Ray giúp debug microservices dễ dàng.

#### Kết nối và tư duy

- Hiểu rõ cách teamwork Dev/Product/DevOps phối hợp trong CI/CD pipeline.
- Tư duy “automate everything” ăn sâu hơn.

#### Bài học rút ra

- Không có CI/CD → không có DevOps.
- IaC là điều kiện tiên quyết.
- Containers → scalability + speed.
- Observability → reliability.

#### Một số hình ảnh khi tham gia sự kiện
![Sự kiện](/images/4-EventParticipated/29.11-event.jpg)
> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn giúp tôi thay đổi cách tư duy DevOps, tự động hóa quy trình phát triển phần mềm và cải thiện hợp tác giữa các nhóm.
