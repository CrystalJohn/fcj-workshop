---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# AI-POWERED ACADEMIC RESEARCH CHATBOT ON AWS

#### Overview

In this workshop, we will build **ARC (Academic Research Chatbot)** - an intelligent chatbot system running on the **AWS Serverless** platform. This solution leverages **Generative AI** and **RAG (Retrieval-Augmented Generation)** to support academic research, document queries, and answer questions flexibly.

Instead of answering questions based on fixed scripts (rule-based), the system uses the **Claude 3.5 Sonnet** model to understand natural language, query data from the vector database, and respond to users accurately.

#### Workshop Objectives

After completing this workshop, you will:

- Understand RAG architecture and how to apply it in practice  
- Deploy a complete chatbot system on AWS  
- Use Amazon Bedrock (Claude 3.5 Sonnet + Cohere Embed)  
- Build an IDP pipeline with Amazon Textract  
- Implement vector search with Qdrant  
- Deploy infrastructure with Terraform  
- Integrate authentication with Amazon Cognito  

#### Content

1. [Workshop Overview](5.1-workshop-overview)
2. [Prerequisites](5.2-prerequiste)
3. [Create S3 VPC Endpoint](5.3-s3-vpc)
4. [Create S3 On-Prem Access](5.4-s3-onprem)
5. [Configure Policy](5.5-policy)
6. [Clean up Resources](5.6-cleanup)