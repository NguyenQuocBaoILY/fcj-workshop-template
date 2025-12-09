---
title: "Bản đề xuất"
date: "2025-12-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# TaskHub - Nền tảng quản lý nhiệm vụ và tiến độ theo mô hình DevSecOps trên AWS

### **1. Tóm tắt điều hành**

TaskHub là nền tảng quản lý nhiệm vụ và tiến trình được thiết kế nhằm
giúp các nhóm làm việc hoặc doanh nghiệp vừa và nhỏ có thể quản lý công
việc, thời hạn và tiến trình một cách trực quan và an toàn.

Hệ thống được phát triển theo mô hình **DevSecOps**, phát triển hoàn
toàn trên **AWS Serverless**, đảm bảo khả năng mở rộng, bảo mật và tối
ưu chi phí.

Quy trình phát triển khai báo sử dụng **AWS CodePipeline** và
**CodeBuild** để tự động hóa CI/CD và kiểm tra bảo mật.

### **2. Phát biểu vấn đề**

**Vấn đề:**

- Doanh nghiệp nhỏ và các nhóm dự án thường gặp khó khăn trong việc quản
  lý khối lượng công việc, theo dõi tiến độ và phân phối giữa các thành
  viên.

- Các công cụ quản lý phổ biến như Jira hoặc Asana thường có chi phí cao
  và không được hỗ trợ liền mạch với quy trình DevSecOps hoặc môi trường
  AWS.

**Giải pháp:**

- TaskHub sử dụng kiến trúc Serverless trên AWS để xây dựng nền tảng nhẹ
  nhàng, bảo mật và tiết kiệm chi phí.

- Nền tảng được phát triển bằng **AWS Lambda**, **API Gateway**,
  **DynamoDB**, **Cognito** và **S3/CloudFront**, đồng thời tích hợp
  **AWS CodePipeline** cho CI/CD và kiểm tra bảo mật động.

**Lợi ích và lợi tức đầu tư**

Giải pháp TaskHub mang lại nhiều lợi ích thiết thực cho các nhóm phát
triển và doanh nghiệp vừa và nhỏ. Hệ thống đóng vai trò là một nền tảng
trung tâm giúp quản lý nhiệm vụ, theo dõi tiến trình và phân quyền thành
viên một cách hiệu quả. Việc ứng dụng mô hình serverless trên AWS giúp
giảm thiểu chi phí vận hành, tối ưu tài nguyên và tăng khả năng mở rộng
khi nhu cầu sử dụng tăng cao. Bên cạnh đó, nền tảng này vẫn hỗ trợ xây
dựng môi trường thực hành DevSecOps, tạo tiền đề cho các nhóm nghiên cứu
và phát triển có thể mở rộng các dự án hơn nữa. Theo ước tính từ Máy
tính giá AWS, chi phí vận hành hệ thống chỉ khoảng **0,66 USD mỗi
tháng**, tương đương **7,92 USD mỗi năm**, trong khi toàn bộ tầng hạ
tầng ban đầu tận dụng dịch vụ dùng chung từ AWS, không phát sinh phần
cứng vật lý. Dự kiến thời gian hoàn vốn đạt được trong 6--12 tháng nhờ
giảm thiểu đáng kể công việc quản lý thủ công và tối ưu hóa quy trình
làm việc nội bộ.

### **3. Kiến trúc giải pháp**

Nền tảng TaskHub được xây dựng dựa trên kiến trúc AWS Serverless, đảm
bảo khả năng mở rộng hoạt động, hiệu suất cao và nền tảng vận hành chi
phí. Hệ thống tập trung vào công việc quản lý nhiệm vụ, công việc nhóm
và dự án tiến độ theo thời gian thực, đồng thời duy trì quy trình phát
triển khai tự động hóa thông tin qua mô hình DevSecOps.

