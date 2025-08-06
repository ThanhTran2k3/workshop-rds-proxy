---
title : "Tạo RDS database instance"
date : 2025-08-07
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Mở dịch vụ RDS

- Tìm kiếm và chọn dịch vụ **Aurora and RDS**

  ![](/images/2.5/0001.png)

---

#### 2. Tạo DB Subnet Group.

- Vào **Aurora and RDS** > chọn **Subnet groups**
- Nhấn **Create DB subnet group**

  ![](/images/2.5/0002.png)

- Nhập thông tin:
    - **Name**: `MyApp-RDSSG`
    - **Description**: `Subnet group for RDS MySQL`
    - **VPC** > chọn **MyApp-VPC**
    - **Availability Zone**  > chọn **ap-southeast-1b** và **ap-southeast-1c**
    - **Subnets**  > chọn **PrivateSubnet1** và **PrivateSubnet2**
- Nhấn **Create**  

    ![](/images/2.5/0003.png)

    ![](/images/2.5/0004.png)

---

#### 2. Tạo database

- Vào **Aurora and RDS** > chọn **Databases**
- Nhấn **Create database**

  ![](/images/2.5/0005.png)

- Nhập thông tin:
    - **Choose a database creation method** > chọn **Standard create**
    - Phần **Engine options**:
      - **Engine type**> chọn **MySQL**

    ![](/images/2.5/0006.png)

    - Phần **Templates** > chọn **Dev/Test**

    ![](/images/2.5/0007.png)

    - Phần **Availability and durability**:
      - **Deployment options** > chọn **Multi-AZ DB instance deployment (2 instances)**

    ![](/images/2.5/0008.png)

    - Phần **Settings**:
      - **DB instance identifier**: `MyApp-RDS`
      - **Master username**: `admin`
      - **Credentials management** > chọn **Self managed**
      - **Master password** tự đặt

    ![](/images/2.5/0009.png)

    - Phần **Instance configuration**:
      - **DB instance class**> chọn **Burstable classes (includes t classes)** > chọn **db.t3.micro**
    
    ![](/images/2.5/0010.png)

    - Phần **Storage**:
      - **Storage type**> chọn **General Purpose SSD (gp2)**
      - **Allocated storage**: `20`
    
    ![](/images/2.5/0011.png)

    - Phần **Connectivity**:
      - **Compute resource**> chọn **Don’t connect to an EC2 compute resource**
      - **Virtual private cloud (VPC)**> chọn **MyApp-VPC**
      - **DB subnet group**> chọn **MyApp-RDSSG**
      - **Existing VPC security groups**> chọn **SG-RDS**

    ![](/images/2.5/0012.png)

    ![](/images/2.5/0013.png)

- Nhấn **Create database**  

    ![](/images/2.5/0014.png)

---

