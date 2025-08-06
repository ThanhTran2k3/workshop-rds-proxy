---
title : "Create RDS Proxy"
date : 2025-08-07
weight : 8
chapter : false
pre : " <b> 2.8 </b> "
---

### 🛠️ Steps to Implement

#### 1. Open the RDS Service

- Search and select the **Aurora and RDS** service

  ![](/images/2.5/0001.png)

---

#### 2. Create RDS Proxy

- Go to **Aurora and RDS** > select **Proxies**
- Click **Create proxy**

  ![](/images/2.8/0001.png)

- Fill in the details:
    - In the **Proxy configuration** section:
      - **Engine family** > select **MariaDB and MySQL**
      - **Proxy identifier**: `MyApp-RDS-Proxy`
  
    ![](/images/2.8/0002.png)

    - In the **Target group configuration** section:
      - **Database** > select **MyApp-RDS**
      - **Connection pool maximum connections**: `20`
    
    ![](/images/2.8/0003.png)

    - In the **Authentication** section:
      - **Identity and access management (IAM) role** > select **rdsProxyServiceRole**
      - **Secrets Manager secrets** > select **MyAPP-Secret**
      - **Client authentication type** > select **MySQL Native password**

    ![](/images/2.8/0004.png)

    - In the **Connectivity** section:
      - **Subnets** > select **PrivateSubnet1** and **PrivateSubnet2**
      - In **Additional connectivity configuration**:
        - **VPC security group** > select **SG-RDSProxy**

- Click **Create proxy** to create it

    ![](/images/2.8/0005.png)

---