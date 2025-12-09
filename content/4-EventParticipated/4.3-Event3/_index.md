---
title: "Event 3"
date: "17/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #2 workshop"

### Event Objectives

- Equip modern DevOps mindset and how to apply it on AWS ecosystem.
- Guide deployment of CI/CD, IaC, container orchestration, and monitoring according to AWS standards.
- Optimize development speed, release quality, and system reliability.

### Speakers

- **FCJ Team** 
- **Kha Van** - Cloud Security Engineer
- **AWS Developer Advocate Team**

### Key Highlights

#### AWS DevOps Services – CI/CD Pipeline

- **Build**: optimize `buildspec.yml` (build caching & parallel test).
- **Demo**: end-to-end pipeline — Commit → Build → Test → Deploy → Monitor.
- **Deployment**: must be 100% automated — no click-ops (no manual deployments).

#### Infrastructure as Code

- **Tools:** `CloudFormation` / `Terraform` / `CDK` — used to deploy infrastructure (CloudFormation is AWS managed service).
- **Template anatomy (YAML):** `Resources` → `Parameters` → `Outputs` → `Mappings`.
- **CDK model:** Constructs → Stacks → Apps. Supports multiple languages (TypeScript, Python, Java...).
- **Benefits:** easy to reuse, modularize, and test.
- **When to choose:**
    - **CloudFormation:** declarative, suitable for enterprise teams.
    - **CDK:** developer-friendly, suitable for Agile teams.

> "No IaC = No DevOps."

#### Container Services on AWS

- **Docker fundamentals**
    - `Dockerfile` → Build → Image → Registry → Run container
    - Registry: Docker Hub or **Amazon ECR**
- **Amazon ECR**
    - Image scanning, lifecycle policies, permissions per repository
- **Amazon ECS**
    - Run containers with **Fargate** or **EC2**
    - Application Load Balancer, auto-scaling by CPU, memory, queue length
- **Amazon EKS**
    - Managed Kubernetes service
    - Use cases: large-scale, multi-team, portable workloads
- **AWS App Runner**
    - For teams who want to "deploy containers like deploying to Vercel"

> Case study: Compare `ECS`, `EKS`, `App Runner` for microservices. "Containers = scalability + portability. ECS/EKS help production run smoothly."

#### Monitoring & Observability

- **CloudWatch**
    - Metrics, Logs, Dashboards
    - Composite alarms
    - Custom metrics for business KPIs

- **AWS X-Ray**
    - Distributed tracing
    - Debug latency, bottlenecks, service maps

- **Best practices**
    - Design alerting to avoid noise
    - Clear on-call workflow and runbooks
    - Monitor using Golden Signals: Latency, Traffic, Errors, Saturation

> "No observability = not knowing how your system is dying."  

### Key Takeaways

#### Modern DevOps Mindset

- Focus on **outcome**, not tools.
- **DORA metrics**: standard for measuring software performance.
- Continuous Integration → Continuous Delivery → Continuous Learning.

#### Critical Technical Architecture

- **CI/CD (AWS)**: CodePipeline, CodeBuild, CodeDeploy — design pipelines according to standards.
- **IaC** is the backbone of automation (CloudFormation / Terraform / CDK).
- **Containerization** helps microservices scale and be portable.
- **Observability**: detect errors quickly, reduce MTTR with metrics, logs, and tracing.

#### DevOps Strategy

- Start from small pipeline → expand gradually.
- Use **blue/green** and **canary** to reduce deployment risk.
- Apply **IaC + GitOps** for multi-team environments.  

### Applying to Work

- Build CI/CD for backend/web projects (CodePipeline / CodeBuild / CodeDeploy).
- Package services with Docker and deploy to `ECS` (Fargate) or `App Runner`.
- Use `CDK` to build AWS infrastructure instead of manual console operations.
- Monitor systems with CloudWatch (dashboards, alarms, custom metrics).
- Set up incident workflow for projects: alert → investigate → fix → postmortem (runbooks & on-call).  

### Event Experience

- Attending the **"AWS Cloud Mastery Series #2"** workshop was an extremely valuable experience, helping me develop the mindset of a DevOps engineer.

#### Learning from highly skilled speakers

- Understood in detail how AWS handles incidents & detection in practice.

#### Hands-on technical experience

- Demo of pipeline from commit → live deployment.
- Demo of drift detection, CDK synth/deploy.
- Real-time container deployment on ECS/ECR.

#### Leveraging modern tools

- CloudFormation + CDK make IaC clear and repeatable.
- ECR scanning enhances security.
- X-Ray makes debugging microservices easy.

#### Networking and thinking

- Understood clearly how Dev/Product/DevOps teams collaborate in CI/CD pipelines.
- "Automate everything" mindset became more ingrained.

#### Lessons learned

- No CI/CD → No DevOps.
- IaC is a prerequisite.
- Containers → scalability + speed.
- Observability → reliability.

#### Some event photos
![Event](/images/4-EventParticipated/29.11-event.jpg)
> Overall, the event not only provided technical knowledge but also helped me change my DevOps thinking, automate software development processes, and improve team collaboration.
