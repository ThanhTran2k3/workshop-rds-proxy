---
title : "Cấu hình"
date : 2025-07-03
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

### Nội Dung

- [1. Tạo Application Load Balancer](#1-tạo-application-load-balancer)
- [2. Triển khai ứng dụng trên ECS](#2-triển-khai-ứng-dụng-trên-ecs)
- [3. Triển khai static website](#3-triển-khai-static-website)
- [4. Cấu hình CloudFront](#4-cấu-hình-cloudfront)

---

#### 1. Tạo Application Load Balancer

- Bước 1: Tạo Target Group.
- Bước 2: Tạo ALB để phân phối lưu lượng truy cập đến các container ECS.
- Bước 3: Chọn Subnet **PublicSubnet1** và **PublicSubnet2**.
- Bước 4: Gán Security Group **SG-ALB**.

---

#### 2. Triển khai ứng dụng trên ECS

- Bước 1: Tạo ECS Cluster.
- Bước 2: Tạo **Task Definition**, chỉ định container, port, CPU, memory.
- Bước 3: Tạo **Service**, chọn loại `FARGATE`, chỉ định Task Definition.
- Bước 4: Gán Application Load Balancer vào Service để phân phối traffic.

---

#### 3. Triển khai static website

- Bước 1: Truy cập dịch vụ **S3** và tạo một bucket mới.
- Bước 2: Upload source static (HTML/CSS/JS) lên S3 bucket.
- Bước 3: Bật chế độ static website hosting cho bucket.
- Bước 4: Cấu hình **Bucket Policy** để public file nếu cần.

---

#### 4. Cấu hình CloudFront

- Bước 1: Truy cập dịch vụ **CloudFront**, tạo mới một Distribution.
- Bước 2: Chọn origin là S3 và ALB.
- Bước 3: Cấu hình custom error responses.
- Bước 4: Copy domain name CloudFront để sử dụng làm website URL chính.

---

