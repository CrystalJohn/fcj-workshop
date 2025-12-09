---
title: "Event 2"
date: "29/11/2025"
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AWS Cloud Mastery Series #3 workshop"

### Event Objectives

- Share and reinforce core knowledge in AWS Well-Architected Framework – Security Pillar.
- Equip mindset and skills to build secure, sustainable systems that comply with AWS standards.

### Speakers

- **FCJ Team** 
- **Kha Van** - Cloud Security Engineer

### Key Highlights

#### Security Foundation – AWS Security Foundation

- **Least Privilege – Zero Trust – Defense in Depth**: 3 foundational principles for all modern security systems.
- **Shared Responsibility Model**: AWS secures the cloud, customers secure in the cloud.
- **Top Cloud Threats in Vietnam**
    - Public S3 / DB / Redis
    - Exposed access keys
    - IAM misconfiguration
    - EC2 infected with malware (mining)
    - Missing logging or GuardDuty not enabled

#### Pillar 1 — Identity & Access Management (IAM)

- **IAM Users**: almost no longer suitable — prioritize using **Roles + SSO + OIDC**.
- **IAM Identity Center (SSO)**: used for multi-account management.
- **Organizational permissions**: apply **SCP** and **permission boundaries** to control permissions at the organizational level.
- **Authentication security & monitoring**: mandatory **credential rotation**, **MFA**; use **Access Analyzer** to detect overly broad policies.

#### Pillar 2 — Detection & Monitoring

- **CloudTrail (organization level)**: stores all API activity for audit and incident investigation.
- **GuardDuty**: detects anomalous behavior on EC2, IAM, S3, EKS.
- **Security Hub**: aggregates findings from multiple services into a central dashboard.
- **Logging sources**: VPC Flow Logs, S3 access logs, ALB logs — used to trace network & access patterns.
- **Alerting & Automation**: use EventBridge to send alerts and trigger automatic remediation.
- **Detection-as-Code**: manage detection rules as source code (IaC) for easier review and deployment.

> Case study: Without logs, there's no evidence — incident investigation becomes impossible.

#### Pillar 3 — Infrastructure Protection

- **VPC segmentation**: reduce "blast radius" when systems are attacked by isolating network zones.
- **Subnet placement (Public vs Private)**:
    - **Public**: ALB, NAT Gateway.
    - **Private**: EC2 (app), Databases, internal services.
- **Security Group (SG) vs Network ACL (NACL)**:
    - **SG** = stateful, applied at instance level.
    - **NACL** = stateless, applied at subnet level.
- **Edge protection**: WAF, Shield Advanced, Network Firewall for edge defense layers.
- **Workload hardening**: best practices for EC2, ECS, EKS security (patching, minimal IAM, image scanning, runtime protections).

> Case study: Most security breaches stem from placing resources in the wrong `public subnet` — always check placement before deployment.

#### Pillar 4 — Data Protection

- **Encryption everywhere**: enable encryption for `S3`, `EBS`, `RDS`, `DynamoDB` (at-rest).
- **AWS KMS**:
    - Manage `Key policies`, `Grants`, and `Rotation` for key lifecycle.
- **Encryption layers**: separate `encryption at-rest` and `in-transit` (TLS/HTTPS).
- **Secrets management**: use **Secrets Manager** / **Parameter Store**:
    - Automatic secret rotation.
    - Manage credentials according to AWS best practices.
- **Data classification & guardrails**: classify data by sensitivity level and apply guardrails (IAM, encryption, access controls).

> Case study: If you encrypt from the start, data breach risk can be reduced by ~50%.

#### Pillar 5 — Incident Response (IR)

- **AWS IR lifecycle**: Prepare → Detect → Investigate → Respond → Recover.

- **Playbooks (real examples):**
    - Compromised IAM key
    - Public S3 bucket
    - EC2 infected with malware / mining

- **Tools & handling techniques:**
    - **Snapshot** (EBS/instance snapshot) to preserve evidence.
    - **Isolation** by changing Security Group to disconnect instance.
    - **Evidence collection** for forensic and audit.

- **Incident Response automation:**
    - **Lambda** – run small remediation, cleanup.
    - **Step Functions** – orchestrate complex IR workflows.
    - **EventBridge** – trigger playbooks when events are detected.

> Case study: Incident response cannot rely entirely on humans — automation is needed as much as possible.  

### Key Takeaways

#### Modern Security Mindset

- **Zero Trust & Least Privilege**: prioritize zero-trust model by default and restrict permissions to the minimum level.
- **Defense in Depth**: build multiple defense layers to reduce risk when one layer is breached.
- **Traceability & IaC**: all changes must be traceable and reproducible — deploy infrastructure with **Infrastructure as Code (IaC)**.
- **Don't trust manual configuration**: avoid manual configuration, prioritize coding and testing.

#### Critical Technical Architecture

- **IAM**
    - **Roles > Users**
    - **SSO > local credentials**
    - **OIDC > Access Keys**

- **Network**
    - **Private-first**: always prioritize placing resources in private placement.
    - **Security Group (SG)** is the "main wall" (stateful); **NACL** is supplementary layer (stateless).
    - **Outbound filtering** is equally important as inbound filtering.

- **Data**
    - **Encrypt by default** (S3, EBS, RDS, DynamoDB).
    - **Secret rotation** according to policy.
    - **Zero exposure** for S3/DB — restrict public access.

- **Detection**
    - **CloudTrail ON**
    - **GuardDuty ON**
    - **Comprehensive logging** to support incident investigation.

- **Incident Response (IR)**
    - Clear **playbook**.
    - **Automation** (remediation, rollback).
    - **Rollback / snapshot** for quick recovery.

#### Modernization Strategy

- No rush → Phased approach.
- Use multi-account architecture to reduce risk.  

### Applying to Work

- Design project IAM with Roles + Permission Sets.
- Apply VPC segmentation and "private-first design".
- Add GuardDuty + CloudTrail org-level for entire dev/prod environment.
- Set up Alerting & IR automation (EventBridge → Lambda).
- Apply Secret Manager + rotation → no hard-coded secrets.
- Deploy IaC (Terraform/CDK) to reduce drift.  

### Event Experience

- Attending the **"AWS Cloud Mastery Series #3"** workshop was an extremely valuable experience, giving me a comprehensive view of how to build secure and sustainable systems using modern methods and tools. Some highlights:

#### Learning from highly skilled speakers

- Understood in detail how AWS handles real-world incident & detection.  

#### Hands-on technical experience

- Learned how to analyze policies using IAM Simulator.
- Witnessed the actual flow of S3 public exposure → auto-remediation.
- Observed IR automation handling compromised EC2.  

#### Leveraging modern tools

- Clearly saw the role of security automation & multi-layer defense.
- Learned OIDC, cross-account guardrails, detection as code.  

#### Networking and thinking

- Realized how large enterprises organize security with multi-account.
- "Secure by design" mindset, not waiting until attacked to fix.  

#### Lessons learned

- Security = culture, not a feature.
- Misconfiguration is the biggest cause → IaC is the solution.
- Cannot operate cloud securely without IAM + Logging + IR  

#### Some event photos
![Event](/images/4-EventParticipated/29.11(1).jpg)
> Overall, the event not only provided technical knowledge but also helped me deepen my understanding of modern security mindset, automate security processes, and improve incident response capabilities in cloud environments.
