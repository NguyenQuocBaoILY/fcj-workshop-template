---
title: "Khởi tạo S3"
date: "2025-09-09"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#  WORKLOG ĐỒ ÁN: CẤU HÌNH S3 ORIGIN (FULL)

Worklog này bao gồm các bước tạo và cấu hình 2 Amazon S3 Buckets riêng biệt, đóng vai trò là nguồn gốc (Origin) cho các tài nguyên khác nhau của hệ thống: Code Frontend và file do người dùng tải lên.

## 1. Cấu Hình S3 Bucket 1: `taskhub-frontend-prod` (Frontend Origin)

Bucket này đóng vai trò là **Origin** cho CloudFront, chứa code Frontend (HTML, CSS, JS) và các tài sản tĩnh.

### 1.1. Truy cập và Khởi tạo Bucket
1. Đăng nhập vào AWS Console, tìm và chọn dịch vụ **Amazon S3**.
2. Click nút **"Create bucket"**.

![anh1](/images/5-workshop/5.6-S3/image1.png)

3. Điền thông tin cấu hình chung:
   
![anh2](/images/5-workshop/5.6-S3/image2.png)

| Cấu Hình | Giá Trị | Giải Thích |
| :--- | :--- | :--- |
| **Bucket name** | `taskhub-frontend-prod` | Tên Bucket phải là **duy nhất**. |
| **AWS Region** | `Asia Pacific (Singapore) ap-southeast-1` | Chọn vùng địa lý gần người dùng mục tiêu nhất để tối ưu hóa độ trễ. |

---

### 1.2. Cấu hình Object Ownership & Public Access

![anh3](/images/5-workshop/5.6-S3/image3.png)

1. **Object Ownership:** Chọn **ACLs disabled (recommended)**.
   * **Mục đích:** Quyền truy cập được quản lý tập trung bằng **Bucket Policy**, giúp quản lý đơn giản hơn.
2. **Block Public Access settings for this bucket:**
   * **Hành động:** Bỏ chọn **[ ] Block all public access** (và tất cả các tùy chọn con).
   * **Lý do:** Cho phép chúng ta cấu hình **Bucket Policy** để cấp quyền đọc **chỉ cho CloudFront OAC** (Origin Access Control) ở bước sau, đảm bảo S3 có thể hoạt động như một Origin hợp lệ.

---

### 1.3. Cấu hình Versioning và Encryption

![anh4](/images/5-workshop/5.6-S3/image4.png)

| Cấu Hình | Giá Trị | Giải Thích |
| :--- | :--- | :--- |
| **Bucket Versioning** | Chọn: **Disable** | Giảm Chi Phí Lưu Trữ vì không cần duy trì nhiều phiên bản của code Frontend. |
| **Default encryption** | **Enable** | Đảm bảo mã hóa dữ liệu khi lưu trữ (at rest). |
| **Encryption type** | Chọn: **Server-side encryption with Amazon S3 managed keys (SSE-S3)** | Phương thức mã hóa mặc định, đơn giản và hiệu quả về chi phí. |
| **Bucket Key** | Chọn: **Enable** | Giảm thiểu chi phí yêu cầu (Request Costs) liên quan đến quá trình mã hóa/giải mã. |

---

## 2. Cấu Hình S3 Bucket 2: `taskhub-files-prod` (User Files Storage)

Bucket này dùng để lưu trữ các tệp do người dùng tải lên (ảnh, media...). **Ưu tiên bảo mật tối đa.**
Làm tương tự S3 ở trên.

![anh5](/images/5-workshop/5.6-S3/image5.png)
![anh6](/images/5-workshop/5.6-S3/image6.png)

### 2.1. Truy cập và Khởi tạo Bucket
1. Lặp lại các bước tạo Bucket (Mục 1.1).
2. **Bucket name:** `taskhub-files-prod`
3. **AWS Region:** `Asia Pacific (Singapore) ap-southeast-1`

---

### 2.2. Cấu hình Public Access (KHÁC BIỆT BẢO MẬT)
- **Block Public Access settings for this bucket:** Giữ nguyên **[X] Block all public access** (CHỌN TẤT CẢ).
- **Lý do:** Đây là **Bucket Bảo Mật** chứa dữ liệu người dùng. Các tệp **KHÔNG ĐƯỢC** truy cập công khai. Việc truy cập chỉ được cấp phép tạm thời thông qua **Pre-signed URLs** được tạo bởi Backend API sau khi xác thực.

---

### 2.3. Cấu hình Versioning và Encryption (Tương Tự)

| Cấu Hình | Giá Trị | Giải Thích |
| :--- | :--- | :--- |
| **Bucket Versioning** | Chọn: **Disable** | Ngăn chặn việc tăng chi phí lưu trữ nhanh chóng khi người dùng cập nhật/xóa tệp. |
| **Default encryption** | **Enable** | Bắt buộc phải mã hóa dữ liệu người dùng. |
| **Encryption type** | Chọn: **Server-side encryption with Amazon S3 managed keys (SSE-S3)** | Tiêu chuẩn mã hóa S3. |
| **Bucket Key** | Chọn: **Enable** | Giảm chi phí yêu cầu mã hóa. |

---

