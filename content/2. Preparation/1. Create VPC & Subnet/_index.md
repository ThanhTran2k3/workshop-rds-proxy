---
title : "Create VPC and Subnet"
date : 2025-08-07
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

### Amazon Virtual Private Cloud (VPC)

Amazon Virtual Private Cloud (Amazon VPC) allows you to launch AWS resources in a logically isolated network that you define. It acts like a virtual data center in the cloud.

---

### 🛠️ Steps to Implement

#### 1️. Access AWS Management Console

- Go to [AWS Management Console](https://console.aws.amazon.com/)

   ![](/images/2.1/0001.png)

- Search and select the **VPC** service

   ![](/images/2.1/0002.png)

---

#### 2️. Create a New VPC

- Go to **VPC Dashboard** > select **Your VPCs**
- Click **Create VPC**

   ![](/images/2.1/0003.png)

- Choose **VPC only**
- Fill in the details:
    - **Name tag**: `MyApp-VPC`
    - **IPv4 CIDR block**: `10.0.0.0/16`
    - **Tenancy**: `Default`

   ![](/images/2.1/0004.png) 

- Click **Create VPC**

   ![](/images/2.1/0005.png) 

📌 **Explanation:**
- CIDR `10.0.0.0/16` allows you to divide into multiple subnets.
- `Tenancy: Default` helps reduce costs and allows all EC2 instance types.

---

#### 3️. Enable DNS Settings for the VPC

- Go back to **Your VPCs**
- Select the VPC you just created > click **Actions** > **Edit VPC settings**

   ![](/images/2.1/0006.png) 

- Enable:
    - ✅ DNS **hostnames**
    - ✅ DNS **resolution**
- Click **Save**
   ![](/images/2.1/0007.png) 

---

#### 4️. Create Subnets

##### 4.1 Create PublicSubnet1
- Go to **VPC Dashboard** > select **Subnets**
- Click **Create subnet**
   ![](/images/2.1/0008.png) 
- Fill in the details:
    - **VPC ID**, select **MyApp-VPC**
    - **Subnet name**: `PublicSubnet1`
    - **Availability Zone**: Choose any (e.g., `ap-southeast-1a`)
    - **IPv4 CIDR block**: `10.0.1.0/24`
- Click **Create subnet**
   ![](/images/2.1/0009.png) 
   ![](/images/2.1/0010.png) 

---

##### 4.2 Create PrivateSubnet1
- Go to **VPC Dashboard** > select **Subnets**
- Click **Create subnet**
   ![](/images/2.1/0008.png) 
- Fill in the details:
    - **VPC ID**, select **MyApp-VPC**
    - **Subnet name**: `PrivateSubnet1`
    - **Availability Zone**: Choose any (e.g., `ap-southeast-1b`)
    - **IPv4 CIDR block**: `10.0.2.0/24`
- Click **Create subnet**
   ![](/images/2.1/0011.png) 
   ![](/images/2.1/0012.png) 

---

##### 4.3 Create PrivateSubnet2
- Go to **VPC Dashboard** > select **Subnets**
- Click **Create subnet**
   ![](/images/2.1/0008.png) 
- Fill in the details:
    - **VPC ID**, select **MyApp-VPC**
    - **Subnet name**: `PrivateSubnet2`
    - **Availability Zone**: Choose any (e.g., `ap-southeast-1b`)
    - **IPv4 CIDR block**: `10.0.3.0/24`
- Click **Create subnet**
   ![](/images/2.1/0015.png) 
   ![](/images/2.1/0016.png) 

---

### ✅ Expected Result

- A VPC named `MyApp-VPC` with CIDR `10.0.0.0/16` is created

   ![](/images/2.1/0013.png) 

- A public subnet `PublicSubnet1` created for ALB and NAT Gateway
- Two private subnet `PrivateSubnet1` created for ECS, RDS, RDS Proxy
 
   ![](/images/2.1/0014.png) 

---
