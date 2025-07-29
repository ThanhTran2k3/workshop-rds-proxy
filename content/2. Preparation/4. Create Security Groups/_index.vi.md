---
title : "Tạo Security Groups"
date :  2025-07-03
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---



Security Group (SG) dùng để kiểm soát lưu lượng vào và ra của các resource như EC2, ALB, ECS, RDS...

## 📋 Bảng cấu hình SG

| SG Name      | Inbound From | Ports       | Ghi chú                     |
|--------------|--------------|-------------|-----------------------------|
| SG-ALB       | 0.0.0.0/0    | 80, 443     | Truy cập HTTP/S từ Internet |
| SG-ECS       | SG-ALB       |  8080 | ALB gọi container (ECS)     |
| SG-RDSProxy  | SG-ECS       | 3306  | ECS gọi tới Proxy           |
| SG-RDS       | SG-RDSProxy  | 3306 | Proxy gọi DB                |

---

### 🛠️ Các bước thực hiện

### 1. Tạo **SG-ALB**

1. Vào **VPC Dashboard** → chọn **Security Groups** bên trái.
2. Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

3. Nhập:
   - **Security group name**: `SG-ALB`
   - **Description**: `Allow HTTP/HTTPS from Internet`
   - **VPC**: chọn VPC bạn đã tạo (MyApp-VPC)

   ![](/images/2.4/0002.png) 

4. Ở phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `HTTP`, **Port**: `80`, **Source**: `0.0.0.0/0`
   - **Type**: `HTTPS`, **Port**: `443`, **Source**: `0.0.0.0/0`

   ![](/images/2.4/0003.png) 

5. Nhấn **Create Security Group**.

   ![](/images/2.4/0004.png) 

---

### 2. Tạo **SG-ECS**

1. Vào **VPC Dashboard** → chọn **Security Groups** bên trái.
2. Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

3. Nhập:
   - **Security group name**: `SG-ECS`
   - **Description**: `Allow traffic from ALB to ECS`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0005.png) 

4. Ở phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `Custom TCP`, **Port**: `8080`, **Source**: `SG-ALB`

   ![](/images/2.4/0006.png) 

5. Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---

### 3. Tạo **SG-RDSProxy**

1. Vào **VPC Dashboard** → chọn **Security Groups** bên trái.
2. Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 
3. Nhập:
   - **Security group name**: `SG-RDSProxy`
   - **Description**: `Allow ECS to access RDS Proxy`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0007.png) 

4. Ở phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-ECS`

   ![](/images/2.4/0008.png) 
5. Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---

### 4. Tạo **SG-RDS**

1. Vào **VPC Dashboard** → chọn **Security Groups** bên trái.
2. Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

3. Nhập:
   - **Security group name**: `SG-RDS`
   - **Description**: `Allow RDS Proxy to access DB`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0009.png) 

4. Ở phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-RDSProxy`

   ![](/images/2.4/0010.png) 

5. Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---


