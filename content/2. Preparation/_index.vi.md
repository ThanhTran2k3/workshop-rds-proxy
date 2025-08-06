---
title : "Chuẩn Bị"
date : 2025-08-07
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

### Thiết Lập Môi Trường AWS

![Sơ đồ kiến trúc tổng thể](/images/2/0001.png)

Tài liệu này hướng dẫn các bước cần thiết để thiết lập hạ tầng AWS trước khi triển khai các dịch vụ như ECS, RDS và RDS Proxy.

### Nội Dung

- [1. Tạo VPC và Subnet](#1-tạo-vpc-và-subnet)  
- [2. Tạo Internet Gateway và NAT Gateway](#2-tạo-internet-gateway-và-nat-gateway)  
- [3. Tạo Route Tables](#3-tạo-route-tables)  
- [4. Tạo Security Groups](#4-tạo-security-groups)  
- [5. Tạo RDS Instance](#5-tạo-rds-instance)  
- [6. Sử Dụng AWS Secrets Manager](#6-sử-dụng-secrets-manager)  
- [7. Gán IAM Role cho ECS và RDS Proxy](#7-gán-iam-role)  
- [8. Tạo RDS Proxy](#8-tạo-rds-proxy)

---

#### 1. Tạo VPC và Subnet

- Bước 1: Tạo một VPC mới với CIDR `10.0.0.0/16`.
- Bước 2: Tạo bốn subnet:
  - **PublicSubnet1**: dùng cho ALB và NAT Gateway (VD: `10.0.1.0/24`)
  - **PublicSubnet2**: dùng cho ALB và NAT Gateway (VD: `10.0.4.0/24`)
  - **PrivateSubnet1**: dùng cho ECS, RDS và RDS Proxy (VD: `10.0.2.0/24`)
  - **PrivateSubnet2**: dùng cho ECS, RDS và RDS Proxy (VD: `10.0.3.0/24`)

---

#### 2. Tạo Internet Gateway và NAT Gateway

- Bước 1: Tạo và gắn Internet Gateway (IGW) vào VPC.
- Bước 2: Tạo NAT Gateway trong **PublicSubnet1**, gán Elastic IP.
- Bước 3: Cấu hình bảng định tuyến:
  - Subnet public → IGW để truy cập Internet trực tiếp.
  - Subnet private → NAT Gateway để truy cập Internet gián tiếp.

---

#### 3. Tạo Route Tables

- Tạo 2 bảng định tuyến:
  - **MyApp-Public-RT**: gán cho các public subnet, định tuyến ra Internet thông qua IGW.
  - **MyApp-Private-RT**: gán cho private subnet, định tuyến ra NAT Gateway.

---

#### 4. Tạo Security Groups

- Bước 1: Tạo SG-ALB cho phép truy cập HTTP/HTTPS (cổng 80/443) từ Internet.
- Bước 2: Tạo SG-ECS cho phép ALB truy cập vào ECS (cổng 8080).
- Bước 3: Tạo SG-RDSProxy cho phép ECS truy cập RDS Proxy (cổng 3306).
- Bước 4: Tạo SG-RDS cho phép ECS và Proxy truy cập Database.

---

#### 5. Tạo RDS Instance

- Bước 1: Tạo DB Subnet Group.
- Bước 2: Triển khai DB trong **PrivateSubnet1** và **PrivateSubnet2**.
- Bước 3: Chọn loại cơ sở dữ liệu MySQL.
- Bước 4: Gán Security Group **SG-RDS**.

---

#### 6. Sử Dụng Secrets Manager

- Bước 1: Tạo secret trong AWS Secrets Manager để lưu thông tin đăng nhập DB dưới dạng JSON.
- Bước 2: Ghi lại ARN của secret để sử dụng trong cấu hình ECS Task.

---

#### 7. Gán IAM Role

- Bước 1: Tạo IAM role: **ecsTaskExecutionRole**, **codeDeployServiceRole** và **rdsProxyServiceRole**
- Bước 2: Gán policy cho phép:
  - Truy cập Secrets Manager
  - Kết nối tới RDS Database
  - Ghi log lên CloudWatch

---

#### 8. Tạo RDS Proxy

- Bước 1: Tạo một RDS Proxy để kết nối ứng dụng ECS đến RDS.
- Bước 2: Chọn RDS instance đã tạo.
- Bước 3: Gán IAM role **rdsProxyServiceRole** cho RDS Proxy.
- Bước 4: Chọn Subnet **PrivateSubnet1** và **PrivateSubnet2**.
- Bước 5: Gán Security group **SG-RDSProxy**.


---

Hãy đảm bảo các thành phần trên được cấu hình đầy đủ trước khi tiến hành triển khai ECS và thiết lập kết nối đến RDS Proxy.
