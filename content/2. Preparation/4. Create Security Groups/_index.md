---
title: "Create Security Groups"
date: 2025-07-03
weight: 4
chapter: false
pre: " <b> 2.4 </b> "
---

Security Groups (SGs) control inbound and outbound traffic for resources such as EC2, ALB, ECS, and RDS.

## 📋 Security Group Configuration Table

| SG Name     | Inbound From | Ports      | Description                     |
|-------------|--------------|------------|---------------------------------|
| SG-ALB      | 0.0.0.0/0    | 80, 443    | Public HTTP/S access            |
| SG-ECS      | SG-ALB       | 8080  | ALB connects to ECS containers  |
| SG-RDSProxy | SG-ECS       | 3306 | ECS accesses RDS Proxy          |
| SG-RDS      | SG-RDSProxy  | 3306 | Proxy connects to database      |

---

### 🛠️ Step-by-Step Instructions

### 1. Create **SG-ALB**

1. Go to **VPC Dashboard** → select **Security Groups** from the left panel.
2. Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

3. Enter:
   - **Security group name**: `SG-ALB`
   - **Description**: `Allow HTTP/HTTPS from Internet`
   - **VPC**: Select `MyApp-VPC`  

   ![](/images/2.4/0002.png)

4. Under **Inbound rules**, click **Add rule**:
   - **Type**: `HTTP`, **Port**: `80`, **Source**: `0.0.0.0/0`
   - **Type**: `HTTPS`, **Port**: `443`, **Source**: `0.0.0.0/0`

   ![](/images/2.4/0003.png)  

5. Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 2. Create **SG-ECS**

1. Go to **VPC Dashboard** → select **Security Groups** from the left panel.
2. Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

3. Enter:
   - **Security group name**: `SG-ECS`
   - **Description**: `Allow traffic from ALB to ECS`
   - **VPC**: Select `MyApp-VPC` 

   ![](/images/2.4/0005.png)

4. Under **Inbound rules**, click **Add rule**:
   - **Type**: `Custom TCP`, **Port**: `8080`, **Source**: `SG-ALB`

   ![](/images/2.4/0006.png)  

5. Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 3. Create **SG-RDSProxy**

1. Go to **VPC Dashboard** → select **Security Groups** from the left panel.
2. Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

3. Enter:
   - **Security group name**: `SG-RDSProxy`
   - **Description**: `Allow ECS to access RDS Proxy`
   - **VPC**: Select `MyApp-VPC`  

     ![](/images/2.4/0007.png)

4. Under **Inbound rules**, click **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-ECS`

   ![](/images/2.4/0008.png)  
5. Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 4. Create **SG-RDS**

1. Go to **VPC Dashboard** → select **Security Groups** from the left panel.
2. Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

3. Enter:
   - **Security group name**: `SG-RDS`
   - **Description**: `Allow RDS Proxy to access DB`
   - **VPC**: Select `MyApp-VPC`  

   ![](/images/2.4/0009.png)

4. Under **Inbound rules**, click **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-RDSProxy`

   ![](/images/2.4/0010.png)  
5. Click **Create Security Group**.  

   ![](/images/2.4/0004.png)
