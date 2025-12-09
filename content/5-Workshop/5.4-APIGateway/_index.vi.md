---
title: "API Gateway"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---
# Khởi tạo API Gateway

### Tạo API Gateway Function

Mở API Gateway

- Chọn **Create API**

![lambda](/images/5-Workshop/5.4-APIGateway/api1.png)

Chọn loại API: **REST API**

Chọn tiếp: **Build**

![lambda](/images/5-Workshop/5.4-APIGateway/api2.png)

### Cấu hình thông tin API

Nhập tên API, ví dụ: **taskhub-backend-api**

Endpoint type: **Regional**

Security policy: SecurityPolicy_TLS13_1_3_2025_09

Nhấn **Create API**

![lambda](/images/5-Workshop/5.4-APIGateway/api3.png)

![lambda](/images/5-Workshop/5.4-APIGateway/api4.png)

### Tạo Resource cho API

Trong menu API Gateway, chọn: **Resources**

Nhấn **Create resource**

![lambda](/images/5-Workshop/5.4-APIGateway/api5.png)

Nhập:

- Resource name: auth, task, projects … tùy proeject

- Resource path: /auth, /task, …

![lambda](/images/5-Workshop/5.4-APIGateway/api6.png)

Nhấn **Create resource**

### Tạo Method và kết nối Lambda

![lambda](/images/5-Workshop/5.4-APIGateway/api7.png)

Chọn **Resource** → nhấn ****Create method**

- Chọn: **ANY** (hoặc POST, GET tùy API)

- Integration type: **Lambda Function**

- **Tick Use Lambda Proxy integration**

- Chọn **Region**

- Nhập tên function Lambda, ví dụ: **taskhub-backend_1**

![lambda](/images/5-Workshop/5.4-APIGateway/api8.png)

Nhấn **Save**

![lambda](/images/5-Workshop/5.4-APIGateway/api90.png)

Làm tương tự để tạo API còn lại

### Cấp quyền cho API Gateway gọi Lambda

Chọn ***Deploy API**

![lambda](/images/5-Workshop/5.4-APIGateway/api10.png)

- Tạo stage mới (nếu chưa có): **prod1**

- Nhấn **Deploy**

![lambda](/images/5-Workshop/5.4-APIGateway/api11.png)

Kết quả: Sẽ nhận được Invoke URL dạng: https://ne6pw5hqej.execute-api.ap-southeast-1.amazonaws.com/prod1

![lambda](/images/5-Workshop/5.4-APIGateway/api12.png)