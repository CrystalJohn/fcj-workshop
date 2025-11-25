---
title: "Proposal"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Academic Research Chatbot
### AWS RAG-based solution for smart academic support and research

#### 1. Executive Summary
**Academic Research Chatbot** is an AI assistant supporting academic research, helping students and lecturers search, summarize, and analyze scientific documents (PDFs, papers) through natural conversation with accurate source citations.

**Key Highlights:**
- **Core Technology**: Combines **IDP** (Amazon Textract) to process documents (including scans) and **RAG** (Amazon Bedrock - Claude 3.5 Sonnet) to generate intelligent responses.
- **Optimized Architecture**: Hybrid model using 1 EC2 t3.small combined with Serverless services (Amplify, Cognito, S3, DynamoDB) to balance performance and cost.
- **Feasibility**: Serves ~50 internal users with operating costs ~$60/month, fast deployment time (20 days), and maximizes AWS Free Tier.

#### 2. Problem Statement

**Current Problem**
Students and researchers have to work with a large number of academic documents (conference papers, journals, theses, technical reports). Many documents are old scanned PDFs (pre-2000), without a text layer, making searching for content, data, and tables very time-consuming.
Public AI tools (ChatGPT, Perplexity, NotebookLM, etc.) are not directly connected to the school/department's internal document repository, making it difficult to ensure security and access rights by subject or research group.
The current infrastructure lacks a unified access point to:
- Manage research documents by subject/topic.
- Allow researchers to ask questions directly on their own papers.
- Ensure answers have clear citations (paper, page, table, section).
**Consequence:** Researchers have to manually read, take notes, and copy data from multiple papers; lecturers find it difficult to quickly synthesize information when preparing lectures or topics; academic data is scattered across many personal machines, difficult to standardize and reuse.

**Solution**
**Academic Research Chatbot** proposes building an internal academic Q&A platform based on AWS, where:
1.  **Dev/Admin loads research document repository:**
    -   Upload PDFs to **Amazon S3**, metadata is stored in **Amazon DynamoDB**.
    -   An EC2 worker consumes the **Amazon SQS** queue, calls **Amazon Textract** to OCR, extract text, tables, forms, including scanned documents.
    -   Worker normalizes/chunks content, sends to **Amazon Bedrock Titan Text Embeddings v2** to generate embeddings, and indexes into **Qdrant** on EC2.
2.  **Researchers ask questions via web interface (Amplify + CloudFront):**
    -   Questions are embedded, querying Qdrant to retrieve the most relevant segments (Retrieval).
    -   These segments are passed to **Claude 3.5 Sonnet** on **Amazon Bedrock** to generate answers with accurate citations (paper, page, section, table) and explanations in academic context.
All access is protected by **Amazon Cognito** (researcher vs admin authorization), logs & metrics are monitored via **Amazon CloudWatch + SNS** (alerts on worker errors, queue backlog, high EC2 CPU).

**Benefits and ROI**
**Academic Efficiency:**
-   Reduces 40–60% of time researchers spend finding data, F1-scores, p-values, sample sizes, experimental equipment, or method descriptions from multiple papers.
-   Reduces citation errors due to forgetting pages/tables, as the chatbot always returns sources and locations.
**Internal Knowledge Management:**
-   Research documents are centralized in an S3 + DynamoDB repository, easy to backup, authorize, and expand.
-   Can be reused for many courses, topics, and labs without building a new system.
**Low & Controllable Infrastructure Costs:**
-   Hybrid model 1 EC2 + managed AI services keeps operating costs for 50 internal users at around < $50/month, mainly paying for EC2, 2–3 VPC endpoint interfaces, and Bedrock/Textract usage.
-   The system is designed to be deployed in about 20 days by a team of 4, suitable for a research/internship project but still has product architecture quality.
**Long-term Value:**
-   Creates a platform to later integrate learning behavior analysis dashboards, paper recommendation modules, or expand to multi-language and multi-field learning assistants.

#### 3. Solution Architecture

Academic Research Chatbot applies the AWS Hybrid RAG Architecture model with IDP (Intelligent Document Processing), combining a single EC2 (FastAPI + Qdrant + Worker) with managed AI services (Textract, Bedrock) to optimize costs while ensuring performance for about 50 internal users.