Kiến trúc tổng thể bao gồm các thành phần chính như Amazon API Gateway
đảm nhận trách nhiệm tiếp nhận và phân phối yêu cầu từ người dùng, AWS
Lambda xử lý phần phụ trợ nghiệp vụ và tương tác với cơ sở dữ liệu
Amazon DynamoDB để lưu trữ thông tin nhiệm vụ, người dùng và quyền
truy cập.

Phần giao diện web được lưu trữ thông tin qua **Amazon S3** và phân
phối toàn cầu bằng **Amazon CloudFront**, trong khi AWS Cognito đảm
bảo độ xác thực và phân quyền cho người dùng.

Quy trình CI/CD được tự động hóa bằng **AWS CodePipeline** kết hợp với
**AWS CodeBuild**, giúp phát triển khai báo và kiểm tra bảo mật liên
tục mà không cần quản lý máy chủ.

Toàn bộ kiến trúc được bảo vệ bởi **AWS WAF** và **AWS KMS** nhằm tăng
cường bảo mật dữ liệu và đảm bảo bổ sung quy chuẩn DevSecOps. **AWS
X-Ray** được sử dụng để theo dõi hiệu suất và phân tích độ trễ. Kiến
trúc tổng hợp được mô tả chi tiết trong sơ đồ bên dưới:


![anh1](/images/2-Proposal/image1.png)

### Dịch vụ AWS đã sử dụng

1.  **Amazon Route 53:** Dịch vụ DNS đáng tin cậy cao, định tuyến
    traffic.

2.  **AWS WAF (Web Application Firewall):** Lớp bảo vệ nâng cao, chặn
    các cuộc tấn công phổ biến.

3.  **Amazon CloudFront:** Phân phối giao diện người dùng và nội dung
    tĩnh toàn cầu.

4.  **Amazon S3 (Simple Storage Service):** Lưu trữ tĩnh toàn bộ mã
    nguồn giao diện web (Next.js build files).

5.  **Amazon Cognito:** Quản lý xác thực và cấp quyền người dùng.

6.  **Amazon API Gateway:** Lớp giao tiếp trung gian, thực hiện xác thực
    và định tuyến các API yêu cầu đến Lambda.

7.  **AWS Lambda:** Xử lý nghiệp vụ chính. Được tích hợp để ghi nhật ký
    hoạt động vào CloudWatch Logs.

8.  **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL hiệu suất cao. Dữ liệu được
    mã hóa bằng **AWS KMS**.

9.  **AWS SNS (Simple Notification Service):** Đảm nhận vai trò thông
    báo không đồng bộ.

10. **AWS Secrets Manager:** Lưu trữ, quản lý và luân phiên các bí mật
    một cách an toàn.

11. **AWS CodePipeline, CodeBuild, & CodeGuru:**

12. **CodePipeline/CodeBuild:** Xây dựng và tự động hóa quy trình CI/CD.
    CodeBuild thực hiện các bài kiểm tra tự động và Quét Bảo mật Tĩnh
    (SAST).

13. **AWS CodeGuru:** Công cụ phân tích mã nguồn tự động, được tích hợp
    vào quy trình CI/CD để **đưa ra đề xuất thông minh về việc tối ưu
    hóa hiệu suất và cải thiện chất lượng mã**, đặc biệt quan trọng
    trong môi trường Lambda.

14. **AWS CloudFormation:** Dịch vụ **Cơ sở hạ tầng dưới dạng Mã (IaC)**
    để triển khai toàn bộ tài nguyên.

15. **AWS CloudWatch Logs & AWS X-Ray:** **CloudWatch Logs** thu thập
    nhật ký. **CloudWatch** sử dụng dữ liệu này để thiết lập cảnh báo.
    **AWS X-Ray** cung cấp khả năng theo dõi truy vết (tracing) chuyên
    sâu.

### **Thiết kế Thành phần**

