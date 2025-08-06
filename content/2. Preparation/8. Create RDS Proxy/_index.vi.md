---
title : "Tạo RDS Proxy"
date : 2025-08-07
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Mở dịch vụ RDS

- Tìm kiếm và chọn dịch vụ **Aurora and RDS**

  ![](/images/2.5/0001.png)

---

#### 2. Tạo RDS Proxy

- Vào **Aurora and RDS** > chọn **Proxies**
- Nhấn **Create proxy**

  ![](/images/2.8/0001.png)

- Nhập thông tin:
    - Phần **Proxy configuration**:
      - **Engine family**> chọn **MariaDB and MySQL**
      - **Proxy identifier**: `MyApp-RDS-Proxy`
  
    ![](/images/2.8/0002.png)

    - Phần **Target group configuration**:
      - **Database** > chọn **MyApp-RDS**
      - **Connection pool maximum connections**: `20`
    
    ![](/images/2.8/0003.png)

    - Phần **Authentication**:
      - **Identity and access management (IAM) role** > chọn **rdsProxyServiceRole**
      - **Secrets Manager secrets** > chọn **MyAPP-Secret**
      - **Client authentication type** > chọn **MySQL Native password**

    ![](/images/2.8/0004.png)

    - Phần **Connectivity**:
      - **Subnets** > chọn **PrivateSubnet1** và **PrivateSubnet2**
      - Trong **Additional connectivity configuration**:
        - **VPC security group** > chọn **SG-RDSProxy**

- Nhấn **Create proxy** để tạo

    ![](/images/2.8/0005.png)

---