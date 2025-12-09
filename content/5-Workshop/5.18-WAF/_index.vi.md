---
title: "Cấu hình Web ACL"
date: "2025-11-14"
weight: 18
chapter: false
pre: " <b> 5.18. </b> "
---

## Bước 1 – Truy cập AWS WAF & Shield


1. Đăng nhập **AWS Console**  
2. Tìm dịch vụ: **WAF & Shield**  
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.1.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

3. Click **AWS WAF**  
4. Menu trái chọn:  
   **Protection packs (web ACLs)**  
   <div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.2.png"
         width="400"
         style="display:block; margin-left:0;">
    </div>  

5. Click nút:  
   **Create protection pack (web ACL)**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.3.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Bước 2 – Chọn loại ứng dụng (Tell us about your app)


### 2 Chọn App category  
 **API & integration services**

 <div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.4.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

## Bước 3 – Chọn CloudFront cần bảo vệ

1. Mở **Select resources to protect**  
2. Click **Add resources**  
   <div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.5.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

3. Chọn:  
   **Global → Add CloudFront or Amplify resources**  
4. Tick CloudFront của TaskHub (S3 frontend)  
<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.6.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>

5. Click **Add**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.7.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  
---

## Bước 4 – Chọn kiểu Rule Pack

1. Chọn:

✅ **Build your own pack from all of the protections AWS WAF offers**  

2. Cột bên phải chọn:

✅ **AWS-managed rule group**

3. Click **Next**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.8.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  
---

## Bước 5 – Thêm Amazon IP Reputation List

1. Chọn rule:

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.9.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

2. Trong **Rule overrides**, set:

| Rule | Action |
|------|--------|
| AWSManagedIPReputationList | ✅ Block |
| AWSManagedReconnaissanceList | ✅ Block |
| AWSManagedIPDDoSList | ✅ Count |

3. Click **Add rule**

✅ Sau bước này rule hiển thị trong danh sách **Add rules**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.10.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

---

## Bước 6 – Kiểm tra danh sách rule đã thêm


Kiểm tra thấy:

- Rule: `AWSManagedRulesAmazonIpReputationList`
- Trạng thái:  **Saved**
- WCU: **25 WCU**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.11.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  
---

## Bước 7 – Đặt tên Web ACL

1. Trong mục **Name and describe**:
   
   - **Name:** `taskhub-waf`  
   - **Description:** (bỏ trống hoặc nhập tùy ý)

---

## Bước 8 – Tạo Protection Pack (Web ACL)

1. Click:

 **Create protection pack (web ACL)**

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.12.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  

2. Đợi AWS tạo Web ACL hoàn tất

<div style="text-align: left; margin-top: 10px;">
    <img src="/images/5-Workshop/5.18-WAF/1.13.png"
         width="1024"
         style="display:block; margin-left:0;">
</div>  
