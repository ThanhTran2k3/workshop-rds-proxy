---
title : "Create RDS Database Instance"
date : 2025-08-07
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### 🛠️ Steps to Implement

#### 1. Open the RDS Service

- Search and select **Aurora and RDS** service

  ![](/images/2.5/0001.png)

---

#### 2. Create DB Subnet Group

- Go to **Aurora and RDS** > select **Subnet groups**
- Click **Create DB subnet group**

  ![](/images/2.5/0002.png)

- Fill in the details:
    - **Name**: `MyApp-RDSSG`
    - **Description**: `Subnet group for RDS MySQL`
    - **VPC** > select **MyApp-VPC**
    - **Availability Zone** > select **ap-southeast-1b** and **ap-southeast-1c**
    - **Subnets** > select **PrivateSubnet1** and **PrivateSubnet2**
- Click **Create**

  ![](/images/2.5/0003.png)
  ![](/images/2.5/0004.png)

---

#### 3. Create Database

- Go to **Aurora and RDS** > select **Databases**
- Click **Create database**

  ![](/images/2.5/0005.png)

- Fill in the details:
    - **Choose a database creation method** > select **Standard create**
    - **Engine options**:
      - **Engine type** > select **MySQL**

  ![](/images/2.5/0006.png)

    - **Templates** > select **Dev/Test**

  ![](/images/2.5/0007.png)

    - **Availability and durability**:
      - **Deployment options** > select **Multi-AZ DB instance deployment (2 instances)**

  ![](/images/2.5/0008.png)

    - **Settings**:
      - **DB instance identifier**: `MyApp-RDS`
      - **Master username**: `admin`
      - **Credentials management** > select **Self managed**
      - Set your own **Master password**

  ![](/images/2.5/0009.png)

    - **Instance configuration**:
      - **DB instance class** > select **Burstable classes (includes t classes)** > choose **db.t3.micro**

  ![](/images/2.5/0010.png)

    - **Storage**:
      - **Storage type** > select **General Purpose SSD (gp2)**
      - **Allocated storage**: `20`

  ![](/images/2.5/0011.png)

    - **Connectivity**:
      - **Compute resource** > select **Don’t connect to an EC2 compute resource**
      - **Virtual private cloud (VPC)** > select **MyApp-VPC**
      - **DB subnet group** > select **MyApp-RDSSG**
      - **Existing VPC security groups** > select **SG-RDS**

  ![](/images/2.5/0012.png)

  ![](/images/2.5/0013.png)

- Click **Create database**

  ![](/images/2.5/0014.png)

---