1.  **Lớp Người dùng giao diện (Frontend):**

    - **Giao diện:** Ứng dụng **Next.js** được build dưới dạng tĩnh
      (Static Build).

    - **Lưu trữ & Phân phối:** Các file tĩnh được lưu trữ an toàn trong
      **Amazon S3** (được cấu hình là Origin cho CloudFront). Giao diện
      này được phân phối toàn cầu bằng **Amazon CloudFront** với độ trễ
      thấp, đồng thời được bảo vệ bởi **AWS WAF** (Web Application
      Firewall) tại lớp Edge.

2.  **Lớp Xử lý nghiệp vụ (Backend):**

    - **Cổng API:** **Amazon API Gateway** tiếp nhận mọi yêu cầu. Nó
      được cấu hình với **Cognito Authorizer** để xác thực Token của
      người dùng trước khi chuyển tiếp yêu cầu.

    - **Xử lý:** Các **Lambda Functions** chịu trách nhiệm xử lý logic
      nghiệp vụ (CRUD nhiệm vụ, quản lý nhóm, phân quyền).

    - **Quản lý Bí mật:** Mỗi Lambda function truy cập các thông tin
      nhạy cảm (như khóa API ngoài) thông qua **AWS Secrets Manager**,
      đảm bảo các bí mật không bao giờ được mã hóa cứng trong code.

3.  **Lớp Dữ liệu (Cơ sở dữ liệu):**

    - **Cơ sở dữ liệu:** **Amazon DynamoDB** được sử dụng để lưu trữ dữ
      liệu nhiệm vụ, tiến độ và cấu hình người dùng. DynamoDB hoạt động
      ở chế độ **On-Demand** để tự động mở rộng và tối ưu chi phí.

    - **Bảo mật Dữ liệu:** Toàn bộ dữ liệu tại chỗ (at-rest) trong
      DynamoDB được mã hóa bằng khóa quản lý bởi **AWS KMS (Key
      Management Service)**, đáp ứng tiêu chuẩn bảo mật cao nhất.

4.  **Bảo mật và Xác thực:**

    - **Xác thực:** **Amazon Cognito** cung cấp cơ chế đăng nhập, quản
      lý phiên và phân quyền người dùng theo vai trò (Role-Based Access
      Control). Cognito cũng hỗ trợ **Xác thực Đa Yếu tố (MFA)** và tích
      hợp SSO (Single Sign-On).

    - **Bảo vệ Edge:** **AWS WAF** được đặt trước CloudFront để ngăn
      chặn các cuộc tấn công DDoS ở Lớp 7 (Layer 7) và các lỗi bảo mật
      phổ biến khác (OWASP Top 10).

5.  **Triển khai và Giám sát:**

    - **CI/CD DevSecOps:** Mã nguồn được lưu trữ trên **GitLab** (theo
      sơ đồ) và được tự động hóa qua chuỗi **AWS
      CodePipeline/CodeBuild**. Quá trình này bao gồm việc chạy
      **CodeGuru** để tối ưu code trước khi triển khai hạ tầng qua
      **CloudFormation**.

    - **Giám sát & Gỡ lỗi:** **AWS CloudWatch Logs** thu thập nhật ký
      chi tiết từ tất cả các dịch vụ. **AWS CloudWatch** sử dụng các
      nhật ký này để thiết lập cảnh báo tự động khi phát hiện lỗi hoặc
      sự cố. **AWS X-Ray** cung cấp cái nhìn tổng quan về hiệu suất
      luồng giao dịch, hỗ trợ gỡ lỗi và tối ưu độ trễ.

### **4. Triển khai kỹ thuật**

Việc phát triển dự án TaskHub được chia thành hai phần chính--- xây dựng
hạ tầng AWS serverless và phát triển nền tảng quản lý nhiệm vụ --- mỗi
phần bao gồm giai đoạn thực hiện chính như sau:

**Khai báo các giai đoạn phát triển**

**Giai đoạn 1: Thiết kế và Lập mô hình (Tháng 1)**

- **Hành động chính: Nghiên cứu Serverless/DevSecOps, lựa chọn các dịch
  vụ cốt lõi (Lambda, DynamoDB, API Gateway). Thiết kế chi tiết sơ đồ
  kiến trúc và mô hình dữ liệu NoSQL.**

