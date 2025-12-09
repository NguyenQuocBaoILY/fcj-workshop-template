---
title: "Cấu hình AWS Secrets Manager"
date: "2025-11-14"
weight: 17
chapter: false
pre: " <b> 5.17. </b> "
---

## Mục tiêu

Sử dụng **AWS Secrets Manager** để lưu trữ cấu hình/secret cho hệ thống **TaskHub** với các yêu cầu:
- Secret được lưu dưới dạng **JSON (Key/value pairs – Plaintext)**
- Dữ liệu được **mã hóa bằng KMS CMK: `taskhub_kms`**
- (Tùy chọn) Bật **tự động xoay vòng secret (rotation)** bằng **AWS Lambda**

---

## Bước 1 – Truy cập AWS Secrets Manager

1. Tại **AWS Console**, nhập vào ô tìm kiếm: `Secrets Manager`.
2. Chọn dịch vụ **Secrets Manager** trong danh sách **Services**.

![1](/images/5-Workshop/5.17-SecretManager/17.1.png)
---

## Bước 2 – Tạo secret mới (Key/value pairs – JSON)

1. Tại trang chính của Secrets Manager, click **Store a new secret**.
   ![1](/images/5-Workshop/5.17-SecretManager/17.2.png)

2. Ở mục **Secret type**, chọn: **Other type of secret**.
3. Tại khu vực **Key/value pairs**:
   - Chuyển từ tab **Key/value** sang tab **Plaintext**.
   - Dán nội dung JSON sau (demo cho DynamoDB + KMS):

   ~~~json
   {
     "Service": "Amazon DynamoDB",
     "Table": "TaskHub Tables",
     "Encryption": "SSE-KMS",
     "KMSKeyAlias": "taskhub_kms",
     "Purpose": "Store users, projects, tasks",
     "DataProtection": "Encrypted at rest"
   }
   ~~~

4. Tại mục **Encryption key**, chọn khóa KMS:
   - **`taskhub_kms`**
5. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.3.png)


---

## Bước 3 – Cấu hình tên secret và thông tin cơ bản

1. Tại bước **Configure secret**:

   - **Secret name**:  
     Ví dụ:  
     `prod/taskhub/secretmanager`

   - **Description (không bắt buộc)**:  
     `Metadata for TaskHub DynamoDB encryption demo`


2. **Tags (optional)**: bỏ qua cho workshop.
3. **Resource permissions (optional)**: giữ mặc định (phân quyền bằng IAM).
4. **Replicate secret (optional)**: không bật trong workshop.
5. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.4.png)

---

## Bước 4 – Cấu hình tự động xoay secret (Rotation – tùy chọn)

> Trong môi trường production, secret thường được xoay vòng theo chu kỳ 30 ngày.  
> Trong workshop, ta cấu hình thời gian ngắn để demo.

1. Tại bước **Configure rotation – optional**, bật:
   - **Automatic rotation**
2. Trong **Rotation schedule**:
   - Chọn **Schedule expression builder**
   - **Time unit**: `Hours`
   - **Hours**: `23`
   - (Tùy chọn) **Window duration**: `4h`
   - Giữ tick **Rotate immediately when the secret is stored**
3. Tại **Rotation function**:
   - Chọn Lambda function: **`taskhub-backend`**
4. Click **Next**.

![1](/images/5-Workshop/5.17-SecretManager/17.5.png)
---

## Bước 5 – Kiểm tra & lưu secret

1. Tại bước **Review**, kiểm tra lại các thông tin:

   - Secret type: **Other type of secret**
   - Encryption key: **`taskhub_kms`**
   - Secret name: `prod/taskhub/metadata`
   - Automatic rotation: **Enabled**
   - Lambda rotation function: **`taskhub-backend`**

2. Kéo xuống phần **Sample code**:
   - AWS sẽ hiển thị sẵn hàm mẫu `getSecret()` cho các ngôn ngữ như:
     - Java
     - JavaScript
     - Python
     - C#
     - Go
   - Backend của TaskHub sẽ sử dụng SDK tương ứng để gọi Secrets Manager thay vì hard-code cấu hình trong source code.

3. Click **Store** để hoàn tất.

![1](/images/5-Workshop/5.17-SecretManager/17.6.png)

---

## Kết quả đạt được

- Secret mới đã được tạo trong **AWS Secrets Manager**.
- Nội dung secret được lưu dưới dạng **JSON**.
- Secret được:
  -  **Mã hóa at-rest bằng KMS (`taskhub_kms`)**
  -  **Có thể tự động xoay vòng bằng Lambda**
- Backend của **TaskHub** có thể truy xuất secret thông qua:
  - **AWS SDK**
  - **IAM Role / Policy**
