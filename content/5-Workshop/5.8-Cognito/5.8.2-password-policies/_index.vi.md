---
title: "Cấu hình Password Policies"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---

#### Cấu hình Password Policies

Trong bước này, bạn sẽ cấu hình các chính sách mật khẩu để đảm bảo yêu cầu mật khẩu mạnh cho người dùng.

1. Điều hướng đến **User Pool** của bạn trong Cognito console
2. Vào tab **Authentication methods**
3. Nhấp **Edit** trong phần Password policy

![Password Policy](/images/5-Workshop/5.8-Cognito/password-policy-edit.png)

#### Cấu hình Password Policy

Cấu hình các yêu cầu mật khẩu sau:

**Độ dài mật khẩu:**
- Độ dài tối thiểu: **8 ký tự**
- Độ dài tối đa: **256 ký tự**

**Độ phức tạp mật khẩu:**
- ✅ Yêu cầu số
- ✅ Yêu cầu ký tự đặc biệt
- ✅ Yêu cầu chữ hoa
- ✅ Yêu cầu chữ thường

![Password Requirements](/images/5-Workshop/5.8-Cognito/password-requirements.png)


#### Lưu cấu hình

1. Xem lại cài đặt chính sách mật khẩu của bạn
2. Nhấp **Save changes**



#### Xác minh Password Policy

Chính sách mật khẩu hiện đã hoạt động. Người dùng sẽ cần tạo mật khẩu đáp ứng các yêu cầu này:

**Ví dụ mật khẩu hợp lệ:**
- `MySecurePass123!`
- `StrongPassword2024#`
- `TaskManager@2025`

**Ví dụ mật khẩu không hợp lệ:**
- `password` (không có chữ hoa, số, ký tự đặc biệt)
- `12345678` (không có chữ cái, ký tự đặc biệt)
- `Pass1!` (quá ngắn)