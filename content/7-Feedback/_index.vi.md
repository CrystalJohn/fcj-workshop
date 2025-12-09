---
title: "Chia sẻ, đóng góp ý kiến"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

### Đánh giá chung

#### 1. Môi trường làm việc

Chương trình First Cloud Journey thật sự là một trải nghiệm tuyệt vời, chương trình được thiết kế ra đã giúp cho tôi hiểu biết và trải nghiệm service AWS (1 trong những Cloud rất mạnh). Chương trình học toàn diện kéo dài 12 tuần cho phép tôi tiến bộ một cách có hệ thống từ kiến thức AWS cơ bản đến triển khai dự án **ARC-Chatbot** — một ứng dụng RAG Chatbot MVP. Cấu trúc worklog hàng tuần giúp tôi có có thể ghi nhận lại tiến độ công việc, tình trạng học tập hiệu quả. Môi trường khuyến khích tự học trong khi vẫn cung cấp đầy đủ tài nguyên và hỗ trợ khi cần.

#### 2. Sự hỗ trợ của mentor / team

Sự hướng dẫn và hợp tác nhóm trong suốt chương trình rất xuất sắc. Team luôn sẵn sàng thảo luận các thách thức kỹ thuật, xem xét đề xuất kiến trúc của tôi và hướng dẫn qua các khái niệm AWS phức tạp như Bedrock, Textract, và RAG + IDP pipeline. Điều tôi đánh giá cao nhất là mentor luôn khuyến khích tôi tự nghiên cứu và đề xuất hướng giải quyết trước khi hỗ trợ — giúp tôi phát triển tư duy độc lập.

#### 3. Sự phù hợp giữa công việc và chuyên ngành học

Nội dung chương trình rất phù hợp với các thực hành điện toán đám mây và kỹ thuật phần mềm hiện đại. Bắt đầu từ các kiến thức nền tảng về mạng (VPC, Security Groups) và tiến tới kiến trúc serverless, database, security và các dịch vụ AI/ML giúp tôi xây dựng một bộ kỹ năng cloud engineering khá toàn diện. 

Dự án thực hành **ARC-Chatbot** — xây dựng chatbot tích hợp AI với admin dashboard, IDP pipeline, và RAG chat — cho tôi kinh nghiệm thực tế áp dụng trực tiếp vào các tình huống thực tế. Việc nhấn mạnh vào Infrastructure as Code (Terraform, CDK) và các thực hành DevOps hoàn toàn phù hợp với tiêu chuẩn ngành hiện tại.

#### 4. Cơ hội học hỏi & phát triển kỹ năng

Hành trình 12 tuần đem đến cho tôi rất nhiều cơ hội học hỏi trên nhiều khía cạnh:

- **Kỹ năng kỹ thuật:** Học được cách sử dụng hơn 15 dịch vụ AWS (`Bedrock`, `Textract`, `SageMaker`, `DynamoDB`, `S3`, `Cognito`, `SQS`, `Lambda`, `ECS`, `ECR`, `ALB`, `CloudWatch`, `X-Ray`, `VPC`, `IAM`...)
- **AI/ML Integration:** Học cách tích hợp Claude 3.5, Cohere Embeddings, RAG pipeline, và Qdrant vector database
- **Thiết kế kiến trúc:** Học cách thiết kế giải pháp đám mây có khả năng mở rộng, tiết kiệm chi phí
- **Tài liệu hóa:** Cải thiện kỹ năng viết kỹ thuật qua worklog hàng tuần và dịch các bài blog AWS
- **Giải quyết vấn đề:** Phát triển kỹ năng debug bằng cách giải quyết các vấn đề như rate limiting, PDF processing, và vector search optimization
- **Tối ưu chi phí:** Học cách đưa ra quyết định kiến trúc dựa trên phân tích chi phí-lợi ích (~$65/month estimation)

Sự tiến bộ từ **AWS Fundamentals (Tuần 1-4)** → **S3 & Security (Tuần 5)** → **Serverless (Tuần 6)** → **Containers (Tuần 7)** → **AI/ML (Tuần 8)** → **Architecture Design (Tuần 9)** → **Full Implementation (Tuần 10-12)** có tốc độ hợp lý và logic.

#### 5. Văn hóa & tinh thần đồng đội

