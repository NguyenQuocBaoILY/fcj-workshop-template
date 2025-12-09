---
title: "Cấu hình App Client"
date: "2025-09-09"
weight: 4
chapter: false
pre: " <b> 5.8.4. </b> "
---

#### Cấu hình App Client

Khi tạo User Pool, AWS Cognito đã tự động tạo một App Client mặc định. Trong bước này, chúng ta sẽ cấu hình App Client này để phù hợp với ứng dụng của mình.

1. Điều hướng đến **User Pool** của bạn trong Cognito console
2. Vào tab **App integration**
3. Bạn sẽ thấy App Client đã được tạo sẵn

![Default App Client](/images/5-Workshop/5.8-Cognito/default-app-client.png)

#### Chỉnh sửa App Client

1. Nhấp vào tên App Client để chỉnh sửa
2. Hoặc nhấp **Edit** nếu có



**Cập nhật thông tin app client:**
- Tên app client: **TaskManagementWebApp** (nếu cần đổi)
- Loại app client: **Public client**
- Luồng xác thực: 
  - ✅ **ALLOW_USER_PASSWORD_AUTH**
  - ✅ **ALLOW_REFRESH_TOKEN_AUTH**
  - ✅ **ALLOW_USER_SRP_AUTH**

![App Client Config](/images/5-Workshop/5.8-Cognito/app-client-config.png)

#### Cấu hình Hosted UI

**Cài đặt Hosted UI:**
- Sử dụng Cognito Hosted UI: **Enabled**
- Loại domain: **Use a Cognito domain**
- Cognito domain: **taskmanagement-auth-[your-unique-id]**

![Hosted UI Domain](/images/5-Workshop/5.8-Cognito/hosted-ui-domain.png)

**Cài đặt app client ban đầu:**
- URL callback được phép: 
  - `http://localhost:3000/callback`
  - `https://your-app-domain.com/callback`
- URL đăng xuất được phép:
  - `http://localhost:3000/`
  - `https://your-app-domain.com/`

![Callback URLs](/images/5-Workshop/5.8-Cognito/callback-urls.png)

**Cài đặt OAuth 2.0:**
- Luồng OAuth được phép:
  - ✅ **Authorization code grant**
  - ✅ **Implicit grant**
- Phạm vi OAuth được phép:
  - ✅ **email**
  - ✅ **openid**
  - ✅ **profile**

![OAuth Settings](/images/5-Workshop/5.8-Cognito/oauth-settings.png)

#### Lưu cấu hình

1. Xem lại tất cả cấu hình
2. Nhấp **Save changes**



#### Xác minh cấu hình App Client

Sau khi cập nhật, ghi chú lại thông tin quan trọng:

**Chi tiết App Client:**
- Client ID: `[your-client-id]`
- URL Hosted UI: `https://taskmanagement-auth-[your-id].auth.us-east-1.amazoncognito.com`


App Client hiện đã được cấu hình và sẵn sàng để xử lý các yêu cầu xác thực từ ứng dụng của bạn. Bạn có thể sử dụng **Client ID** và **Hosted UI URL** để tích hợp với ứng dụng web hoặc mobile của mình.