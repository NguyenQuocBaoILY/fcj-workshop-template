---
title: "Proposal"
date: "2025-12-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# TaskHub – A DevSecOps-Based Task and Progress Management Platform on AWS

### **1. Executive Summary**

TaskHub is a task and progress management platform designed to help small and medium-sized businesses as well as working teams manage workloads, deadlines, and progress in a visual and secure manner.

The system is developed following the **DevSecOps** model and is fully built on **AWS Serverless**, ensuring scalability, security, and cost optimization.

The development and deployment process adopts **AWS CodePipeline** and **CodeBuild** to automate CI/CD and security testing.

---

### **2. Problem Statement**

**Problems:**

- Small businesses and project teams often face difficulties in managing workload, tracking progress, and distributing tasks among members.

- Popular tools such as Jira or Asana usually come with high costs and lack seamless integration with DevSecOps processes or AWS environments.

**Solutions:**

- TaskHub leverages a Serverless AWS architecture to build a lightweight, secure, and cost-efficient platform.

- The platform is developed using **AWS Lambda**, **API Gateway**, **DynamoDB**, **Cognito**, and **S3/CloudFront**, while **AWS CodePipeline** is integrated for CI/CD and dynamic security testing.

**Benefits and Return on Investment (ROI)**

TaskHub delivers practical benefits for development teams and small-to-medium enterprises. The system functions as a centralized platform for task management, progress tracking, and role-based member management. The adoption of AWS Serverless architecture significantly reduces operational costs, optimizes resource utilization, and enhances scalability as usage demand grows. Furthermore, the platform serves as a hands-on DevSecOps practice environment, enabling research and development teams to expand future projects. According to estimates from the AWS Pricing Calculator, the system operation cost is approximately **0.66 USD per month**, equivalent to **7.92 USD per year**. Since the entire infrastructure is based on shared AWS services, no physical hardware investment is required. The expected break-even time is within **6–12 months**, thanks to substantial reductions in manual management effort and optimized internal workflows.

---

### **3. Solution Architecture**

TaskHub is built upon an AWS Serverless architecture, ensuring operational scalability, high performance, and cost efficiency. The platform focuses on real-time task, team, and project progress management while maintaining an automated DevSecOps deployment pipeline.

The overall architecture includes key components such as **Amazon API Gateway**, which acts as the request entry point and traffic distributor, **AWS Lambda** for backend business logic processing, and **Amazon DynamoDB** for storing task data, user information, and access control.

The web frontend is hosted on **Amazon S3** and globally distributed via **Amazon CloudFront**, while **AWS Cognito** handles authentication and authorization.

The CI/CD process is fully automated using **AWS CodePipeline** combined with **AWS CodeBuild**, enabling continuous deployment and security validation without server management.

The entire architecture is protected by **AWS WAF** and **AWS KMS** to enhance security and enforce DevSecOps compliance. **AWS X-Ray** is used for performance tracing and latency analysis. The complete architecture is illustrated in the diagram below:

![architecture](/images/2-Proposal/image1.png)

---

### **AWS Services Utilized**

1. **Amazon Route 53:** Highly reliable DNS service for traffic routing.

2. **AWS WAF (Web Application Firewall):** Advanced protection layer against common web attacks.

3. **Amazon CloudFront:** Global content delivery for the user interface and static assets.

4. **Amazon S3 (Simple Storage Service):** Static hosting of all frontend build files (Next.js).

5. **Amazon Cognito:** User authentication and authorization management.

6. **Amazon API Gateway:** Middleware layer for authentication and API routing to Lambda.

7. **AWS Lambda:** Core business logic processing and CloudWatch logging integration.

8. **Amazon DynamoDB:** High-performance NoSQL database encrypted using **AWS KMS**.

9. **AWS SNS (Simple Notification Service):** Asynchronous notification service.

10. **AWS Secrets Manager:** Secure storage, management, and rotation of sensitive credentials.