Chương trình FCJ nuôi dưỡng văn hóa học tập liên tục, hợp tác và đổi mới. Các cuộc họp nhóm mang tính xây dựng, với thảo luận cởi mở về các đánh đổi kỹ thuật và quyết định kiến trúc. Các workshop như **"AWS Cloud Mastery Series"** và **"Building Agentic AI"** tạo cơ hội mở rộng kiến thức ngoài chương trình cốt lõi. 

#### 6. Chính sách / hỗ trợ của chương trình

Cấu trúc chương trình được thiết kế tốt với các mục tiêu hàng tuần rõ ràng, tài liệu tham khảo toàn diện (AWS Study Group, Cloud Journey, AWS docs chính thức) và các bài lab thực hành. Sự linh hoạt để khám phá các phương pháp khác nhau (nhiều tùy chọn database, vector search, PDF extraction) trong khi vẫn tập trung vào dự án chính rất có giá trị. 

Quyền truy cập vào tài khoản AWS và hướng dẫn về quản lý ngân sách giúp học cách tối ưu chi phí ngay từ ngày đầu.

---

### Phản hồi chi tiết về các giai đoạn chương trình

**Giai đoạn 1 (Tuần 1-4): AWS Fundamentals & Core Services**
- **Điểm mạnh:** Giới thiệu đầy đủ và có hệ thống về AWS, VPC, EC2, Linux deployment. Các bài lab thực hành củng cố kiến thức lý thuyết.

**Giai đoạn 2 (Tuần 5-6): Security, Serverless & Databases**
- **Điểm mạnh:** Học S3, IAM, Security best practices (Tuần 5). Tiến triển vào Lambda, API Gateway, DynamoDB (Tuần 6).

**Giai đoạn 3 (Tuần 7-8): Containers & AI/ML Services**
- **Điểm mạnh:** Học Docker, ECR, ECS (Tuần 7). Nghiên cứu Bedrock, Textract và các dịch vụ AI/ML (Tuần 8).

**Giai đoạn 4 (Tuần 9): Architecture Design**
- **Điểm mạnh:** Thiết kế architecture cho ARC-Chatbot project, lập kế hoạch triển khai và cost estimation.

**Giai đoạn 5 (Tuần 10-12): Full Implementation & Testing**
- **Điểm mạnh:** Triển khai thực tế — Assessment M0 (Tuần 10), IDP Pipeline M1 (Tuần 11), RAG Chat M2 & Testing M3 (Tuần 12). Kinh nghiệm end-to-end deployment cực kỳ hữu ích.

---

### Điều tôi hài lòng nhất

- **Hoàn thiện giải pháp Production-ready:** Triển khai thành công hệ thống ARC-Chatbot hoàn chỉnh từ con số 0, bao gồm IDP pipeline, RAG chat, admin dashboard với tổng cộng **200+ tests passed**.

- **Thành tựu kỹ thuật:**
  - PDF Detector service (17 tests passed)
  - PDF Extractor với PyPDF2 (30 tests passed)
  - Qdrant vector search (35 tests passed)
  - Claude Service với streaming (20 tests passed)
  - Rate Limiter & Budget Manager (45 tests passed)
  - Bedrock Retry với exponential backoff (35 tests passed)

- **Chiến thắng trong giải quyết vấn đề:** Vượt qua các thách thức kỹ thuật như digital vs scanned PDF detection, text chunking optimization, và rate limiting cho AI APIs.

- **Tiến bộ đo lường được:** Từ người mới bắt đầu với AWS vào Tuần 1, đến việc triển khai tự tin các kiến trúc AI/ML đa dịch vụ vào Tuần 12.

---

### Tôi có giới thiệu chương trình này không?

**Chắc chắn có!** Tôi rất khuyến khích chương trình First Cloud Journey cho bất kỳ ai quan tâm đến điện toán đám mây vì nhiều lý do:

- **Chương trình toàn diện:** Bao quát tất cả các nhóm dịch vụ AWS thiết yếu từ cơ bản đến nâng cao
- **Phương pháp thực hành:** Triển khai dự án thực tế cung cấp kinh nghiệm thực tế vượt xa các chứng chỉ
- **Tiến triển có cấu trúc:** Khung thời gian 12 tuần với các mốc rõ ràng giúp duy trì động lực
- **Cộng đồng hỗ trợ:** Mentor và thành viên nhóm tích cực giúp đỡ với các thách thức kỹ thuật
- **Phù hợp với nghề nghiệp:** Kỹ năng học được (IaC, serverless, security, AI/ML, tối ưu chi phí) áp dụng trực tiếp vào các vị trí trong ngành
- **Tính linh hoạt:** Chương trình cho phép khám phá các dịch vụ khác nhau trong khi tập trung vào mục tiêu cốt lõi

