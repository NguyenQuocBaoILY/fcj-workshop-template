---
title: "Sự kiện 3"
date: "2025-11-29"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---
# AWS Cloud Mastery Series #3 – Well-Architected Security Pillar
### 1. Thông tin sự kiện
- **Tên sự kiện:** AWS Cloud Mastery Series #3 – Well-Architected Security Pillar  
- **Thời gian:** 8:30 – 12:00  
- **Đơn vị tổ chức:** Amazon Web Services (AWS)

---

### 2. Mục đích tham dự
Tôi tham dự sự kiện nhằm nắm vững 5 trụ cột của Well-Architected Security Pillar trên AWS, hiểu các mối đe dọa bảo mật hiện đại, học cách triển khai bảo mật toàn diện từ danh tính, mạng, dữ liệu đến phản ứng sự cố, và áp dụng vào các tình huống thực tế trong doanh nghiệp.

---

### 3. Tóm tắt nội dung theo Agenda

#### **8:30 – 8:50 — Opening & Security Foundation**
Nội dung chính:
- Vai trò của Security Pillar trong Well-Architected Framework  
- Nguyên tắc cốt lõi: Least Privilege – Zero Trust – Defense in Depth  
- Mô hình trách nhiệm chia sẻ (Shared Responsibility Model)  
- Các mối đe dọa bảo mật phổ biến tại doanh nghiệp Việt Nam  

**Điểm nổi bật:**  
Hiểu nền tảng triết lý bảo mật của AWS, biết cách mô hình trách nhiệm chia sẻ áp dụng vào từng lớp dịch vụ, và nắm được bức tranh tổng quan về rủi ro bảo mật hiện nay.

---

#### **8:50 – 9:30 — Trụ cột 1: Identity & Access Management**
Nội dung chính:
- Kiến trúc IAM hiện đại: Users, Roles, Policies  
- Loại bỏ long-term credentials – dùng quyền truy cập tạm thời  
- IAM Identity Center: SSO và permission sets  
- SCP & Permission Boundaries trong quản trị đa tài khoản  
- MFA, credential rotation, Access Analyzer  
- Mini demo: IAM Policy validation  

**Điểm nổi bật:**  
Nắm vững mô hình IAM hiện đại, hiểu tầm quan trọng của việc loại bỏ long-term credentials, và được trải nghiệm mô phỏng quyền truy cập bằng công cụ validation.

---

#### **9:30 – 9:55 — Trụ cột 2: Detection & Continuous Monitoring**
Nội dung chính:
- CloudTrail logging tập trung  
- GuardDuty threat detection & Security Hub  
- Logging toàn diện: VPC Flow Logs, ALB logs, S3 access logs  
- Alerting & automation với EventBridge  
- Khái niệm Detection-as-Code  

**Điểm nổi bật:**  
Hiểu chiến lược giám sát đa lớp, cách tự động hóa phát hiện – phản ứng, và lợi ích của việc quản trị rule bảo mật như code.

---

#### **9:55 – 10:10 — Nghỉ giải lao**
Networking cùng chuyên gia bảo mật và AWS SA.

---

#### **10:10 – 10:40 — Trụ cột 3: Infrastructure Protection**
Nội dung chính:
- Chiến lược phân tách mạng và cô lập workload  
- Best practice: Public/Private subnet  
- Security Groups vs NACLs  
- Kết hợp WAF + Shield + Network Firewall  
- Bảo mật workload: EC2, ECS, EKS  

**Điểm nổi bật:**  
Nắm được chiến lược bảo mật mạng theo nhiều lớp, hiểu khi nào dùng SG và NACL, cũng như cách bảo vệ workload trên container và compute.

---

#### **10:40 – 11:10 — Trụ cột 4: Data Protection**
Nội dung chính:
- KMS: key policy, grant, rotation  
- Mã hóa dữ liệu at-rest & in-transit: S3, EBS, RDS, DynamoDB  
- Secrets Manager & Parameter Store: rotation patterns  
- Data classification & access guardrails  

**Điểm nổi bật:**  
Hiểu rõ các dịch vụ mã hóa, pattern quản lý secrets, và xây dựng hệ thống phân loại – bảo vệ dữ liệu trên AWS.

---

#### **11:10 – 11:40 — Trụ cột 5: Incident Response**
Nội dung chính:
- Vòng đời Incident Response theo AWS  
- Playbook thực tế:  
  - Lộ IAM credentials  
  - S3 public exposure  
  - Malware trên EC2  
- Snapshot, cách ly workload, thu thập evidence  
- Tự động hóa IR bằng Lambda & Step Functions  

**Điểm nổi bật:**  
Nắm được phản ứng sự cố theo chuẩn AWS, cách tự động hóa phản ứng, và xây dựng SOAR mini trên AWS.

---

#### **11:40 – 12:00 — Wrap-Up & Q&A**
Nội dung chính:
- Tổng kết 5 trụ cột Security  
- Các thách thức phổ biến trong doanh nghiệp Việt Nam  
- Lộ trình chứng chỉ: Security Specialty & SA Pro  

---

### Những điểm chính rút ra
- Hiểu đầy đủ 5 trụ cột Security của Well-Architected Framework và mối liên kết giữa chúng  
- Thành thạo IAM hiện đại theo nguyên tắc Zero Trust  
- Triển khai monitoring & automated incident response toàn diện  
- Áp dụng chiến lược bảo mật hạ tầng theo nhiều lớp  
- Thành thạo mã hóa, quản lý secrets và bảo vệ dữ liệu  
- Tiếp cận các tình huống bảo mật thực tế theo tiêu chuẩn enterprise  

---

### Khả năng áp dụng thực tế
- Đánh giá bảo mật dựa trên 5 Security Pillars  
- Thiết kế lại IAM, loại bỏ long-term credentials  
- Tự động hóa threat detection với GuardDuty – Security Hub – EventBridge  
- Xây dựng network segmentation cho hệ thống hiện tại  
- Triển khai chuẩn mã hóa và phân loại dữ liệu  
- Xây dựng playbook & tự động hóa IR cho các tình huống phổ biến  

---

### Trải nghiệm sự kiện
Tham dự “AWS Cloud Mastery Series #3” mang lại góc nhìn toàn diện về bảo mật cloud và cách triển khai thực tế trong doanh nghiệp.  
Cách trình bày của chuyên gia AWS dễ hiểu, trọng tâm và kết hợp nhiều demo minh họa.

---

### Những điểm nổi bật
- **Đào tạo chuyên sâu:** Trình bày bởi chuyên gia bảo mật AWS giàu kinh nghiệm enterprise  
- **Demo thực hành:** IAM policy validation, threat detection, incident response  
- **Case study thực tế:** Các sự cố bảo mật phổ biến và cách xử lý  
- **Kiến thức toàn diện:** Bao trùm cả 5 trụ cột Security với hướng dẫn chi tiết  

---

### Bài học quan trọng
- Bảo mật là quá trình liên tục, không phải cấu hình một lần  
- Zero Trust đòi hỏi thay đổi cách tiếp cận IAM toàn diện  
- Tự động hóa đóng vai trò quan trọng khi hệ thống mở rộng  
- Dữ liệu phải được bảo vệ ở mọi lớp từ hạ tầng đến ứng dụng  

---

### Hình ảnh sự kiện
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.1.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.2.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.3.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.4.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.5.jpg)
![Hình ảnh sự kiện](/images/4-EventParticipated/event3.6.jpg)

