---
title : "Configure Internet Gateway & NAT Gateway"
date : 2025-07-03
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

### 🛠️ Steps to Follow

#### 1. Create an Internet Gateway (IGW)

- Go to **VPC Dashboard** > select **Internet Gateways**
- Click **Create internet gateway**  
  ![](/images/2.2/0001.png) 
- Enter:
  - **Name tag**: `MyApp-IGW`
- Click **Create internet gateway**  
  ![](/images/2.2/0002.png) 
- After creation, select **MyApp-IGW** > click **Actions** > **Attach to VPC**  
  ![](/images/2.2/0003.png) 
- Choose **MyApp-VPC** > click **Attach internet gateway**  
  ![](/images/2.2/0004.png) 

📌 **Explanation:**
- The Internet Gateway allows public subnets to access the Internet.

---

#### 2. Create a NAT Gateway

- Go to **VPC Dashboard** > **NAT Gateways**
- Click **Create NAT Gateway**  
  ![](/images/2.2/0005.png) 
- Enter:
  - **Name**: `MyApp-NAT`
  - **Subnet**: select **PublicSubnet1**
  - **Elastic IP allocation ID**: select **Allocate Elastic IP**
- Click **Create NAT Gateway**  
  ![](/images/2.2/0006.png)  
  ![](/images/2.2/0007.png) 

📌 **Explanation:**
- The NAT Gateway allows private subnets to access the Internet (e.g., for system updates, downloading packages).

---