11. **AWS CodePipeline, CodeBuild & CodeGuru**

12. **CodePipeline/CodeBuild:** CI/CD automation; CodeBuild performs static security testing (SAST).

13. **AWS CodeGuru:** Automated source code analysis tool integrated into CI/CD to **provide intelligent recommendations for performance optimization and code quality improvement**, especially for Lambda environments.

14. **AWS CloudFormation:** **Infrastructure as Code (IaC)** service for automated resource provisioning.

15. **AWS CloudWatch Logs & AWS X-Ray:** **CloudWatch Logs** collects logs, **CloudWatch** sets alarms, and **X-Ray** provides distributed tracing and performance insights.

---

### **Component Design**

#### 1. **Frontend Layer:**

- **User Interface:** Built with **Next.js** as a Static Site.

- **Storage & Distribution:** Static files are securely hosted on **Amazon S3** and globally delivered via **Amazon CloudFront**, protected by **AWS WAF** at the Edge layer.

#### 2. **Backend Layer:**

- **API Gateway:** **Amazon API Gateway** receives all requests and verifies authentication using **Cognito Authorizer**.

- **Processing:** **AWS Lambda Functions** handle business logic (task CRUD, team management, role authorization).

- **Secrets Management:** Each Lambda function securely accesses sensitive credentials via **AWS Secrets Manager**, preventing secrets from being hardcoded.

#### 3. **Data Layer:**

- **Database:** **Amazon DynamoDB** stores task data, progress, and user configurations in **On-Demand** mode for auto-scaling and cost efficiency.

- **Data Security:** All at-rest data in DynamoDB is encrypted using **AWS KMS (Key Management Service)**.

#### 4. **Security & Authentication:**

- **Authentication:** **Amazon Cognito** manages login, sessions, and role-based access control (RBAC), supporting **Multi-Factor Authentication (MFA)** and **Single Sign-On (SSO)**.

- **Edge Protection:** **AWS WAF** is placed in front of CloudFront to mitigate Layer 7 DDoS attacks and OWASP Top 10 threats.

#### 5. **Deployment & Monitoring:**

- **DevSecOps CI/CD:** Source code is hosted on **GitLab** and automated by **AWS CodePipeline/CodeBuild**, including **CodeGuru** analysis before infrastructure deployment via **CloudFormation**.

- **Monitoring & Debugging:** **CloudWatch Logs** collects service logs; **CloudWatch** creates automated alarms; **AWS X-Ray** provides tracing and latency optimization.

---

### **4. Technical Implementation**

The TaskHub project is divided into two main areas—AWS Serverless infrastructure development and task management platform development—each consisting of the following phases:

---

### **Development Phases**

#### **Phase 1: Design & Modeling (Month 1)**

- **Key Actions:** Research on Serverless/DevSecOps, selection of core services (Lambda, DynamoDB, API Gateway), detailed architecture design, and NoSQL data modeling.

- **Deliverables:** Architecture Diagram and Data Model Documentation.

#### **Phase 2: Infrastructure as Code Initialization (Month 2)**

- **Key Actions:** Detailed cost estimation; AWS CDK development for foundational services (S3, CloudFront, Cognito).

- **Deliverables:** Base AWS CDK source code and Cost Estimation Report.

#### **Phase 3: DevSecOps Automation Setup (Months 2–3)**

- **Key Actions:** Full CI/CD pipeline setup (CodePipeline/CodeBuild), integration of AWS CodeGuru and SAST for automated code quality and security scanning.

- **Deliverables:** Operational CodePipeline and automated security scanning workflow.

#### **Phase 4: Development & Deployment (Months 3–4)**

- **Key Actions:** Feature development (Lambda with TypeScript, frontend with Next.js), integration testing, and production deployment via Pipeline.

- **Deliverables:** TaskHub Beta Version (Full CRUD) and Testing Report.

---

