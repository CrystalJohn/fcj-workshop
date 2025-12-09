---
title: "Event 4"
date: "15/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “AWS Cloud Mastery Series #1 workshop”

### Mục Đích Của Sự Kiện

Giới thiệu hệ sinh thái AI/ML/GenAI trên AWS.

Hướng dẫn triển khai ML end-to-end bằng SageMaker.

Xây dựng ứng dụng Generative AI qua Amazon Bedrock, RAG, Agents.

Làm quen với MLOps, IaC, CICD, container workflows phục vụ AI/ML.

Xây dựng tư duy vận hành AI ở môi trường doanh nghiệp.
### Danh Sách Diễn Giả

- **AWS AI/ML Specialist Team** – AWS Vietnam
- **AWS Solutions Architect**
- **AI/ML Community Leader in Vietnam**

### Nội Dung Nổi Bật

#### AWS AI/ML Services Overview

- **Amazon SageMaker** — nền tảng ML end-to-end
    - Data preparation: **SageMaker Data Wrangler**, **Ground Truth** (data labeling), **Feature Store** (quản lý feature tái sử dụng)

- **Training & Tuning**
    - Training jobs → autoscaling GPU/CPU
    - Hỗ trợ distributed training
    - Hyperparameter tuning (Auto-tuning, Bayesian Optimization)

- **Deployment Options**
    - Real-time endpoint
    - Serverless inference
    - Multi-model endpoint
    - Asynchronous (async) inference

- **MLOps**
    - Model registry
    - CI/CD pipelines bằng **SageMaker Pipelines**
    - Monitoring drift: data drift, feature drift, model drift

- **Live demo**
    - Trải nghiệm **SageMaker Studio**: notebook, xử lý dữ liệu, chạy training và deploy model.
#### Generative AI with Amazon Bedrock
#### Generative AI with Amazon Bedrock

- **Foundation models introduced**
    - **Claude 3.5** — strong reasoning, long context, good for coding tasks
    - **Llama 3** — open-weight, suitable for fine-tuning, cost-effective
    - **Titan** — high-quality embeddings for RAG (retrieval-augmented generation)
    - **Mistral** — fast, lightweight

- **Prompt engineering techniques**
    - Zero-shot, few-shot
    - Chain-of-Thought
    - Role prompting
    - Multi-step reasoning
    - Prompt templates

- **Best practices**
    - Chunking hợp lý (chunking inputs appropriately)
    - Metadata filtering
    - Re-ranking
    - Apply guardrails to ensure safety and control

#### Bedrock Agents

- Agents có thể tự thực thi workflow multi-step.
- Kích hoạt `Lambda` để gọi API, database và các workflow bên ngoài.
- Có thể thay thế logic backend trong nhiều use case (orchestration của business logic).
- Kết hợp chặt chẽ với `EventBridge` và `Step Functions` trong pipeline AI để điều phối và retry.

#### CICD Workflow for Containers (ECR + ECS)

1. Developer commit → `CodeCommit`
2. `CodeBuild` builds the image
3. Push image vào `ECR`
4. `ECS` pull image để deploy (Fargate / EC2)
5. `CloudWatch` giám sát logs & metrics
6. `CodePipeline` orchestrate toàn bộ quy trình

Add security (DevSecOps):

- `CodeBuild` validation (unit tests, static analysis)
- Image scanning trong `ECR` (vulnerability scan)
- Deployment policies / IAM controls để kiểm soát việc deploy

→ Đây là DevSecOps chuẩn AWS.

### Những Gì Học Được

#### Kiến thức AI/ML nền tảng, GenAI

- ML lifecycle
- Feature engineering
- Model deployment & monitoring
- Cost optimization cho training/inference
- Cách chọn Foundation Model phù hợp với use case
- Prompt engineering nâng cao
- RAG architecture cho production (retrieval-augmented generation)
- Build Agents để thực thi workflow tự động

#### Kiến Trúc Kỹ Thuật Quan Trọng

#### Kiến Trúc Kỹ Thuật Quan Trọng

- **IaC (Infrastructure as Code):** `Terraform` vs `CloudFormation` vs `CDK`
- **Container inference workflow:** `ECS` / `ECR` cho AI inference container
- **Monitoring & Observability:** giám sát AI workloads với `CloudWatch` + `X-Ray`

### Ứng Dụng Vào Công Việc

- Build chatbot doanh nghiệp với `Bedrock` + RAG (retrieval-augmented generation)
- Thiết kế kiến trúc AI end-to-end (data → ML → app)
- Dùng `Lambda` + `Step Functions` để điều phối RAG pipeline
- Dùng `Terraform` / `CDK` để xây hạ tầng GenAI
- Triển khai inference container qua `ECS` / `Fargate`

### Trải nghiệm trong event

- Tham gia workshop **“AWS Cloud Mastery Series #1”** tôi thấy rõ cách AWS triển khai AI/ML real-world, hiểu sự khác nhau ML truyền thống và GenAI, tăng tư duy thiết kế AI architecture.

#### Học hỏi từ các diễn giả có chuyên môn cao

- Hiểu chi tiết cách AWS xây dựng hệ sinh thái AI/ML/GenAI toàn diện.


#### Ứng dụng công cụ hiện đại

- **SageMaker** — `SageMaker` giúp làm ML end-to-end: chuẩn bị dữ liệu, training, tuning, deployment và MLOps.
- **Bedrock** — cung cấp Foundation Models (`Claude`, `Llama`, `Titan`) và hỗ trợ RAG + Agents để xây chatbot và workflow AI nhanh.
- **IaC & DevOps** — `CloudFormation`, `CDK`, `Terraform`, `CodePipeline` giúp triển khai AI/ML có kiểm soát và tự động.
- **Data & ETL Tools** — `Glue`, `S3` (Data Lake), `EventBridge`, `Step Functions` hỗ trợ pipeline dữ liệu cho AI.
- **Monitoring** — `CloudWatch` cho giám sát AI/ML, logging và drift detection.

#### Kết nối và tư duy

- Học tư duy AI system thay vì chỉ làm mô hình ML riêng lẻ.
- Nắm xu hướng AI/ML tại Việt Nam và cách doanh nghiệp ứng dụng.

#### Bài học rút ra

- GenAI và ML truyền thống kết hợp để tạo giải pháp AI mạnh trong thực tế.

- GenAI và ML truyền thống kết hợp để tạo giải pháp AI mạnh trong thực tế.
- `Bedrock` là nền tảng GenAI doanh nghiệp: an toàn, triển khai nhanh, không cần tự train model.
- `MLOps` + `IaC` là bắt buộc để AI chạy production ổn định.
- `RAG` là cách “đưa kiến thức doanh nghiệp vào mô hình” hiệu quả nhất.

#### Một số hình ảnh khi tham gia sự kiện
![Sự kiện](/images/4-EventParticipated/15.11-event.jpg)
> Tổng thể, sự kiện không chỉ cung cấp kiến thức kỹ thuật mà còn mang đến cho tôi tư duy thiết kế hệ thống AI/ML toàn diện, từ data đến deployment và vận hành, giúp tôi tự tin hơn trong việc xây dựng các giải pháp AI thực tế cho doanh nghiệp.