---
title: "Worklog Tuần 2"
date: "2025-11-14"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu Tuần 2:

* Hiểu các kiến thức cơ bản về AWS Virtual Private Cloud 
* Tìm hiểu về bảo mật VPC và các tính năng Multi-VPC
* Hiểu các kiến thức cơ bản về VPC, DirectConnect, Load Balancer và các tài nguyên bổ sung khác

### Các nhiệm vụ cần thực hiện trong tuần:
| Thứ | Công việc                                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Học các kiến thức liên quan đến Virtual Private Cloud <br> - Tường lửa trong VPC <br> - **Thực hành:** <br>&emsp; + Tạo VPC, Subnet, Internet Gateway <br>&emsp; + Tạo Route Table, Security Group <br>&emsp; + Bật VPC Flow Logs <br> - Triển khai Amazon EC2 Instances                                                                                        | 15/09/2025 | 15/09/2025      | <https://000003.awsstudygroup.com/1-introduce/> |
| 3   | - Thiết lập Hybrid DNS với Route 53 Resolver <br> - **Thực hành:** <br>&emsp; + Tạo Key Pair <br>&emsp; + Khởi tạo CloudFormation Template <br>&emsp; + Cấu hình Security Group <br>&emsp; + Kết nối tới RDGW                                              | 16/09/2025 | 16/09/2025      | <https://000010.awsstudygroup.com/3-connecttordgw/> |
| 4   | - Tìm hiểu về VPC Peering và cách thiết lập <br> - **Thực hành:** <br>&emsp; + Khởi tạo CloudFormation Template <br>&emsp; + Tạo Security Group <br> &emsp; + Tạo EC2 Instance <br> &emsp; + Cập nhật Network ACL <br> &emsp; + Cấu hình Route Tables để bật giao tiếp giữa các VPC đã peering <br> &emsp; + Bật và kiểm tra Cross-Peer DNS | 17/09/2025 | 17/09/2025      | <https://000019.awsstudygroup.com/6-crosspeerdns/> |
| 5   | - Tìm hiểu kiến thức cơ bản về AWS Transit Gateway <br>  - **Thực hành:** <br>&emsp; + Tạo Transit Gateway <br>&emsp; + Tạo Transit Gateway Attachments <br>&emsp; + Tạo Transit Gateway Route Tables <br>&emsp; + Thêm Transit Gateway Routes vào VPC Route Tables                           | 18/08/2025 | 18/08/2025      | <https://000020.awsstudygroup.com/2-prerequiste/> |
| 6   | - Tổng hợp kiến thức về nền tảng Networking (VPC, Subnets, Gateways, Routing) <br> - Làm lại một số bài tập về VPC Peering và kết nối các VPC thông qua Transit Gateway                                                                                     | 19/08/2025 | 19/08/2025      |  |


### Kết quả đạt được trong Tuần 2:

* **Nắm vững nền tảng VPC:** Tạo và cấu hình thành công các thành phần cốt lõi của Virtual Private Cloud (VPC):  
    * Subnets, Internet Gateway, Route Tables và Security Groups.
* **Triển khai & Giám sát cơ bản:** Có kinh nghiệm thực tế trong việc triển khai **Amazon EC2 Instances** trong môi trường VPC tự tạo.
* **Bảo mật & Giám sát mạng:** Áp dụng các nguyên tắc bảo mật bằng cách cấu hình **Firewall (Security Groups và Network ACLs)** và bật thành công **VPC Flow Logs** để giám sát lưu lượng.
* **Kết nối Multi-VPC:** Thành thạo việc thiết lập và kiểm tra kết nối trong các mô hình mạng phức tạp:  
    * **VPC Peering:** Cấu hình Route Tables và kiểm thử **Cross-Peer DNS** giữa các VPC.  
    * **Transit Gateway (TGW):** Tìm hiểu và triển khai TGW, quản lý attachments và route tables để kết nối nhiều VPC ở quy mô lớn.
* **Hybrid DNS & Truy cập:** Thực hành xây dựng **Hybrid DNS** sử dụng **Route 53 Resolver** và cấu hình các phương thức truy cập an toàn (tạo Key Pair, truy cập RDGW) thông qua CloudFormation.
* **Tổng hợp kiến thức:** Hệ thống hóa toàn bộ kiến thức Networking trong tuần thông qua các bài tập tích hợp về định tuyến và bảo mật trong hệ thống Multi-VPC.
