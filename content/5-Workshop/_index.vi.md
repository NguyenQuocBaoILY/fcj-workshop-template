---
title: "Workshop"
date: "2025-09-09"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Xây dựng Nền tảng Quản lý Nhiệm vụ với DevOps trên AWS Serverless

#### Tổng quan

**AWS Serverless** cho phép xây dựng và triển khai ứng dụng mà không cần quản lý máy chủ, tự động mở rộng theo nhu cầu và chỉ trả phí cho những gì bạn sử dụng.

Trong workshop này, chúng ta sẽ học cách thiết kế, xây dựng và triển khai một nền tảng quản lý nhiệm vụ **TaskHub** hoàn chỉnh sử dụng kiến trúc serverless và quy trình DevSecOps tự động hóa.

Chúng ta sẽ tạo một hệ thống bao gồm frontend, backend API, database và CI/CD pipeline hoàn chỉnh. Workshop tập trung vào ba thành phần chính để xây dựng ứng dụng production-ready trên AWS:

+ **Serverless Backend** - Sử dụng AWS Lambda để xử lý nghiệp vụ, API Gateway làm cổng giao tiếp, DynamoDB lưu trữ dữ liệu, và Cognito quản lý xác thực người dùng với chi phí tối ưu.

+ **Content Delivery** - Triển khai ứng dụng Next.js trên S3, phân phối toàn cầu qua CloudFront với độ trễ thấp, và bảo vệ bằng AWS WAF chống các cuộc tấn công web phổ biến.

+ **DevOps Pipeline** - Tự động hóa quy trình build, test và deploy sử dụng CodePipeline và CodeBuild, tích hợp kiểm tra bảo mật với CodeGuru, và quản lý infrastructure as code với CloudFormation.

#### Nội dung

1. [Tổng quan về workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Triển khai hàm phi máy chủ với AWS Lambda](5.3-Lambda/)
4. [Xây dựng Cổng API với Amazon API Gateway](5.4-APIGateway/)
5. [Lưu trữ đối tượng đơn giản và bảo mật với Amazon S3](5.6-S3/)
6. [Tăng tốc độ phân phối nội dung với Amazon CloudFront (CDN)](5.7-CloudFront/)
7. [Quản lý danh tính và truy cập người dùng với Amazon Cognito](5.8-Cognito/)
8. [Quản lý khóa mã hóa với AWS Key Management Service (KMS)](5.9-KeyManagementService/)
9. [SrcetManager](5.17-SecretManager/)
10. [WAF](5.18-WAF/)