- **Sản phẩm đầu ra: Sơ đồ Kiến trúc và Tài liệu Mô hình Dữ liệu.**

**Giai đoạn 2: Khởi tạo Hạ tầng dưới dạng Mã (Tháng 2)**

- **Hành động chính: Tính toán chi phí vận hành chi tiết. Sử dụng AWS
  CDK để xây dựng mã nguồn IaC cho các dịch vụ nền tảng (S3, CloudFront,
  Cognito), đảm bảo khả năng tái tạo môi trường.**

- **Sản phẩm đầu ra: Mã nguồn AWS CDK cơ sở và Báo cáo Chi phí Vận
  hành.**

**Giai đoạn 3: Thiết lập Tự động hóa DevSecOps (Tháng 2--3)**

- **Hành động chính: Thiết lập CI/CD Pipeline hoàn chỉnh
  (CodePipeline/CodeBuild). Tích hợp các công cụ AWS CodeGuru và SAST để
  tự động hóa việc kiểm tra chất lượng và bảo mật mã nguồn trước khi
  triển khai.**

- **Sản phẩm đầu ra: CodePipeline đã hoạt động và quy trình quét bảo mật
  tự động.**

**Giai đoạn 4: Phát triển và Triển khai (Tháng 3--4)**

- **Hành động chính: Phát triển chức năng (Lambda Functions với
  TypeScript) và giao diện (Next.js). Thực hiện Kiểm tra Tích hợp
  (Integration Testing) giữa các dịch vụ. Triển khai bản chính thức
  (Production release) qua Pipeline.**

- **Sản phẩm đầu ra: TaskHub Beta Version (hoàn chỉnh CRUD) và Báo cáo
  Kiểm tra.**

**Yêu cầu kỹ thuật**

- **Kiến trúc và Công cụ: Toàn bộ hệ thống được khai báo và quản lý bằng
  AWS CDK để đảm bảo tính nhất quán của hạ tầng.**

- **Công nghệ: Backend sử dụng TypeScript/Node.js. Giao diện sử dụng
  Next.js (React).**

- **Quản lý Mã nguồn: Mã nguồn trên GitLab, triển khai tự động bằng AWS
  CodePipeline.**

- **Giám sát: Cấu hình CloudWatch, X-Ray, và CloudWatch Logs để giám sát
  hiệu suất và gỡ lỗi chuyên sâu.**

- **Yêu cầu Phi chức năng: Hệ thống được đặt tại Singapore
  (ap-southeast-1) để tối ưu tốc độ tại Việt Nam, có khả năng mở rộng
  lên 50 người dùng và sử dụng AWS KMS để mã hóa dữ liệu.**

### **5. Dòng thời gian & Các quan quan trọng**

#### **Dòng thời gian dự án**

- **Trước thực tập (Tháng 0):** Chuẩn bị kế hoạch, nghiên cứu DevSecOps
  và dịch vụ AWS Serverless.

- **Tháng 1:** Thiết lập môi trường phát triển, khởi động hạ tầng AWS và
  CI/CD đường ống.

- **Tháng 2:** Thiết kế kiến ​​trúc, phát triển chức năng chính và kiểm
  tra bảo mật tự động.

- **Tháng 3:** Tích hợp frontend -- backend, phát triển thử nghiệm và ra
  mắt nền tảng.

- **Sau khi ra mắt:** Bảo trì, đánh giá hiệu suất và mở rộng tính năng
  nâng cao.

### **6. Ước tính ngân sách**

| Tài Nguyên | Trách Nhiệm | Tỷ Lệ (USD) / Giờ |
|---|---|---|
| Kiến Trúc Sư Giải Pháp [1 người] | Thiết kế Kiến trúc Hệ thống, Thiết kế API, Thiết kế Cơ sở dữ liệu, Dẫn dắt Kỹ thuật | 6 |
| Kỹ Sư [3 người] | Phát triển Backend, Phát triển Frontend, Triển khai Bảo mật | 4 |
| Khác (DevOps) [1 người] | CI/CD, Triển khai Đám mây, Giám sát, Bảo mật, Cấu hình Bảo mật | 4 |