**Data Processing and Conversation Flow**
![Architecture Diagram](/images/2-Proposal/FCJ-MVP-architecture.png)

**AWS Services Used**
-   **Amazon Route 53:** DNS management for chatbot platform domain.
-   **Amazon CloudFront:** CDN distributing web interface (chat + admin) with low latency.
-   **AWS Amplify Hosting:** Hosts web application (React/Next) for Researchers and Dev/Admin.
-   **Amazon Cognito:** User authentication, researcher vs admin role management.
-   **Amazon S3:** Stores original PDF files uploaded by Dev/Admin (raw documents).
-   **Amazon SQS (doc_ingestion_queue):** Job queue for document processing.
-   **Amazon Textract:** IDP/OCR for scanned PDFs and digital PDFs.
-   **Amazon Bedrock:**
    -   **Titan Text Embeddings v2:** Generates embedding vectors for text chunks.
    -   **Claude 3.5 Sonnet:** Generates academic answers from context + user questions (RAG).
-   **Amazon DynamoDB:** Documents table: document metadata, pipeline status (UPLOADED, IDP_RUNNING, EMBEDDING_DONE, FAILED).
-   **Amazon EC2 (t3.small, private subnet):**
    -   Runs FastAPI backend (REST API for chat and admin).
    -   Runs Qdrant Vector DB to store and query embeddings.
    -   Runs Worker process consuming SQS, calling Textract + Titan, indexing into Qdrant, updating DynamoDB.
-   **VPC + ALB + VPC Endpoints:**
    -   VPC + private subnet for EC2 (not directly exposed to Internet).
    -   Application Load Balancer (ALB): entry point for all APIs from Amplify to EC2.
    -   Gateway Endpoint (S3, DynamoDB) and Interface Endpoint (Textract, Bedrock, SQS – if used) for EC2 to call AWS services without NAT Gateway.
-   **Amazon CloudWatch + Amazon SNS:**
    -   Collects logs and metrics from EC2, ALB, SQS.
    -   CloudWatch Alarms send alerts via SNS when CPU is high, SQS backlog, worker errors, etc.
-   **AWS CodePipeline / CodeBuild:** Automates build & deploy for backend (FastAPI on EC2).

**Component Design**
-   **Users:**
    -   **Researchers:** Q&A, academic content lookup.
    -   **Dev/Admin:** Upload, manage, and re-index documents.
-   **Document Processing (IDP):**
    -   PDFs uploaded by Dev/Admin to S3.
    -   Worker on EC2 calls Textract for OCR and text/table extraction.
-   **Indexing & Vector DB:**
    -   Worker normalizes, chunks content.
    -   Calls Bedrock Titan Embeddings v2 to create embeddings.
    -   Saves embeddings + metadata to Qdrant on EC2.
-   **AI Conversation (RAG):**
    -   FastAPI embeds question, queries Qdrant for top-k relevant segments.
    -   Sends context + question to Claude 3.5 Sonnet (Bedrock) to generate answer with citation.
-   **User Management:**
    -   Cognito authenticates and authorizes researcher / admin.
-   **Storage & State:**
    -   DynamoDB stores document metadata (doc_id, status, owner, …) and (optional) chat history.

#### 4. Technical Implementation
**Implementation Phases**
The project consists of 2 main parts — web platform (UI + auth) and RAG + IDP backend — deployed across 4 phases:
-   **Research & Architecture Finalization:**
    -   Review requirements (50 researchers, 1 EC2, IDP + RAG).
    -   Finalize architecture: VPC, EC2 (FastAPI + Qdrant + Worker), Amplify, Cognito, S3, SQS, DynamoDB, Textract, Bedrock.
-   **POC & Connectivity Check:**
    -   Create EC2, VPC endpoints, test calling Textract, Titan Embeddings, Claude 3.5 Sonnet.
    -   Run simple Qdrant on EC2, test vector insert/search.
    -   Create skeleton FastAPI + a minimal Chat UI on Amplify.
-   **Feature Completion:**
    -   Build /api/chat (FastAPI) + RAG pipeline: embed query → Qdrant → Claude + citation.
    -   Build /api/admin/: upload PDF, save to S3 + DynamoDB, push message to SQS.
    -   Write Worker on EC2: SQS → Textract → normalize/chunk → Titan → Qdrant → update DynamoDB.
    -   Complete Chat UI and Admin UI (upload + view document status).
