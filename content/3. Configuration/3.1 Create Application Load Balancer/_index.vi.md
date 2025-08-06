---
title : "Tạo Application Load Balancer"
date : 2025-08-07
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Mở dịch vụ EC2

- Tìm kiếm và chọn dịch vụ **EC2**

  ![](/images/3.1/0001.png)

---

#### 2. Tạo Target Groups

- Vào **EC2** > chọn **Target groups**
- Nhấn **Create target group** 

  ![](/images/3.1/0002.png)

- Nhập thông tin:
    - Bước **Specify group details**:
      Phần **Basic configuration**:
        - **Basic configuration** > chọn **IP addresses**
        - **Target group name**: `MyApp-TG`
        - **Protocol**: `HTTP`, **Port**: `8080`
        - **IP address type** > chọn **IPv4**
        - **VPC** chọn **MyAPP-VPC**
        - **Protocol version** > chọn **HTTP1**
        - Nhấn **Next** để tiếp tục
  
  ![](/images/3.1/0003.png)

  ![](/images/3.1/0004.png)

  ![](/images/3.1/0005.png)

    - Bước **Register targets**:
      - Nhấn **Create target group** đế tạo
 
  ![](/images/3.1/0006.png)

---

#### 3. Tạo Application Load Balancer
- Vào **EC2** > chọn **Load Balancers**
- Nhấn **Create load balancer** 
- **Load balancer types** > chọn **Application Load Balancer**

  ![](/images/3.1/0007.png)

  ![](/images/3.1/0008.png)

- Nhập thông tin:
  - Phần **Basic configuration**:
    - **Load balancer name**: `MyApp-ALB`
    - **Scheme** > chọn **Internet-facing**
    - **Load balancer IP address type** > chọn **IPv4**

  ![](/images/3.1/0009.png)
  
  - Phần **Network mapping**:
    - **VPC** > chọn **MyApp-VPC**
    - **Availability Zones and subnets**: 
      - Chọn **ap-southeast-1a (apse1-az1)** > chọn **PublicSubnet1**
      - Chọn **ap-southeast-1b (apse1-az1)** > chọn **PublicSubnet2**

  ![](/images/3.1/0010.png)

  - Phần **Security groups**:
    - **Security groups** > chọn **SG-ALB**

  ![](/images/3.1/0011.png)

  - Phần **Listeners and routing**:
    - **Default action** > chọn **MyApp-TG**

  ![](/images/3.1/0012.png)

- Nhấn **Create load balancer** để tạo

  ![](/images/3.1/0013.png)

---
