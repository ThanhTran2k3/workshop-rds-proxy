---
title : "Triển khai ứng dụng trên ECS"
date : 2025-08-07
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Mở dịch vụ Elastic Container Service

- Tìm kiếm và chọn dịch vụ **Elastic Container Service**

  ![](/images/3.2/0001.png)

---

#### 2. Tạo Task Definition

- Chuẩn bị: 
  - Truy cập [AWS Aurora and RDS](https://console.aws.amazon.com/rds/)
    - Chọn **Databases** > Mở **MyApp-RDS**
    - Ghi nhớ giá trị **Endpoint** cho **DB_HOST**

  ![](/images/3.2/0029.png)

  - Truy cập [AWS Aurora and RDS](https://console.aws.amazon.com/rds/)
    - Chọn **Proxies** > Mở **MyApp-RDS-Proxy**
    - Ghi nhớ giá trị **Proxy endpoints** cho **DB_PROXY_HOST**

  - Truy cập [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
    - Chọn **MyAPP-Secret** 
    - Ghi nhớ giá trị **Secret ARN** cho **DB_USER** và **DB_PASS**

  ![](/images/3.2/0010.png)

  - Truy cập [JWT Secret Key Generator](https://jwtsecretkeygenerator.com/)
    - Chọn **Generated Key**
    - Ghi nhớ giá trị **Generated Secret Key** cho **JWT_SECRET**

  ![](/images/3.2/0011.png)

  - Truy cập [Google Cloud Console](https://console.cloud.google.com/)
    - Ghi nhớ giá trị **Client ID** cho **GOOGLE_CLIENT_ID**
    - Ghi nhớ giá trị **Client secret** cho **GOOGLE_CLIENT_SECRET**
    - Video hướng dẫn:

  {{< youtube ci8PGyXbzgE >}}
  
  
  - Truy cập [Cloudinary Console](https://console.cloudinary.com/)
    - Ghi nhớ giá trị **Cloud name** cho **CLOUD_NAME**
    - Ghi nhớ giá trị **API Key** cho **CLOUD_API_KEY**
    - Ghi nhớ giá trị **API Secret** cho **CLOUD_API_SECRET**
    - Video hướng dẫn:

  {{< youtube 2YU6TMdYkps >}}

---

  - Vào **Amazon Elastic Container Service** > chọn **Task definitions**
  - Nhấn **Create new task definition** > chọn **Create new task definition**

  ![](/images/3.2/0002.png)

- Nhập thông tin:
  - Phần **Task definition configuration**:
    - **Task definition family**: `MyApp-Task`
  
  ![](/images/3.2/0003.png)

  - Phần **Infrastructure requirements**:
    - **Launch type** > chọn **AWS Fargate**
    - **CPU** > chọn **1 vCPU**
    - **Memory** > chọn **2 GB**
    - **Task role** > chọn **ecsTaskExecutionRole**
  
  ![](/images/3.2/0004.png)

  - Phần **Container**:
  - **Container - 1**:
    - **Name**: `api_gateway`
    - **Image URI**: `thanhtran2k3it/api_gateway:latest`
    - **Container port**: `8080`

  ![](/images/3.2/0005.png)
      
    - **Environment variables**: 
      - **Key**: `AUTH_API`, **Value type** > chọn **Value**, **Value**: `http://localhost:8081`
      - **Key**: `USER_API`, **Value type** > chọn **Value**, **Value**: `http://localhost:8082`
      - **Key**: `FILE_API`, **Value type** > chọn **Value**, **Value**: `http://localhost:8083`
      - **Key**: `DB_HOST`, **Value type** > chọn **Value**, **Value**: 
      `myapp-rds.c3aaekemgmo1.ap-southeast-1.rds.amazonaws.com:3306`
      - **Key**: `DB_USER`, **Value type** > chọn **Value**, **ValueFrom**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:username::`
      - **Key**: `DB_PASS`, **Value type** > chọn **Value**, **ValueFrom**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:password::`

  ![](/images/3.2/0006.png)

    - Trong **Logging - optional** > chọn **Use log collection**

  ![](/images/3.2/0030.png)
        
      
  - Nhấn **Add container** để thêm 

  - **Container - 2**:
    - **Name**: `auth_service`
    - **Image URI**: `thanhtran2k3it/auth_service:latest`
    - **Container port**: `8081`

  ![](/images/3.2/0007.png)

    - **Environment variables**: 
      - **Key**: `DB_PROXY_HOST`, **Value type** > chọn **Value**, **Value**:  
      `myapp-rds-proxy.proxy-c3aaekemgmo1.ap-southeast-1.rds.amazonaws.com:3306`
      - **Key**: `DB_USER`, **Value type** > chọn **ValueFrom**, **Value**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:username::`
      - **Key**: `DB_PASS`, **Value type** > chọn **ValueFrom**, **Value**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:password::`
      - **Key**: `JWT_SECRET`, **Value type** > chọn **Value**, **Value**: `HfX2n9hmoHctYOcpdobNBQprMUqnHgecolNCG8EtRvkwV9Ixv3aRpy3RxGjdydlt4Jmbna9b5r2pqTXlcMoDY88qSAj1GHtPsrJ5LGTkodAe8dgK07CPIf27LdvAIg17hWv1ndwRI0JCXyR7jw0t6MEANXyxSz9m7lo0gww1iV4NRFk3tlXwFPYJ6OvddCTLxxh5AGYeTt2QhsvHWYlguf0OPy2Wc1o1bLs6FFSrBARHqwOWpyjQiqeItHHesWpL`
      - **Key**: `GOOGLE_CLIENT_ID`, **Value type** > chọn **Value**, **Value**: `........`
      - **Key**: `GOOGLE_CLIENT_SECRET`, **Value type** > chọn **Value**, **Value**: `.........`
      - **Key**: `HOST_NAME`, **Value type** > chọn **Value**, **Value**: `http://localhost:8080`

  ![](/images/3.2/0008.png)

    - Trong **Logging - optional** > chọn **Use log collection**

  ![](/images/3.2/0030.png)

    - Trong **Startup dependency ordering - optional** > chọn **Add container dependency**:
      - **Container** > chọn `api_gateway`
      - **Condition** > chọn `START`

  ![](/images/3.2/0031.png)
      
  - Nhấn **Add container** để thêm 

  - **Container - 3**:
    - **Name**: `user_service`
    - **Image URI**: `thanhtran2k3it/user_service:latest`
    - **Container port**: `8082`

  ![](/images/3.2/0012.png)

    - **Environment variables**: 
      - **Key**: `DB_PROXY_HOST`, **Value type** > chọn **Value**, **Value**:  
      `myapp-rds-proxy.proxy-c3aaekemgmo1.ap-southeast-1.rds.amazonaws.com:3306`
      - **Key**: `DB_USER`, **Value type** > chọn **ValueFrom**, **Value**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:username::`
      - **Key**: `DB_PASS`, **Value type** > chọn **ValueFrom**, **Value**: `arn:aws:secretsmanager:ap-southeast-1:329178086719:secret:MyAPP-Secret-9taxTT:password::`
      - **Key**: `JWT_SECRET`, **Value type** > chọn **Value**, **Value**: `HfX2n9hmoHctYOcpdobNBQprMUqnHgecolNCG8EtRvkwV9Ixv3aRpy3RxGjdydlt4Jmbna9b5r2pqTXlcMoDY88qSAj1GHtPsrJ5LGTkodAe8dgK07CPIf27LdvAIg17hWv1ndwRI0JCXyR7jw0t6MEANXyxSz9m7lo0gww1iV4NRFk3tlXwFPYJ6OvddCTLxxh5AGYeTt2QhsvHWYlguf0OPy2Wc1o1bLs6FFSrBARHqwOWpyjQiqeItHHesWpL`
      - **Key**: `HOST_NAME`, **Value type** > chọn **Value**, **Value**: `http://localhost:8080`

  ![](/images/3.2/0013.png)

    - Trong **Logging - optional** > chọn **Use log collection**

  ![](/images/3.2/0030.png)

    - Trong **Startup dependency ordering - optional** > chọn **Add container dependency**:
      - **Container** > chọn `api_gateway`
      - **Condition** > chọn `START`

  ![](/images/3.2/0031.png)
      
      
  - Nhấn **Add container** để thêm 

  - **Container - 4**:
    - **Name**: `file_service`
    - **Image URI**: `thanhtran2k3it/file_service:latest`
    - **Container port**: `8083`

  ![](/images/3.2/0014.png)

    - **Environment variables**: 
      - **Key**: `CLOUD_NAME`, **Value type** > chọn **Value**, **Value**: `............`
      - **Key**: `CLOUD_API_KEY`, **Value type** > chọn **Value**, **Value**: `............`
      - **Key**: `CLOUD_API_SECRET`, **Value type** > chọn **Value**, **Value**: `............`
          
  ![](/images/3.2/0015.png)
      
    - Trong **Logging - optional** > chọn **Use log collection**

  ![](/images/3.2/0030.png)
      

- Nhấn **Create** để tạo

  ![](/images/3.2/0016.png)

---- 

#### 3. Tạo ECS Cluster

- Vào **Amazon Elastic Container Service** > chọn **Clusters**
- Nhấn **Create cluster**

  ![](/images/3.2/0017.png)

- Nhập thông tin:
  - Phần **Cluster configuration**:
    - **Cluster name**: `Myapp-Cluster`

  - Phần **Infrastructure - optional Info**:
    - Chọn **AWS Fargate (serverless)**

  ![](/images/3.2/0018.png)

- Nhấn **Create** để tạo  

  ![](/images/3.2/0019.png)

---

#### 3. Tạo Service 

- Vào **Amazon Elastic Container Service** > chọn **Clusters**
- Chọn **Myapp-Cluster**

  ![](/images/3.2/0020.png)

- Mở Tab **Services** > chọn **Create**

  ![](/images/3.2/0021.png)

- Nhập thông tin:
  - Phần **Service details**: 
    - **Task definition family** > chọn **MyApp-Task**
    - **Service name**: `MyApp-Service`
  
  ![](/images/3.2/0022.png)
  
  - Phần **Environment**:
    - **Compute options** > chọn **Launch type**
    - **Launch type** > chọn **FARGATE**
    - **Platform version** > chọn **LATEST**
  
  ![](/images/3.2/0023.png)

  - Phần **Networking**:
    - **VPC** > chọn **MyApp-VPC**
    - **Subnets** > chọn **PrivateSubnet1** và **PrivateSubnet2**
    - **Security group** > chọn **SG-ECS**
    - **Public IP** > chọn **Turn on**

  ![](/images/3.2/0024.png)
  
  - Phần **Load balancing**:
    - Chọn **Use load balancing**
    - **Load balancer type** > chọn **Application Load Balancer**
    - **Container** > chọn **api_gateway 8080:8080**
    - **Load balancer** > chọn **MyApp-ALB**
    - **Listener** > chọn **HTTP:80**
    - **Target group** > chọn **MyApp-TG**
  
  ![](/images/3.2/0025.png)

  ![](/images/3.2/0026.png)

- Nhấn **Create** để tạo 

  ![](/images/3.2/0027.png)

---