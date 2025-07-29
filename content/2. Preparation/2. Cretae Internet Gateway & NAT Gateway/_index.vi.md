---
title : "Cấu hình Internet Gateway & NAT Gateway"
date :  2025-07-03
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Tạo Internet Gateway (IGW)

- Vào **VPC Dashboard** > chọn **Internet Gateways**
- Nhấn **Create internet gateway**
  ![](/images/2.2/0001.png) 
- Nhập:
  - **Name tag**: `MyApp-IGW`
- Nhấn **Create internet gateway**
  ![](/images/2.2/0002.png) 
- Sau khi tạo xong, chọn **MyApp-IGW** > nhấn **Actions** > **Attach to VPC**
  ![](/images/2.2/0003.png) 
- Chọn **MyApp-VPC** > nhấn **Attach internet gateway**
  ![](/images/2.2/0004.png) 
  

📌 **Giải thích:**
- Internet Gateway cho phép các subnet public truy cập Internet.

---

#### 2. Tạo NAT Gateway

- Vào **VPC Dashboard** > **NAT Gateways**
- Nhấn **Create NAT Gateway**
   ![](/images/2.2/0005.png) 
- Nhập:
  - **Name**: `MyApp-NAT`
  - Nhấn **Subnet**: chọn **PublicSubnet1**
  - **Elastic IP allocation ID**: chọn **Allocate Elastic IP**
- Nhấn **Create NAT Gateway**
    ![](/images/2.2/0006.png) 
    ![](/images/2.2/0007.png) 

📌 **Giải thích:**
- NAT Gateway giúp các subnet private truy cập Internet (ví dụ: để cập nhật hệ thống, tải package).

---

