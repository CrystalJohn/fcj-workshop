---
title: "Event 4"
date: "15/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #1 workshop"

### Event Objectives

- Introduce the AI/ML/GenAI ecosystem on AWS.
- Guide end-to-end ML deployment using SageMaker.
- Build Generative AI applications through Amazon Bedrock, RAG, Agents.
- Get familiar with MLOps, IaC, CICD, container workflows for AI/ML.
- Develop operational thinking for AI in enterprise environments.

### Speakers

- **AWS AI/ML Specialist Team** – AWS Vietnam
- **AWS Solutions Architect**
- **AI/ML Community Leader in Vietnam**

### Key Highlights

#### AWS AI/ML Services Overview

- **Amazon SageMaker** — end-to-end ML platform
    - Data preparation: **SageMaker Data Wrangler**, **Ground Truth** (data labeling), **Feature Store** (reusable feature management)

- **Training & Tuning**
    - Training jobs → autoscaling GPU/CPU
    - Supports distributed training
    - Hyperparameter tuning (Auto-tuning, Bayesian Optimization)

- **Deployment Options**
    - Real-time endpoint
    - Serverless inference
    - Multi-model endpoint
    - Asynchronous (async) inference

- **MLOps**
    - Model registry
    - CI/CD pipelines using **SageMaker Pipelines**
    - Monitoring drift: data drift, feature drift, model drift

- **Live demo**
    - Experience **SageMaker Studio**: notebook, data processing, running training and deploying models.

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
    - Chunking inputs appropriately
    - Metadata filtering
    - Re-ranking
    - Apply guardrails to ensure safety and control

#### Bedrock Agents

- Agents can autonomously execute multi-step workflows.
- Trigger `Lambda` to call APIs, databases, and external workflows.
- Can replace backend logic in many use cases (business logic orchestration).
- Integrate closely with `EventBridge` and `Step Functions` in AI pipelines for orchestration and retry.

#### CICD Workflow for Containers (ECR + ECS)

1. Developer commit → `CodeCommit`
2. `CodeBuild` builds the image
3. Push image to `ECR`
4. `ECS` pulls image to deploy (Fargate / EC2)
5. `CloudWatch` monitors logs & metrics
6. `CodePipeline` orchestrates the entire process

Add security (DevSecOps):

- `CodeBuild` validation (unit tests, static analysis)
- Image scanning in `ECR` (vulnerability scan)
- Deployment policies / IAM controls to manage deployment

→ This is AWS-standard DevSecOps.  

### Key Takeaways

#### AI/ML and GenAI Fundamentals

- ML lifecycle
- Feature engineering
- Model deployment & monitoring
- Cost optimization for training/inference
- How to choose the right Foundation Model for use cases
- Advanced prompt engineering
- RAG architecture for production (retrieval-augmented generation)
- Build Agents to execute workflows automatically

#### Critical Technical Architecture

- **IaC (Infrastructure as Code):** `Terraform` vs `CloudFormation` vs `CDK`
- **Container inference workflow:** `ECS` / `ECR` for AI inference containers
- **Monitoring & Observability:** monitoring AI workloads with `CloudWatch` + `X-Ray`  

### Applying to Work

- Build enterprise chatbot with `Bedrock` + RAG (retrieval-augmented generation)
- Design end-to-end AI architecture (data → ML → app)
- Use `Lambda` + `Step Functions` to orchestrate RAG pipelines
- Use `Terraform` / `CDK` to build GenAI infrastructure
- Deploy inference containers via `ECS` / `Fargate`  

### Event Experience

- Attending the **"AWS Cloud Mastery Series #1"** workshop, I clearly saw how AWS deploys real-world AI/ML, understood the differences between traditional ML and GenAI, and enhanced my AI architecture design thinking.

#### Learning from highly skilled speakers

- Understood in detail how AWS builds a comprehensive AI/ML/GenAI ecosystem.

#### Leveraging modern tools

- **SageMaker** — `SageMaker` helps with end-to-end ML: data preparation, training, tuning, deployment, and MLOps.
- **Bedrock** — provides Foundation Models (`Claude`, `Llama`, `Titan`) and supports RAG + Agents to quickly build chatbots and AI workflows.
- **IaC & DevOps** — `CloudFormation`, `CDK`, `Terraform`, `CodePipeline` enable controlled and automated AI/ML deployment.
- **Data & ETL Tools** — `Glue`, `S3` (Data Lake), `EventBridge`, `Step Functions` support data pipelines for AI.
- **Monitoring** — `CloudWatch` for AI/ML monitoring, logging, and drift detection.

#### Networking and thinking

- Learned to think about AI systems instead of just isolated ML models.
- Grasped AI/ML trends in Vietnam and how enterprises apply them.

#### Lessons learned

- GenAI and traditional ML combine to create powerful AI solutions in practice.
- `Bedrock` is an enterprise GenAI platform: secure, fast to deploy, no need to train models yourself.
- `MLOps` + `IaC` are essential for stable AI in production.
- `RAG` is the most effective way to "bring enterprise knowledge into models."

#### Some event photos
![Event](/images/4-EventParticipated/15.11-event.jpg)
> Overall, the event not only provided technical knowledge but also gave me a comprehensive AI/ML system design mindset, from data to deployment and operations, making me more confident in building practical AI solutions for enterprises.
