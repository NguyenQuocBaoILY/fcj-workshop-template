---
title: "Thiếp lập KMS"
date: "2025-11-14"
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

## Mục tiêu

Tạo **Customer Managed Key (CMK)** trên AWS KMS để:
- Mã hóa dữ liệu trong **DynamoDB**
- Mã hóa **Secrets Manager**
- Đảm bảo tiêu chuẩn **DevSecOps – dữ liệu được mã hóa bằng KMS**

---

## Bước 1 – Truy cập AWS Key Management Service (KMS)

1. Đăng nhập **AWS Console**  
2. Tìm dịch vụ: **KMS**  
3. Chọn **Key Management Service**

![Tìm dịch vụ KMS](/images/5-Workshop/5.9-KeyManagementService/9.1.png)

4. Vào menu **Customer managed keys**  
5. Click **Create a key**

![Tạo Customer managed key](/images/5-Workshop/5.9-KeyManagementService/9.2.png)

---

## Bước 2 – Cấu hình khóa (Configure Key)

1. Tại **Key type**, chọn **Symmetric**  
2. Tại **Key usage**, chọn **Encrypt and decrypt**

![Cấu hình Key type và Key usage](/images/5-Workshop/5.9-KeyManagementService/9.3.png)

3. Các tùy chọn khác giữ mặc định  
4. Click **Next**

---

## Bước 3 – Thêm nhãn (Alias & Mô tả)

### Alias

Nhập:

~~~text
taskhub_kms
~~~

![Nhập alias taskhub_kms](/images/5-Workshop/5.9-KeyManagementService/9.4.png)

Click **Next**

---

## Bước 4 – Phân quyền quản trị khóa (Key Administrative Permissions)

1. Trong danh sách **Key administrators**, chọn **QuocBao**

![Chọn Key administrators](/images/5-Workshop/5.9-KeyManagementService/9.5.png)

2. Giữ nguyên tùy chọn **Allow key administrators to delete this key**  
3. Click **Next**

User được chọn ở bước này có toàn quyền quản trị KMS Key:
- Chỉnh sửa policy  
- Kích hoạt / vô hiệu hóa key  
- Xóa key  

---

## Bước 5 – Phân quyền sử dụng khóa (Key Usage Permissions)

1. Trong danh sách **Key users**, chọn **QuocBao**

![Chọn Key users](/images/5-Workshop/5.9-KeyManagementService/9.6.png)

2. Không cần thêm **Other AWS accounts**  
3. Click **Next**

---

## Bước 6 – Chỉnh sửa chính sách khóa (Edit Key Policy)

1. Tại bước **Edit key policy**, click **Edit**  
2. Kiểm tra policy phải có đầy đủ các nhóm quyền sau:
   - Root account: `kms:*`  
   - User **QuocBao** được phép quản trị key và sử dụng key  

~~~json
{
  "Version": "2012-10-17",
  "Id": "key-consolepolicy-3",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow access for Key Administrators",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:Create*",
        "kms:Describe*",
        "kms:Enable*",
        "kms:List*",
        "kms:Put*",
        "kms:Update*",
        "kms:Revoke*",
        "kms:Disable*",
        "kms:Get*",
        "kms:Delete*",
        "kms:TagResource",
        "kms:UntagResource",
        "kms:ScheduleKeyDeletion",
        "kms:CancelKeyDeletion",
        "kms:RotateKeyOnDemand"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::166557634525:user/QuocBao"
      },
      "Action": [
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {
        "Bool": {
          "kms:GrantIsForAWSResource": "true"
        }
      }
    }
  ]
}
~~~

3. Chỉnh sửa nội dung policy nếu cần cho đúng ARN tài khoản  

![Trang edit key policy](/images/5-Workshop/5.9-KeyManagementService/9.7.png)

4. Click **Next**

AWS sẽ tự động gắn policy hợp lệ cho key.

---

## Bước 7 – Kiểm tra & Hoàn tất (Review & Finish)

1. Kiểm tra lại toàn bộ cấu hình:

| Mục       | Giá trị             |
|----------|---------------------|
| Key type | Symmetric           |
| Key usage | Encrypt and decrypt |
| Alias    | taskhub_kms         |
| Key Admin | QuocBao            |
| Key User  | QuocBao            |

2. Click **Finish**

AWS bắt đầu khởi tạo KMS Key.

---

## Bước 8 – Xác nhận tạo KMS Key thành công

![Danh sách Customer managed keys](/images/5-Workshop/5.9-KeyManagementService/9.8.png)

1. Sau khi tạo thành công, truy cập lại **KMS → Customer managed keys**  
2. Kiểm tra thông tin:
   - Alias: `taskhub_kms`  
   - Status: `Enabled`  
   - Key type: `Symmetric`  
   - Key spec: `SYMMETRIC_DEFAULT`  
   - Key usage: `Encrypt and decrypt`  

![Chi tiết key taskhub_kms](/images/5-Workshop/5.9-KeyManagementService/9.9.png)

Như vậy KMS Key đã được tạo thành công.
