---
title: "Tạo Cognito User Pool"
date: "2025-09-09"
weight: 1
chapter: false
pre: " <b> 5.8.1. </b> "
---

#### Tạo User Pool

Trong bước này, bạn sẽ tạo một Cognito User Pool để quản lý xác thực người dùng cho ứng dụng của mình.

1. Điều hướng đến dịch vụ **AWS Cognito** trong AWS Console
2. Nhấp **Create user pool**

![Create User Pool](/images/5-Workshop/5.8-Cognito/create-user-pool-1.png)

3. **Cấu hình tùy chọn (Configure options)**
   
   **Options for sign-in identifiers:**
   - Chọn **Email** (cho phép người dùng đăng nhập bằng email)
   - Bỏ chọn **Phone number** và **Username**
   
   **Self-registration:**
   - ✅ **Enable self-registration** (cho phép người dùng tự đăng ký)
   
   **Required attributes for sign-up:**
   - Chọn **email** và **name** làm thuộc tính bắt buộc
   
   ![Configure Options](/images/5-Workshop/5.8-Cognito/configure-options.png)
   
   - Nhấp **Create user pool**

4. **Nhập tên User Pool**
   - User pool name: **TaskManagementUserPool**
   - Nhấp **Create user pool**

![User Pool Name](/images/5-Workshop/5.8-Cognito/user-pool-name.png)

#### Xác minh việc tạo User Pool

Sau khi tạo thành công, bạn sẽ thấy User Pool của mình trong Cognito console với cấu hình mặc định:

![User Pool Created](/images/5-Workshop/5.8-Cognito/user-pool-created.png)

#### Kiểm tra cấu hình mặc định

Cognito sẽ tự động tạo User Pool với các cấu hình mặc định:

**Sign-in experience:**
- Sign-in options: **Username** (có thể thay đổi thành Email)
- Password policy: **Cognito defaults**
- Multi-factor authentication: **Optional**

![Default Settings](/images/5-Workshop/5.8-Cognito/default-settings.png)

**Sign-up experience:**
- Self-service sign-up: **Enabled**
- Email verification: **Enabled**
- Required attributes: **email**

**Message delivery:**
- Email provider: **Send email with Cognito**

**App integration:**
- Hosted UI: **Not configured** (sẽ cấu hình ở bước sau)

#### Ghi chú quan trọng

⚠️ **Lưu ý:** Các tùy chọn sign-in identifiers và required attributes **không thể thay đổi** sau khi tạo User Pool. Nếu cần thay đổi, bạn phải tạo User Pool mới.

Ghi chú lại **User Pool ID** từ trang tổng quan vì bạn sẽ cần nó cho các bước tiếp theo:



Trong các bước tiếp theo, chúng ta sẽ cấu hình chi tiết các tính năng như password policies, email verification và tạo App Client.