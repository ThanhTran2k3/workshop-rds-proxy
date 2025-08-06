---
title: "Create Route Table"
date : 2025-08-07
weight : 3
chapter : false
pre: " <b> 2.3 </b> "
---

### 🛠️ Steps to Implement

#### 1. Create Route Table for Public Subnet

- Go to **VPC Dashboard** > select **Route Tables**
- Click **Create route table**  

  ![](/images/2.3/0001.png) 

- Fill in the details:
  - **Name tag**: `MyApp-Public-RT`
  - **VPC**: select `MyApp-VPC`
- Click **Create route table**  

  ![](/images/2.3/0002.png) 

- Select the route table you just created > **Routes** tab > click **Edit routes**  

  ![](/images/2.3/0003.png)

  - Click **Add route**
    - **Destination**: `0.0.0.0/0`
    - **Target**: choose **Internet Gateway (MyApp-IGW)**
  - Click **Save changes**  

  ![](/images/2.3/0004.png)  

  ![](/images/2.3/0005.png) 

- Go to the **Subnet associations** tab > click **Edit subnet associations**  

  ![](/images/2.3/0006.png) 

  - Check **PublicSubnet1**
  - Click **Save associations**  

  ![](/images/2.3/0007.png) 

---

#### 2. Create Route Table for Private Subnet

- Click **Create route table** 

  ![](/images/2.3/0001.png) 

- Fill in the details:
  - **Name tag**: `MyApp-Private-RT`
  - **VPC**: select `MyApp-VPC`
- Click **Create route table**  

  ![](/images/2.3/0008.png) 

- Select the route table you just created > **Routes** tab > **Edit routes** 

  ![](/images/2.3/0009.png) 

  - Click **Add route**
    - **Destination**: `0.0.0.0/0`
    - **Target**: choose **NAT Gateway (MyApp-NAT)**
  - Click **Save changes** 

  ![](/images/2.3/0010.png)  

  ![](/images/2.3/0011.png) 

- Go to the **Subnet associations** tab > click **Edit subnet associations** 

  ![](/images/2.3/0012.png) 

  - Check **PrivateSubnet1**
  - Click **Save associations** 

  ![](/images/2.3/0013.png) 

---