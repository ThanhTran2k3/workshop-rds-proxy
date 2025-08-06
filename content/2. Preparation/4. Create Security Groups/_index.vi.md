---
title : "Tạo Security Group"
date : 2025-08-07
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---


### 🛠️ Các bước thực hiện

### 1. Tạo **SG-ALB**

- Vào **VPC Dashboard** > chọn **Security Groups**.
- Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

- Nhập thông tin:
   - **Security group name**: `SG-ALB`
   - **Description**: `Allow HTTP/HTTPS from Internet`
   - **VPC**: chọn VPC bạn đã tạo (MyApp-VPC)

   ![](/images/2.4/0002.png) 

- Phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `HTTP`, **Port**: `80`, **Source**: `0.0.0.0/0`
   - **Type**: `HTTPS`, **Port**: `443`, **Source**: `0.0.0.0/0`

   ![](/images/2.4/0003.png) 

- Nhấn **Create Security Group**.

   ![](/images/2.4/0004.png) 

---

### 2. Tạo **SG-ECS**

- Vào **VPC Dashboard** > chọn **Security Groups**.
- Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

- Nhập thông tin:
   - **Security group name**: `SG-ECS`
   - **Description**: `Allow traffic from ALB to ECS`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0005.png) 

- Phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `Custom TCP`, **Port**: `8080`, **Source**: `SG-ALB`

   ![](/images/2.4/0006.png) 

- Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---

### 3. Tạo **SG-RDSProxy**

- Vào **VPC Dashboard** > chọn **Security Groups**.
- Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 
- Nhập thông tin:
   - **Security group name**: `SG-RDSProxy`
   - **Description**: `Allow ECS to access RDS Proxy`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0007.png) 

- Phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-ECS`

   ![](/images/2.4/0008.png) 
- Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---

### 4. Tạo **SG-RDS**

- Vào **VPC Dashboard** > chọn **Security Groups**.
- Nhấn **Create Security Group**.

   ![](/images/2.4/0001.png) 

- Nhập thông tin:
   - **Security group name**: `SG-RDS`
   - **Description**: `Allow RDS Proxy to access DB`
   - **VPC**: `MyApp-VPC`

   ![](/images/2.4/0009.png) 

- Phần **Inbound rules**, nhấn **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-RDSProxy`
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-ECS`

   ![](/images/2.4/0010.png) 

- Nhấn **Create Security Group**

   ![](/images/2.4/0004.png) 

---


