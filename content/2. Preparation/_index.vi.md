---
title : "Các Bước Chuẩn Bị"
date : 2025-07-03
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Thiết Lập Môi Trường AWS

![Sơ đồ kiến trúc tổng thể](/images/2/awsAttributes.png)

Tài liệu này hướng dẫn các bước cần thiết để thiết lập hạ tầng AWS trước khi triển khai các dịch vụ như ECS, RDS và RDS Proxy.

## Nội Dung

- [1. Tạo VPC & Subnet](#1-tạo-vpc--subnet)
- [2. Tạo Internet Gateway & NAT Gateway](#2-tạo-internet-gateway--nat-gateway)
- [3. Tạo Security Groups](#3-tạo-security-groups)
- [4. Tạo RDS Instance](#4-tạo-rds-instance)
- [5. Sử Dụng Secrets Manager](#5-sử-dụng-secrets-manager)
- [6. Gán IAM Role](#6-gán-iam-role)

---

## 1. Tạo VPC & Subnet

- Bước 1: Tạo một VPC mới với CIDR `10.0.0.0/16`.
- Bước 2: Tạo hai subnet:
  - **PublicSubnet1**: dùng cho ALB và NAT Gateway (VD: `10.0.1.0/24`)
  - **PrivateSubnet1**: dùng cho ECS, RDS và RDS Proxy (VD: `10.0.2.0/24`)

---

## 2. Tạo Internet Gateway & NAT Gateway

- Bước 1: Tạo và gắn Internet Gateway (IGW) vào VPC.
- Bước 2: Tạo NAT Gateway trong `PublicSubnet1`, gán Elastic IP.
- Bước 3: Cấu hình bảng định tuyến:
  - Subnet public → IGW để truy cập Internet trực tiếp.
  - Subnet private → NAT Gateway để truy cập Internet gián tiếp.

---

## 3. Tạo Security Groups

- Bước 1: Tạo SG-ALB cho phép truy cập HTTP/HTTPS (cổng 80/443) từ Internet.
- Bước 2: Tạo SG-ECS cho phép ALB truy cập vào ECS (cổng 3000, 8080).
- Bước 3: Tạo SG-RDSProxy cho phép ECS truy cập RDS Proxy (cổng 3306, 5432).
- Bước 4: Tạo SG-RDS chỉ cho phép Proxy truy cập Database.

---

## 4. Tạo RDS Instance

- Bước 1: Chọn loại cơ sở dữ liệu (MySQL, PostgreSQL hoặc Aurora).
- Bước 2: Triển khai DB trong `PrivateSubnet1`.
- Bước 3: Gán Security Group SG-RDS và (tùy chọn) bật xác thực IAM.

---

## 5. Sử Dụng Secrets Manager

- Bước 1: Tạo secret trong AWS Secrets Manager để lưu thông tin đăng nhập DB dưới dạng JSON.
- Bước 2: Ghi lại ARN của secret để sử dụng trong cấu hình ECS Task.

---

## 6. Gán IAM Role

- Bước 1: Tạo IAM role (ví dụ: `ecsTaskExecutionRole`).
- Bước 2: Gán policy cho phép:
  - Truy cập Secrets Manager
  - Kết nối tới RDS Database
  - Ghi log lên CloudWatch

---

Hãy đảm bảo các thành phần trên được cấu hình đầy đủ trước khi tiến hành triển khai ECS và thiết lập kết nối đến RDS Proxy.
