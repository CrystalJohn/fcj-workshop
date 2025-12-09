---
title: "Week 12 Worklog"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---

### Week 12 Objectives:

* Complete RAG Chat component (M2) — Rate limiting, fallback, error handling
* Start Testing & Golive (M3) — Login page, Chat UI, Admin dashboard

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Implement Rate Limiter (60 RPM, 100K TPM) <br> - Add Budget Manager ($10/day, $200/month) <br> - Write tests for rate limiting logic                                                                | 11/24/2025 | 11/24/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 3   | - Implement fallback to Claude Haiku <br> - Add error handling & retry logic with exponential backoff <br> - Write tests (35 tests passed)                                                            | 11/25/2025 | 11/25/2025      | <https://docs.aws.amazon.com/bedrock/>    |
| 4   | - Implement login page with Cognito <br> - Build AuthService with JWT validation <br> - Test authentication flow end-to-end                                                                           | 11/26/2025 | 11/26/2025      | <https://docs.aws.amazon.com/cognito/>    |
| 5   | - Build Chat interface UI <br> - Implement ChatPage with message bubbles, citations <br> - Create `CitationCard`, `DocumentViewerModal` components                                                    | 11/27/2025 | 11/27/2025      | <https://react.dev/>                      |
| 6   | - Build Admin dashboard with drag-drop upload <br> - Show document processing status (real-time auto-refresh) <br> - Start E2E integration testing                                                    | 11/28/2025 | 11/28/2025      | <https://react.dev/>                      |


### Week 12 Achievements:

* **Completed RAG Chat (M2):**
  * Rate Limiter
  * Budget Manager: $10/day, $200/month limit (22 tests passed)
  * Bedrock Retry with exponential backoff (35 tests passed)
  * Fallback from Claude 3.5 Sonnet → Claude Haiku when rate limited

* **Started Testing & Golive (M3):**
  * AuthService with JWT validation — user authentication via Cognito
  * ChatPage with message bubbles and citations — main chat interface
  * `CitationCard`, `DocumentViewerModal` components — display document sources
  * AdminPage with drag-drop upload — document management
  * Real-time status monitoring with auto-refresh

* **Learned:**
  * Rate limiting patterns for AI APIs (token-based & request-based)
  * Cost management strategies for Bedrock (budget alerts, fallback models)
  * Exponential backoff with jitter for retry logic
  * React components for chat UI (message streaming, citations)
  * Cognito JWT validation in frontend
