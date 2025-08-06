---
title : "Create Security Groups"
date : 2025-08-07
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---



### 🛠️ Steps to Implement

### 1. Create **SG-ALB**

- Go to **VPC Dashboard** > select **Security Groups**.
- Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

- Fill in the details:
   - **Security group name**: `SG-ALB`
   - **Description**: `Allow HTTP/HTTPS from Internet`
   - **VPC**: Select `MyApp-VPC`  

   ![](/images/2.4/0002.png)

- In the **Inbound rules** section, click **Add rule**:
   - **Type**: `HTTP`, **Port**: `80`, **Source**: `0.0.0.0/0`
   - **Type**: `HTTPS`, **Port**: `443`, **Source**: `0.0.0.0/0`

   ![](/images/2.4/0003.png)  

- Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 2. Create **SG-ECS**

- Go to **VPC Dashboard** > select **Security Groups**.
- Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

- Fill in the details:
   - **Security group name**: `SG-ECS`
   - **Description**: `Allow traffic from ALB to ECS`
   - **VPC**: Select `MyApp-VPC` 

   ![](/images/2.4/0005.png)

- In the **Inbound rules** section, click **Add rule**:
   - **Type**: `Custom TCP`, **Port**: `8080`, **Source**: `SG-ALB`

   ![](/images/2.4/0006.png)  

- Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 3. Create **SG-RDSProxy**

- Go to **VPC Dashboard** > select **Security Groups**.
- Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

- Fill in the details:
   - **Security group name**: `SG-RDSProxy`
   - **Description**: `Allow ECS to access RDS Proxy`
   - **VPC**: Select `MyApp-VPC`  

     ![](/images/2.4/0007.png)

- In the **Inbound rules** section, click **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-ECS`

   ![](/images/2.4/0008.png)  

- Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---

### 4. Create **SG-RDS**

- Go to **VPC Dashboard** > select **Security Groups**.
2. Click **Create Security Group**.  

   ![](/images/2.4/0001.png)

3. Fill in the details:
   - **Security group name**: `SG-RDS`
   - **Description**: `Allow RDS Proxy to access DB`
   - **VPC**: Select `MyApp-VPC`  

   ![](/images/2.4/0009.png)

4. Under **Inbound rules**, click **Add rule**:
   - **Type**: `MYSQL/Aurora`, **Port**: `3306`, **Source**: `SG-RDSProxy`

   ![](/images/2.4/0010.png)  

5. Click **Create Security Group**.  

   ![](/images/2.4/0004.png)

---