| Giai Đoạn Dự Án | Kiến Trúc Sư Giải Pháp | Kỹ Sư | Khác (Vui lòng chỉ định) | Tổng Giờ |
|---|---|---|---|---|
| Thiết kế Hệ thống & Kiến trúc | 20 | 10 | 0 | 30 |
| Phát triển Backend | 10 | 80 | 0 | 90 |
| Phát triển Frontend | 5 | 60 | 0 | 65 |
| Thiết lập Bảo mật & CI/CD | 5 | 30 | 10 | 45 |
| Kiểm thử & Triển khai | 5 | 30 | 0 | 35 |
| **Tổng Giờ** | **45** | **210** | **10** | **265** |
| **Tổng Chi Phí (USD)** | **270** | **840** | **40** | **800** |

Phân bổ Đóng góp Chi phí giữa Đối tác, Khách hàng, AWS:

| Bên | Đóng góp (USD) | % Đóng góp trên Tổng thể |
|---|---|---|
| Khách hàng | 0 | 0 |
| Đối tác | 0 | 0 |
| AWS | 800 | 100 |

### **7. Đánh giá rủi ro**

**Ma trận rủi ro**

- Sự cố mạng hoặc gián đoạn dịch vụ AWS: Tác động trung bình, khả năng
  trung bình.

- Khai báo CI/CD lỗi: Tác động cao, khả năng thấp.

- Vượt qua ngân sách AWS: Tác động trung bình, khả năng thấp.

- Lỗi bảo mật: Tác động cao, khả năng trung bình.

- Giảm hiệu suất khi tải tăng: Tác động trung bình, khả năng trung bình.

**Giảm thiểu Chiến lược**

- Sử dụng AWS đa vùng và giám sát CloudWatch/X-Ray.

- Kiểm tra và kiểm tra mã nguồn trước khi khai báo thông qua
  **CodePipeline**.

- Cài đặt cảnh báo chi phí qua AWS Budgets.

- Quét tự động bảo mật bằng **CodeBuild** (thay thế cho GitHub Actions).

**Kế hoạch dự phòng**

- Duy trì môi trường dàn dựng để khôi phục nhanh chóng.

- Sử dụng CloudFormation và AWS Backup để sao lưu cấu hình và dữ liệu.

### **8. Kết quả mong đợi**

**Cải tiến kỹ thuật**

- **Tự động hóa toàn diện:** Chuyển đổi hoàn toàn quy trình phát triển
  sang mô hình **DevSecOps** tự động. Thời gian triển khai (Deploy) hệ
  thống mới chỉ mất **dưới 6 phút**.

- **Hiệu suất Đảm bảo:** Ứng dụng hoạt động nhanh chóng (API phản hồi
  **dưới 150ms**) và ổn định (**99.9% Uptime**) nhờ kiến trúc
  Serverless.

- **Bảo mật Tích hợp:** Quét và khắc phục tự động các lỗi bảo mật cấp
  cao ngay trong quá trình Code Build.

- **Sẵn sàng Mở rộng:** Nền tảng có khả năng mở rộng để phục vụ nhiều
  người dùng và xử lý lượng truy cập lớn mà không cần thay đổi cấu trúc.

**Giá trị dài hạn**

- **Tạo Tài sản Kỹ thuật:** Tạo ra bộ mã **AWS CDK/CloudFormation** hoàn
  chỉnh. Đây là khuôn mẫu kiến trúc Serverless đã được tối ưu chi phí,
  có thể tái sử dụng cho **các dự án khác** của nhóm.

- **Nền tảng Vững chắc:** Thiết lập môi trường quản lý công việc và phát
  triển theo tiêu chuẩn công nghiệp (DevSecOps), sẵn sàng cho việc mở
  rộng tính năng trong tương lai.