---
title: "Giới thiệu"
date: "2025-09-09"
weight: 1
chapter: false
pre: "<b> 5.1. </b>"
---

## Xây dựng nền tảng TaskHub theo mô hình DevSecOps trên AWS 

#### Giới thiệu Workshop

Trong workshop này, bạn sẽ thực hành xây dựng toàn bộ nền tảng **TaskHub** theo mô hình **AWS Serverless + DevSecOps**, dựa trên kiến trúc thực tế được các doanh nghiệp áp dụng để triển khai hệ thống linh hoạt, bảo mật và tối ưu chi phí.

Workshop được phân chia theo từng nhóm dịch vụ AWS, giúp bạn:

+ Hiểu vai trò từng dịch vụ trong kiến trúc serverless hiện đại.  
+ Tự tay triển khai API Gateway, Lambda, DynamoDB, Cognito, S3/CloudFront.  
+ Tiếp cận DevSecOps chuyên nghiệp thông qua CodePipeline, CodeBuild và CodeGuru.  
+ Triển khai bảo mật ở cả Backend (KMS, Secrets Manager) và Edge (WAF, Shield).  
+ Kết nối hoàn chỉnh frontend Next.js với backend serverless AWS.

---

#### Tổng quan kiến trúc được triển khai

Trong workshop này, bạn sẽ thiết lập toàn bộ các thành phần chính của nền tảng TaskHub:

![Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram.png)


+ **Amazon S3** – Lưu trữ build tĩnh của ứng dụng Next.js.  
+ **Amazon CloudFront** – Phân phối giao diện toàn cầu với độ trễ thấp.  
+ **AWS WAF & AWS Shield** – Bảo vệ ứng dụng khỏi tấn công DDoS và OWASP Top 10.
+ **Amazon Cognito** – Xác thực, quản lý người dùng, phân quyền Admin/Member.
+ **Amazon API Gateway** – Cổng tiếp nhận request từ frontend.  
+ **AWS Lambda (Node.js/TypeScript)** – Xử lý toàn bộ logic nghiệp vụ.
+ **Amazon DynamoDB** – Lưu dữ liệu nhiệm vụ, người dùng, tiến độ.  
+ **AWS KMS** – Mã hóa dữ liệu tại DynamoDB.  
+ **AWS Secrets Manager** – Lưu trữ bí mật và khóa API.
+ **AWS CodePipeline** – Tự động hóa toàn bộ CI/CD.  
+ **AWS CodeBuild** – Build frontend/backend và chạy Security Scan.  
+ **AWS CodeGuru Reviewer** – Phân tích chất lượng mã và đề xuất tối ưu.  
+ **AWS CloudFormation** – Triển khai hạ tầng theo IaC.
+ **Amazon CloudWatch Logs** – Ghi nhận log từ Lambda và API Gateway.    
+ **AWS X-Ray** – Phân tích độ trễ và tracing toàn hệ thống.
+ **Amazon SNS** – Gửi thông báo sự kiện hoặc cảnh báo.