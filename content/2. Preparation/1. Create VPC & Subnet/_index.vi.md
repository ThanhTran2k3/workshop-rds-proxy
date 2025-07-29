---
title : "Tạo VPC và Subnet"
date :  2025-07-03
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

### Amazon Virtual Private Cloud (VPC)

Amazon Virtual Private Cloud (Amazon VPC) cho phép bạn triển khai tài nguyên AWS trong một mạng logic biệt lập mà bạn tự định nghĩa. Nó hoạt động như một trung tâm dữ liệu ảo trong đám mây.

---

### ✅Mục tiêu

Tạo một VPC mới với sơ đồ mạng như sau:

| Tên subnet       | CIDR           | Vai trò                         |
|------------------|----------------|----------------------------------|
| PublicSubnet1    | 10.0.1.0/24    | Chạy ALB, NAT Gateway            |
| PrivateSubnet1   | 10.0.2.0/24    | Chạy ECS Task, RDS, RDS Proxy    |

---

### 🛠️ Các bước thực hiện

#### 1️. Truy cập AWS Management Console

- Truy cập [AWS Management Console](https://console.aws.amazon.com/)

   ![](/images/2.1/0001.png)

- Tìm và chọn dịch vụ **VPC**

   ![](/images/2.1/0002.png)

---

#### 2️. Tạo VPC mới

- Vào **VPC Dashboard** > chọn **Your VPCs**
- Nhấn **Create VPC**

   ![](/images/2.1/0003.png)

- Chọn **VPC only**
- Nhập thông tin:
    - **Name tag**: `MyApp-VPC`
    - **IPv4 CIDR block**: `10.0.0.0/16`
    - **Tenancy**: `Default`

    ![](/images/2.1/0004.png) 

- Nhấn **Create VPC**

   ![](/images/2.1/0005.png) 
   

📌 **Giải thích:**
- CIDR `10.0.0.0/16` cho phép bạn chia nhỏ thành nhiều subnet.
- `Tenancy: Default` giúp tiết kiệm chi phí và không giới hạn loại EC2 instance.

---

#### 3️. Bật DNS cho VPC

- Vào lại **Your VPCs**
- Chọn VPC vừa tạo > **Actions** > **Edit VPC settings**

   ![](/images/2.1/0006.png) 

- Bật:
    - ✅ Enable DNS **hostnames**
    - ✅ Enable DNS **resolution**
- Nhấn **Save**
   ![](/images/2.1/0007.png) 

---

#### 4️. Tạo Subnet
##### 4.1 Tạo PublicSubnet1
- Vào **VPC Dashboard** > chọn **Subnets**
- Nhấn **Create subnet**
   ![](/images/2.1/0008.png) 
- Nhấn **VPC ID** > chọn **MyApp-VPC**
- Nhập thông tin:
    - **Subnet name**: `PublicSubnet1`
    - Nhấn **Availability Zone**  > chọn bất kỳ **(VD: `ap-southeast-1a`)**
    - **IPv4 subnet CIDR block**: `10.0.1.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0009.png) 
   ![](/images/2.1/0010.png) 

---

##### 4.2 Tạo PrivateSubnet1
- Vào **VPC Dashboard** > chọn **Subnets**
- Nhấn **Create subnet**
   ![](/images/2.1/0008.png) 
- Nhấn **VPC ID** > chọn **MyApp-VPC**
- Nhập thông tin:
    - **Subnet name**: `PrivateSubnet1`
    - Nhấn **Availability Zone**  > chọn bất kỳ **(VD: `ap-southeast-1b`)**
    - **IPv4 subnet CIDR block**: `10.0.2.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0011.png) 
   ![](/images/2.1/0012.png) 

---

### ✅ Kết quả đạt được

- Tạo được một VPC tên `MyApp-VPC` với CIDR `10.0.0.0/16`
   ![](/images/2.1/0013.png) 
- Tạo 1 subnet công khai `PublicSubnet1` dùng cho ALB và NAT Gateway
- Tạo 1 subnet riêng tư `PrivateSubnet1` dùng cho ECS, RDS, RDS Proxy
   ![](/images/2.1/0014.png) 

---
