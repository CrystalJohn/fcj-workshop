---
title: "Worklog Tuần 12"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---

### Mục tiêu tuần 12:

* Hoàn thiện RAG Chat component (M2) — Rate limiting, fallback, error handling
* Bắt đầu Testing & Golive (M3) — Login page, Chat UI, Admin dashboard

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Implement Rate Limiter (60 RPM, 100K TPM) <br> - Add Budget Manager ($10/day, $200/month) <br> - Viết tests cho rate limiting logic                                                      | 24/11/2025   | 24/11/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 3   | - Implement fallback to Claude Haiku <br> - Add error handling & retry logic với exponential backoff <br> - Viết tests (35 tests passed)                                                   | 25/11/2025   | 25/11/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 4   | - Implement login page với Cognito <br> - Build AuthService với JWT validation <br> - Test authentication flow end-to-end                                                                  | 26/11/2025   | 26/11/2025      | <https://docs.aws.amazon.com/cognito/>    |
| 5   | - Build Chat interface UI <br> - Implement ChatPage với message bubbles, citations <br> - Tạo `CitationCard`, `DocumentViewerModal` components                                             | 27/11/2025   | 27/11/2025      | <https://react.dev/>                      |
| 6   | - Build Admin dashboard với drag-drop upload <br> - Show document processing status (real-time auto-refresh) <br> - Bắt đầu integration testing E2E                                        | 28/11/2025   | 28/11/2025      | <https://react.dev/>                      |


### Kết quả đạt được tuần 12:

* **Hoàn thiện RAG Chat (M2):**
  * Rate Limiter
  * Budget Manager: $10/day, $200/month limit (22 tests passed)
  * Bedrock Retry với exponential backoff (35 tests passed)
  * Fallback từ Claude 3.5 Sonnet → Claude Haiku khi rate limit

* **Bắt đầu Testing & Golive (M3):**
  * AuthService với JWT validation — xác thực người dùng qua Cognito
  * ChatPage với message bubbles và citations — giao diện chat chính
  * `CitationCard`, `DocumentViewerModal` components — hiển thị nguồn tài liệu
  * AdminPage với drag-drop upload — quản lý documents
  * Real-time status monitoring với auto-refresh

* **Học được:**
  * Rate limiting patterns cho AI APIs (token-based & request-based)
  * Cost management strategies cho Bedrock (budget alerts, fallback models)
  * Exponential backoff với jitter cho retry logic
  * React components cho chat UI (message streaming, citations)
  * Cognito JWT validation trong frontend