> *"Chương trình đã giúp tôi chuyển từ hiểu biết lý thuyết sang khả năng triển khai thực tế."*

---

### Đề xuất & mong muốn cho các khóa tương lai

**Đề xuất để cải thiện trải nghiệm thực tập:**

1. **Chuẩn bị trước thực tập tốt hơn**
   - Cung cấp danh sách đọc trước hoặc chuỗi video về các khái niệm cloud cơ bản
   - Thiết lập tài khoản AWS và CLI trước Tuần 1 để tối đa hóa thời gian thực hành

2. **Retrospectives hàng tuần**
   - Các buổi reflection có cấu trúc mỗi tuần để thảo luận thách thức và bài học
   - Chia sẻ các vấn đề phổ biến và giải pháp trên tất cả các thực tập sinh

3. **Buổi diễn giả khách mời**
   - Mời AWS Solutions Architects hoặc chuyên gia cloud chia sẻ kinh nghiệm thực tế
   - Thông tin chi tiết về ngành về các dịch vụ mới nổi và best practices

4. **Trình bày Capstone**
   - Trình bày chính thức về dự án cuối cùng cho mentor và đồng nghiệp
   - Thực hành truyền đạt quyết định kỹ thuật cho nhiều đối tượng khác nhau

5. **Mạng lưới Alumni**
   - Kết nối với những người tham gia chương trình FCJ trước đây
   - Cơ hội mentorship cho các thực tập sinh tương lai

**Tôi có muốn tiếp tục chương trình này trong tương lai không?**

Có, tôi rất muốn tiếp tục với các chương trình nâng cao như:
- **Chuyên môn AWS nâng cao:** Machine Learning, Data Analytics hoặc Security specialty tracks
- **Chuẩn bị chứng chỉ AWS:** Solutions Architect Professional, DevOps Engineer
- **Đóng góp mã nguồn mở:** Đóng góp vào AWS CDK constructs hoặc các dự án cộng đồng

---

### Góp ý khác

Chương trình First Cloud Journey vượt quá mong đợi của tôi về mọi mặt. 12 tuần học từ nền tảng AWS đến khi tự triển khai một giải pháp ARC-Chatbot hoàn chỉnh thực sự giúp tôi tự tin hơn hẳn. Việc nhấn mạnh vào học tập thực hành, tối ưu chi phí và best practices đã chuẩn bị tốt cho tôi cho sự nghiệp trong điện toán đám mây.

**Điều tôi trân trọng nhất trong chương trình là:**

- Sự tự do để khám phá các phương pháp kiến trúc khác nhau và học hỏi từ sai lầm
- Môi trường nhóm hỗ trợ khuyến khích đặt câu hỏi và giải pháp sáng tạo
- Yêu cầu phải tài liệu hóa đầy đủ đã giúp tôi nâng cao đáng kể kỹ năng viết kỹ thuật
- Dự án thực tế yêu cầu tích hợp nhiều dịch vụ AWS (Bedrock, Textract, DynamoDB, S3, Cognito, SQS, ECS...)

> *Cảm ơn team FCJ, các mentor và các thực tập sinh đồng nghiệp đã có trải nghiệm học tập tuyệt vời. Kiến thức và kỹ năng đạt được trong 12 tuần này sẽ là nền tảng vững chắc cho sự nghiệp tương lai của tôi trong điện toán đám mây và kỹ thuật phần mềm.*

---

### Tóm tắt đánh giá

| Hạng mục | Đánh giá |
|----------|----------|
| Môi trường làm việc | ⭐⭐⭐⭐⭐ (5/5) |
| Hỗ trợ Mentor | ⭐⭐⭐⭐⭐ (5/5) |
| Sự phù hợp chương trình | ⭐⭐⭐⭐⭐ (5/5) |
| Cơ hội học tập | ⭐⭐⭐⭐⭐ (5/5) |
| Văn hóa nhóm | ⭐⭐⭐⭐⭐ (5/5) |
| Cấu trúc chương trình | ⭐⭐⭐⭐☆ (4.5/5) |
| **Đánh giá chung** | **⭐⭐⭐⭐⭐ (5/5)** |