### **Technical Requirements**

- **Architecture & Tools:** Entire system is managed using AWS CDK for infrastructure consistency.

- **Technology Stack:** Backend uses TypeScript/Node.js; frontend uses Next.js (React).

- **Source Code Management:** GitLab with automated deployment via AWS CodePipeline.

- **Monitoring:** CloudWatch, X-Ray, and CloudWatch Logs for deep performance monitoring and debugging.

- **Non-functional Requirements:** Deployed in Singapore region (**ap-southeast-1**) for optimal Vietnam latency, scalable to **50 users**, and encrypted using **AWS KMS**.

---

### **5. Timeline & Key Milestones**

#### **Project Timeline**

- **Pre-Internship (Month 0):** Planning, DevSecOps research, and AWS Serverless service study.

- **Month 1:** Development environment setup, AWS infrastructure initiation, and CI/CD pipeline initialization.

- **Month 2:** Architecture design, core feature implementation, and automated security testing.

- **Month 3:** Frontend-backend integration, beta development, and platform release.

- **Post-Release:** Maintenance, performance evaluation, and feature expansion.

---

### **6. Budget Estimation**

#### **Hourly Rates**

| Resource | Responsibility | Rate (USD)/Hour |
|----------|----------------|------------------|
| Solution Architect [1] | System & API design, Database design, Technical leadership | 6 |
| Engineers [3] | Backend, Frontend, Security implementation | 4 |
| Others (DevOps) [1] | CI/CD, Cloud deployment, Monitoring, Security | 4 |

#### **Project Phases Cost Breakdown**

| Project Phase | Architect | Engineers | Others | Total Hours |
|---------------|-----------|-----------|--------|--------------|
| System & Architecture Design | 20 | 10 | 0 | 30 |
| Backend Development | 10 | 80 | 0 | 90 |
| Frontend Development | 5 | 60 | 0 | 65 |
| Security & CI/CD Setup | 5 | 30 | 10 | 45 |
| Testing & Deployment | 5 | 30 | 0 | 35 |
| **Total Hours** | **45** | **210** | **10** | **265** |
| **Total Cost (USD)** | **270** | **840** | **40** | **800** |

#### **Cost Contribution Distribution**

| Party | Contribution (USD) | Percentage |
|-------|---------------------|------------|
| Customer | 0 | 0% |
| Partner | 0 | 0% |
| AWS | 800 | 100% |

---

### **7. Risk Assessment**

#### **Risk Matrix**

- AWS service disruption: Medium impact – Medium probability  
- CI/CD misconfiguration: High impact – Low probability  
- AWS budget overrun: Medium impact – Low probability  
- Security vulnerabilities: High impact – Medium probability  
- Performance degradation under load: Medium impact – Medium probability  

#### **Mitigation Strategies**

- Multi-region monitoring using CloudWatch and X-Ray  

- Code review and validation before deployment via **CodePipeline**

- Cost monitoring using AWS Budgets  

- Automated security scanning via **CodeBuild** (replacing GitHub Actions)

#### **Contingency Plan**

- Maintain staging environments for rapid recovery

- Use CloudFormation and AWS Backup for configuration and data backup

---

### **8. Expected Outcomes**

#### **Technical Improvements**

- **Full Automation:** A complete transition to automated **DevSecOps**, achieving deployment times under **6 minutes**.

- **Guaranteed Performance:** API response time under **150ms** and system availability at **99.9% uptime**.

- **Integrated Security:** Automated detection and remediation of critical security issues during the build phase.

- **Scalability Ready:** The platform is capable of scaling to support high traffic and user growth without architectural changes.

#### **Long-Term Value**

- **Reusable Technical Assets:** A complete set of **AWS CDK/CloudFormation** templates optimized for cost and scalability, reusable for future projects.

- **Solid Platform Foundation:** An industry-standard DevSecOps-based task management environment ready for long-term feature expansion.

