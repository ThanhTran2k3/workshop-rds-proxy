---
title: "Create Application Load Balancer"
date : 2025-08-07
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

### 🛠️ Steps to Implement

#### 1. Open EC2 Service

- Search and select the **EC2** service

  ![](/images/3.1/0001.png)

---

#### 2. Create Target Groups

- Go to **EC2** > select **Target groups**
- Click **Create target group** 

  ![](/images/3.1/0002.png)

- Enter the following information:
    - In the **Specify group details** step:
      Under **Basic configuration**:
        - **Basic configuration** > select **IP addresses**
        - **Target group name**: `MyApp-TG`
        - **Protocol**: `HTTP`, **Port**: `8080`
        - **IP address type** > select **IPv4**
        - **VPC** select **MyAPP-VPC**
        - **Protocol version** > select **HTTP1**
        - Click **Next** to continue
  
  ![](/images/3.1/0003.png)

  ![](/images/3.1/0004.png)

  ![](/images/3.1/0005.png)

    - In the **Register targets** step:
      - Click **Create target group** to proceed
 
  ![](/images/3.1/0006.png)

---

#### 3. Create Application Load Balancer
- Go to **EC2** > select **Load Balancers**
- Click **Create load balancer**
- Under **Load balancer types**, select **Application Load Balancer**

  ![](/images/3.1/0007.png)

  ![](/images/3.1/0008.png)

- Enter the following information:
  - Under **Basic configuration**:
    - **Load balancer name**: `MyApp-ALB`
    - **Scheme** > select **Internet-facing**
    - **Load balancer IP address type** > select **IPv4`

  ![](/images/3.1/0009.png)
  
  - Under **Network mapping**:
    - **VPC** > select **MyApp-VPC**
    - **Availability Zones and subnets**:
      - Select **ap-southeast-1a (apse1-az1)** > select **PublicSubnet1**
      - Select **ap-southeast-1b (apse1-az1)** > select **PublicSubnet2**

  ![](/images/3.1/0010.png)

  - Under **Security groups**:
    - **Security groups** > select **SG-ALB**

  ![](/images/3.1/0011.png)

  - Under **Listeners and routing**:
    - **Default action** > select **MyApp-TG**

  ![](/images/3.1/0012.png)

- Click **Create load balancer** to complete

  ![](/images/3.1/0013.png)

---