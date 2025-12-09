---
title: "Khởi tạo CloudFront"
date: "2025-12-09"
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

# CẤU HÌNH CLOUDFRONT DISTRIBUTION HOÀN CHỈNH

Worklog này trình bày các bước cấu hình CloudFront Distribution, tập trung vào thiết lập Bảo mật Origin (OAC) và xác nhận các điều kiện tiên quyết trên S3 Bucket.

## 1. KIỂM TRA ĐIỀU KIỆN TIÊN QUYẾT S3

Trước khi hoàn tất CloudFront, phải đảm bảo S3 Bucket **`taskhub-frontend-prod`** đã được bảo vệ và cấu hình đúng.

### 1.1. Static Website Hosting Status
* **Kiểm tra:** Tab **Properties** của S3 Bucket.
* **Trạng thái:** **S3 static website hosting** phải ở trạng thái **Disabled**.
![anh1](/images/5-workshop/5.7-CloudFront/image1.png)
* **Giải thích:** CloudFront là CDN, không cần S3 tự phân phát nội dung.

### 1.2. Block Public Access & Bucket Policy
* **Kiểm tra:** Tab **Permissions** của S3 Bucket.
* **Block public access (bucket settings):** Phải ở trạng thái **On** (`Block all public access`).
![anh2](/images/5-workshop/5.7-CloudFront/image2.png)
* **Bucket policy (Xác nhận cuối cùng):** Phải chứa Policy OAC đã được **CloudFront tự động cập nhật**.

---

## 2. CẤU HÌNH CLOUDFRONT DISTRIBUTION

### 2.1. Step 1 & 2: Get started

![anh4](/images/5-workshop/5.7-CloudFront/image4.png)
![anh5](/images/5-workshop/5.7-CloudFront/image5.png)
![anh6](/images/5-workshop/5.7-CloudFront/image6.png)


| Cấu Hình | Giá Trị | Giải Thích |
| :--- | :--- | :--- |
| **Hành động** | Click **"Create distribution"**. | Bắt đầu quá trình tạo CDN. |
| **Kế hoạch** | Chọn **Free Plan** ($0/month). | Kế hoạch miễn phí cho đồ án. |
| **Distribution name** | `taskhub-frontend-cdn` | Đặt tên dễ nhớ. |
| **Distribution type** | **Single website or app**. | Loại hình phù hợp cho Frontend. |

### 2.2. Step 3: Specify origin (OAC Setup)

#### 1. Chỉ định Origin
* **Origin type:** Chọn **Amazon S3**.
![anh7](/images/5-workshop/5.7-CloudFront/image7.png)
* **S3 origin:** Chọn S3 Bucket **`taskhub-frontend-prod`**.
![anh8](/images/5-workshop/5.7-CloudFront/image8.png)

#### 2. Thiết lập OAC (Bảo mật Tự động)
1.  Tick **"Allow private S3 bucket access to CloudFront - Recommended"**.
2.  Chọn **Use recommended origin settings**.
    * **Giải thích:** Khi hoàn tất tạo Distribution, CloudFront sẽ tự động cập nhật Bucket Policy để cấp quyền truy cập bằng OAC.

#### 3. Cấu hình Cache
* **Cache settings:** Chọn **Use recommended cache settings tailored to serving S3 content**.

### 2.3. Step 4 & 5: Security & TLS

![anh9](/images/5-workshop/5.7-CloudFront/image9.png)

| Cấu Hình | Giá Trị Áp Dụng | Giải Thích |
| :--- | :--- | :--- |
| **WAF** | Bỏ chọn các tính năng trả phí. | Giữ mặc định cho đồ án. |
| **Viewer protocol policy** | **Redirect HTTP to HTTPS** | Bắt buộc sử dụng giao thức an toàn. |
| **Custom SSL certificate** | **Default CloudFront Certificate** | Kích hoạt HTTPS miễn phí. |

### 2.4. Step 6: Review and create

#### 1. Review Origin (Xác nhận nguồn gốc)
* **S3 origin:** **taskhub-frontend-prod**. (Phải đảm bảo đã chọn đúng Bucket Frontend).
* **Grant CloudFront access to origin:** Phải là **Yes**. (Xác nhận OAC hoạt động).

#### 2. Cấu hình Final
* **Price Class:** Chọn **Use all edge locations (best performance)**.
* Click nút **"Create distribution"** để triển khai.

![anh10](/images/5-workshop/5.7-CloudFront/image10.png)

---

## 3. KIỂM TRA SAU TRIỂN KHAI: XÁC NHẬN BẢO MẬT OAC

Sau khi Distribution chuyển sang trạng thái **`Deploying`**, bạn thực hiện bước kiểm tra quan trọng này để xác nhận OAC đã hoạt động:

1.  **Truy cập S3 Bucket** `taskhub-frontend-prod`.
2.  Vào tab **"Permissions"**.
3.  Tìm mục **"Bucket policy"**.
4.  **Xác nhận:** Policy phải được cập nhật tự động và chứa đoạn JSON ủy quyền cho dịch vụ CloudFront. Điều này chứng tỏ OAC đã khóa truy cập trực tiếp và chỉ cho phép CloudFront đọc tệp.

![anh3](/images/5-workshop/5.7-CloudFront/image3.png)