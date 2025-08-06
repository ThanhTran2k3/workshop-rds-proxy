---
title : "Tạo Route Table"
date : 2025-08-07
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Tạo Route Table cho Public Subnet

- Vào **VPC Dashboard** > chọn **Route Tables**
- Nhấn **Create route table**

  ![](/images/2.3/0001.png) 
    
- Nhập thông tin:
  - **Name tag**: `MyApp-Public-RT`
  - Nhấn **VPC**: chọn `MyApp-VPC`
- Nhấn **Create route table**

  ![](/images/2.3/0002.png) 

- Chọn route table vừa tạo > tab **Routes** > nhấn **Edit routes**

  ![](/images/2.3/0003.png) 

  - Nhấn **Add route**
    - **Destination**: `0.0.0.0/0`
    - **Target**: chọn **Internet Gateway (MyApp-IGW)**
  - Nhấn **Save changes**

  ![](/images/2.3/0004.png) 

  ![](/images/2.3/0005.png) 

- Chọn tab **Subnet associations** > **Edit subnet associations**

  ![](/images/2.3/0006.png) 
    
  - Click chọn **PublicSubnet1** và **PublicSubnet2**
  - Nhấn **Save associations**

  ![](/images/2.3/0007.png) 


---

#### 2. Tạo Route Table cho Private Subnet

- Nhấn **Create route table**

  ![](/images/2.3/0001.png) 

- Nhập thông tin:
  - **Name tag**: `MyApp-Private-RT`
  - Nhấn **VPC**: chọn `MyApp-VPC`
- Nhấn **Create route table**

  ![](/images/2.3/0008.png) 

- Chọn route table vừa tạo > tab **Routes** > **Edit routes**

  ![](/images/2.3/0009.png) 

  - Nhấn **Add route**
    - **Destination**: `0.0.0.0/0`
    - **Target**: chọn **NAT Gateway (MyApp-NAT)**
  - Nhấn **Save changes**

  ![](/images/2.3/0010.png) 

  ![](/images/2.3/0011.png) 

- Chọn tab **Subnet associations** > **Edit subnet associations**

  ![](/images/2.3/0012.png) 

  - Click chọn **PrivateSubnet1** và **PrivateSubnet2**
  - Nhấn **Save associations**

  ![](/images/2.3/0013.png) 

---