-   **Testing, Optimization, Internal Demo Deployment:**
    -   End-to-end test with a set of ~50–100 papers.
    -   Add CloudWatch Logs/Alarms, SNS notify on error or queue backlog.
    -   Adjust EC2, Qdrant configuration, batch size to optimize time and cost.
    -   Prepare user guide and demo for the group of 50 researchers.

**Technical Requirements**
-   **Frontend & Auth:**
    -   React/Next.js hosted on AWS Amplify, CloudFront CDN, Route 53 DNS.
    -   Amazon Cognito manages identity and permissions (Researcher/Admin).
-   **Backend & Compute:**
    -   EC2 t3.small (Private Subnet) running All-in-one: FastAPI, Qdrant Vector DB, and Worker.
    -   Asynchronous processing: Worker reads SQS, triggers Textract and Bedrock to index data.
-   **IDP & RAG:**
    -   **Storage:** S3 (Original files), DynamoDB (Metadata & Status).
    -   **AI Core:** Textract (OCR scanned docs), Bedrock Titan (Embedding), Claude 3.5 Sonnet (Question Answering).
-   **Network & Observability:**
    -   **Network:** VPC Private Subnet, VPC Endpoints for secure connection to AWS Services.
    -   **Monitoring:** CloudWatch Logs/Metrics + SNS alerts (High CPU, Worker errors).

#### 5. Timeline & Milestones
The project is executed over 3 months with specific phases:
-   **Month 1: Foundation & Preparation**
    -   Review core AWS knowledge (VPC, IAM, S3, DynamoDB, SQS, EC2).
    -   Deep dive into AI services: Amazon Bedrock, Textract.
-   **Month 2: Design & POC (Proof of Concept)**
    -   Finalize system architecture and RAG/IDP data flow.
    -   Setup basic infrastructure: VPC, EC2, Amplify, Cognito.
    -   Build basic RAG Chat feature (FastAPI + Qdrant + Claude) to validate solution.
-   **Month 3: Deployment, Testing & Operations**
    -   Complete automated document processing flow (Worker + Textract).
    -   Build full UI/UX for Admin and Researcher.
    -   Load test, optimize costs, and officially deploy for internal user group.

#### 6. Budget Estimation
You can view costs on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01)
Or download the [Budget Estimation File](../attachments/budget_estimation.pdf).

**Infrastructure Costs (Estimated Monthly)**
-   **Fixed Infrastructure (~$40–45):**
    -   **Compute & Network:** EC2 t3.small (~$15) + VPC Endpoints (~$20).
    -   **Storage & Web:** S3, DynamoDB, SQS, Amplify, CloudWatch (~$5–10).
-   **AI Costs (Variable):**
    -   **Amazon Textract:** ~$15–25 (batch processing first 10,000 pages).
    -   **Amazon Bedrock:** ~$5–15 (serving 50 users).
**Total:** **~$50–60/month** for internal research environment.

#### 7. Risk Assessment
**Risk Matrix**
-   **Hallucination (AI fabrication):** High impact, medium probability.
-   **Budget Overrun (AI Services):** Medium impact, medium probability.
-   **Infrastructure Failure (EC2/Qdrant):** High impact, low probability.

**Mitigation Strategies**
-   **AI Quality:** Mandatory source citations, limit input context from Qdrant.
-   **Cost:** Set up AWS Budgets/Alarms, control document ingestion volume.
-   **Infrastructure & Security:** Periodic EBS backups, data encryption (S3/DynamoDB), strict permissions via Cognito/IAM.

**Contingency Plans**
-   **System Failure:** Restore from Snapshot, pause ingestion (buffer via SQS).
-   **Cost Overrun:** Temporarily lock new uploads, limit daily query quotas.

#### 8. Expected Outcomes
**Technical Improvements**
-   Transform scattered document repositories (PDF/Scan) into digital knowledge queryable and automatically citable.
-   Significantly reduce manual search time thanks to RAG + IDP technology.

**Long-term Value**
-   Build a digitized research platform for 50+ researchers, easily scalable.
-   Create a foundation for advanced features: Document recommendations, research trend analysis, and Literature Review support.