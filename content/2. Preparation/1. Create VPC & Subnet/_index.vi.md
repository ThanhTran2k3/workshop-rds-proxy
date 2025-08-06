---
title : "Tạo VPC và Subnet"
date : 2025-08-07
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

### Amazon Virtual Private Cloud (VPC)

Amazon Virtual Private Cloud (Amazon VPC) cho phép bạn triển khai tài nguyên AWS trong một mạng logic biệt lập mà bạn tự định nghĩa. Nó hoạt động như một trung tâm dữ liệu ảo trong đám mây.

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
- Nhập thông tin:
    - **VPC ID** > chọn **MyApp-VPC**
    - **Subnet name**: `PublicSubnet1`
    - Nhấn **Availability Zone**  > chọn **ap-southeast-1a**
    - **IPv4 subnet CIDR block**: `10.0.1.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0009.png) 
   ![](/images/2.1/0010.png) 

---

##### 4.2 Tạo PublicSubnet2
- Vào **VPC Dashboard** > chọn **Subnets**
- Nhấn **Create subnet**
   ![](/images/2.1/0008.png) 
- Nhập thông tin:
    - **VPC ID** > chọn **MyApp-VPC**
    - **Subnet name**: `PublicSubnet2`
    - Nhấn **Availability Zone**  > chọn **ap-southeast-1b**
    - **IPv4 subnet CIDR block**: `10.0.4.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0013.png) 
   ![](/images/2.1/0014.png) 

---

##### 4.3 Tạo PrivateSubnet1
- Vào **VPC Dashboard** > chọn **Subnets**
- Nhấn **Create subnet**
   ![](/images/2.1/0008.png) 
- Nhập thông tin:
    - **VPC ID** > chọn **MyApp-VPC**
    - **Subnet name**: `PrivateSubnet1`
    - Nhấn **Availability Zone**  > chọn **ap-southeast-1b**
    - **IPv4 subnet CIDR block**: `10.0.2.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0011.png) 
   ![](/images/2.1/0012.png) 

##### 4.4 Tạo PrivateSubnet2
- Vào **VPC Dashboard** > chọn **Subnets**
- Nhấn **Create subnet**
   ![](/images/2.1/0008.png) 
- Nhập thông tin:
    - **VPC ID** > chọn **MyApp-VPC**
    - **Subnet name**: `PrivateSubnet2`
    - Nhấn **Availability Zone**  > chọn **ap-southeast-1c**
    - **IPv4 subnet CIDR block**: `10.0.3.0/24`
- Nhấn **Create subnet**
   ![](/images/2.1/0015.png) 
   ![](/images/2.1/0016.png) 

---
