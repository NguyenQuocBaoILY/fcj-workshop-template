---
title: "Lambda"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---
# Khởi tạo Lambda

### Tạo Lambda Function

Chọn **Create function** → **Author from scratch**.
![lambda](/images/5-Workshop/5.3-Lambda/lambda1.png)

Trong phần **Basic information**, cấu hình:
- Function name: **taskhub-backend-1**
- Runtime: **.NET 8** (C#/F#/Powershell)
- Architecture: **arm64** (theo mặc định)

Phần **Permissions** → Change default execution role:
- Chọn **Create a new role with basic Lambda permissions** (AWS sẽ tự tạo role có quyền ghi log vào CloudWatch Logs)
![lambda2](/images/5-Workshop/5.3-Lambda/lambda2.png)

Giữ nguyên các cấu hình còn lại và nhấn **Create function** để hoàn tất.
![lambda2](/images/5-Workshop/5.3-Lambda/lambda3.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda4.png)   

### Build Backend & Đóng gói thành file ZIP

Do Lambda không cho sửa code trực tiếp với runtime .NET, bạn phải upload file zip.

- Tại máy local, build backend bằng lệnh:

```json
dotnet publish -c Release -o publish
```

- Mở thư mục publish 
   
-> Chọn toàn bộ file bên trong, không chọn cả folder.

- Nén lại thành file: **backend.zip**

### Upload mã nguồn lên Lambda

- Trong tab **Code** → chọn **Upload from** → **.zip file**.

![lambda2](/images/5-Workshop/5.3-Lambda/lambda5.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda7.png)

- Chọn file backend.zip.

- Nhấn Upload và chờ Lambda deploy.
  
![lambda2](/images/5-Workshop/5.3-Lambda/lambda8.png)

Sau khi upload xong: **Code properties** sẽ hiển thị package size, SHA256 hash, thời gian cập nhật.

![lambda2](/images/5-Workshop/5.3-Lambda/lambda10.png)

### Kiểm tra Runtime Settings

Trong phần Runtime settings:

- Runtime: **.NET 8**

- Handler: **YourProject::YourNamespace.YourHandler::FunctionHandler** (tùy structure của project)

- Architecture: **rm64**
  
Đảm bảo Lambda đang chạy đúng entry-point.

### Cấu hình Environment Variables

- Mở tab **Configuration** → **Environment variables**
  
![lambda2](/images/5-Workshop/5.3-Lambda/lambda11.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda12.png)

- Nhấn **Edit** → **Add environment variable**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda13.png)

![lambda2](/images/5-Workshop/5.3-Lambda/lambda14.png)

- Thêm các biến ví dụ:

 **ASPNETCORE_ENVIRONMENT**=**Production**

 **DynamoDB_TableName**= **Your_dynamoDB**

 **Jwt_Secret**= **Your_JWT_Secret**

Nhấn **Save**.

### Cấu hình Timeout & Memory

Mở tab Configuration → General configuration    

Nhấn Edit

![lambda2](/images/5-Workshop/5.3-Lambda/lambda15.png)

Đặt:

- Memory: **512MB – 1024MB**

- Timeout: **30s** (API) hoặc cao hơn nếu cần

![lambda2](/images/5-Workshop/5.3-Lambda/lambda16.png)

**Lưu lại**.

### Thêm Trigger (API Gateway)

Để biến Lambda thành API có URL:

- Tab Configuration → Triggers

- Nhấn Add trigger

![lambda2](/images/5-Workshop/5.3-Lambda/lambda17.png)  


Chọn:

- **API Gateway**

- **Create an API**

- **REST API**

Security: Open (hoặc IAM/Authorizer tùy hệ thống)

- Nhấn **Add**

![lambda2](/images/5-Workshop/5.3-Lambda/lambda18.png) 

![lambda2](/images/5-Workshop/5.3-Lambda/lambda19.png) 
