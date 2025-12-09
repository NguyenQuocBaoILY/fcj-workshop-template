---
title: "Introduction"
date: "2025-09-09"
weight: 1
chapter: false
pre: "<b> 5.1. </b>"
---

## Building the TaskHub Platform with the DevSecOps Model on AWS

#### Workshop Introduction

In this workshop, you will build the entire **TaskHub** platform following the **AWS Serverless + DevSecOps** model, based on real-world architectures widely adopted by modern enterprises to achieve scalability, security, and cost efficiency.

The workshop is divided into groups of AWS services, helping you:

+ Understand the role of each service in a modern serverless architecture.  
+ Deploy API Gateway, Lambda, DynamoDB, Cognito, and S3/CloudFront hands-on.  
+ Apply DevSecOps practices using CodePipeline, CodeBuild, and CodeGuru.  
+ Implement security at both the Backend (KMS, Secrets Manager) and Edge (WAF, Shield).  
+ Fully integrate a Next.js frontend with an AWS serverless backend.


#### Architectural Overview

In this workshop, you will build all the key components of the TaskHub platform:

![Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram.png)

+ **Amazon S3** – Stores the static build of the Next.js application.  
+ **Amazon CloudFront** – Delivers the UI globally with low latency.  
+ **AWS WAF & AWS Shield** – Protect the application from DDoS attacks and OWASP Top 10 threats.
+ **Amazon Cognito** – Handles user authentication, identity management, and Admin/Member role authorization.
+ **Amazon API Gateway** – Serves as the entry point for frontend requests.  
+ **AWS Lambda (Node.js/TypeScript)** – Processes all business logic.
+ **Amazon DynamoDB** – Stores tasks, users, and progress data.  
+ **AWS KMS** – Encrypts data stored in DynamoDB.  
+ **AWS Secrets Manager** – Stores sensitive information and API keys securely.
+ **AWS CodePipeline** – Automates the entire CI/CD process.  
+ **AWS CodeBuild** – Builds frontend/backend and performs security scans.  
+ **AWS CodeGuru Reviewer** – Analyzes code quality and provides optimization recommendations.  
+ **AWS CloudFormation** – Deploys infrastructure via IaC.
+ **Amazon CloudWatch Logs** – Captures logs from Lambda and API Gateway.  
+ **AWS X-Ray** – Provides system-wide tracing and latency analysis.
+ **Amazon SNS** – Sends system events and alert notifications.
