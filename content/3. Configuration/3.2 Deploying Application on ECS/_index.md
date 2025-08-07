---
title : "Deploying Application on ECS"
date : 2025-08-07
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

### 🛠️ Steps to Implement

#### 1. Open Elastic Container Service

- Search and select the **Elastic Container Service**

  ![](/images/3.2/0001.png)

---

#### 2. Create Task Definition

- Preparation: 
  - Go to [AWS Aurora and RDS](https://console.aws.amazon.com/rds/)
    - Select **Databases** > Open **MyApp-RDS**
    - Note the **Endpoint** value for **DB_HOST**

  ![](/images/3.2/0029.png)

  - Go to [AWS Aurora and RDS](https://console.aws.amazon.com/rds/)
    - Select **Proxies** > Open **MyApp-RDS-Proxy**
    - Note the **Proxy endpoints** for **DB_PROXY_HOST**
  
  - Go to [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
    - Select **MyAPP-Secret** 
    - Note the **Secret ARN** value for **DB_USER** and **DB_PASS**

  ![](/images/3.2/0010.png)

  - Go to [JWT Secret Key Generator](https://jwtsecretkeygenerator.com/)
    - Click **Generated Key**
    - Note the **Generated Secret Key** value for **JWT_SECRET**

  ![](/images/3.2/0011.png)

  - Go to [Google Cloud Console](https://console.cloud.google.com/)
    - Note the **Client ID** for **GOOGLE_CLIENT_ID**
    - Note the **Client secret** for **GOOGLE_CLIENT_SECRET**
    - Tutorial video:

  {{< youtube ci8PGyXbzgE >}}
  
  - Go to [Cloudinary Console](https://console.cloudinary.com/)
    - Note the **Cloud name** for **CLOUD_NAME**
    - Note the **API Key** for **CLOUD_API_KEY**
    - Note the **API Secret** for **CLOUD_API_SECRET**
    - Tutorial video:

  {{< youtube 2YU6TMdYkps >}}

---

  - Go to **Amazon Elastic Container Service** > select **Task definitions**
  - Click **Create new task definition**

  ![](/images/3.2/0002.png)

- Enter information:
  - **Task definition configuration** section:
    - **Task definition family**: `MyApp-Task`
  
  ![](/images/3.2/0003.png)

  - **Infrastructure requirements** section:
    - **Launch type** > select **AWS Fargate**
    - **CPU** > select **1 vCPU**
    - **Memory** > select **2 GB**
    - **Task role** > select **ecsTaskExecutionRole**
  
  ![](/images/3.2/0004.png)

  - **Container** section:
  - **Container - 1**:
    - **Name**: `api_gateway`
    - **Image URI**: `thanhtran2k3it/api_gateway:latest`
    - **Container port**: `8080`

  ![](/images/3.2/0005.png)
      
    - **Environment variables**: 
      - `AUTH_API`: `http://localhost:8081`
      - `USER_API`: `http://localhost:8082`
      - `FILE_API`: `http://localhost:8083`
      - `DB_HOST`: `myapp-rds.c3aaekemgmo1.ap-southeast-1.rds.amazonaws.com:3306`
      - `DB_USER`: `arn:aws:secretsmanager:...:username::`
      - `DB_PASS`: `arn:aws:secretsmanager:...:password::`

  ![](/images/3.2/0006.png)

    - In **Logging - optional** > select **Use log collection**

  ![](/images/3.2/0030.png)

  - Click **Add container** to add 

  - **Container - 2**:
    - **Name**: `auth_service`
    - **Image URI**: `thanhtran2k3it/auth_service:latest`
    - **Container port**: `8081`

  ![](/images/3.2/0007.png)

    - **Environment variables**:
      - `DB_PROXY_HOST`, `DB_USER`, `DB_PASS`, `JWT_SECRET`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `HOST_NAME`

  ![](/images/3.2/0008.png)

    - In **Startup dependency ordering** > Add dependency on `api_gateway`

  ![](/images/3.2/0031.png)

  - Add **Container - 3**: `user_service`, port `8082`, similar setup

  ![](/images/3.2/0012.png)

  ![](/images/3.2/0013.png)

  - Add **Container - 4**: `file_service`, port `8083`, with Cloudinary ENV vars

  ![](/images/3.2/0014.png)

  ![](/images/3.2/0015.png)

- Click **Create**

  ![](/images/3.2/0016.png)

---- 

#### 3. Create ECS Cluster

- Go to **ECS** > **Clusters** > **Create cluster**

  ![](/images/3.2/0017.png)

- Cluster config:
  - Name: `Myapp-Cluster`
  - Use **AWS Fargate (serverless)**

  ![](/images/3.2/0018.png)

- Click **Create**  

  ![](/images/3.2/0019.png)

---

#### 4. Create Service 

- Go to ECS > Clusters > Select `Myapp-Cluster`
- Open **Services** tab > **Create**

  ![](/images/3.2/0020.png)

  ![](/images/3.2/0021.png)

- Service config:
  - Task definition: `MyApp-Task`
  - Name: `MyApp-Service`
  - Launch type: **FARGATE**, version: **LATEST**

  ![](/images/3.2/0022.png)
  ![](/images/3.2/0023.png)

  - **Networking**: VPC: `MyApp-VPC`, Subnets: Private1 & Private2, SG: `SG-ECS`, Public IP: **Turn on**

  ![](/images/3.2/0024.png)

  - **Load balancing**:
    - Enable it, type: **ALB**
    - Target: `api_gateway 8080:8080`, ALB: `MyApp-ALB`, Listener: `HTTP:80`, TG: `MyApp-TG`

  ![](/images/3.2/0025.png)
  ![](/images/3.2/0026.png)

- Click **Create**

  ![](/images/3.2/0027.png)

---