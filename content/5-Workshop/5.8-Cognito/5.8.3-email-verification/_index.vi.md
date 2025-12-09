---
title: "Thiết lập Email Verification"
date: "2025-09-09"
weight: 3
chapter: false
pre: " <b> 5.8.3. </b> "
---

#### Thiết lập Email Verification

Trong bước này, bạn sẽ cấu hình xác minh email để đảm bảo người dùng xác minh địa chỉ email của họ trong quá trình đăng ký.

1. Điều hướng đến **User Pool** của bạn trong Cognito console
2. Vào tab **Sign-up**
3. Nhấp **Edit** trong phần Attribute verification and user account confirmation

![Email Verification](/images/5-Workshop/5.8-Cognito/email-verification-edit.png)

#### Cấu hình Email Verification

**Xác minh thuộc tính và xác nhận tài khoản người dùng:**
- ✅ **Send email message, verify email address**
- Tin nhắn xác minh: **Code**
- Chủ đề email xác minh: `Xác minh email của bạn cho Task Management System`
- Nội dung email xác minh: 
```
Mã xác minh của bạn cho Task Management System là {####}. 
Vui lòng nhập mã này để hoàn tất đăng ký.
```

![Email Settings](/images/5-Workshop/5.8-Cognito/email-settings.png)

#### Cấu hình Email Delivery

**Phương thức gửi email:**
- **Send email with Cognito** (cho môi trường phát triển)
- Địa chỉ email FROM: **no-reply@verificationemail.com**

![Email Delivery](/images/5-Workshop/5.8-Cognito/email-delivery.png)

**Lưu ý:** Đối với ứng dụng production, hãy xem xét sử dụng **Amazon SES** để có khả năng gửi email tốt hơn và domain tùy chỉnh.

