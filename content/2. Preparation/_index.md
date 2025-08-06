---
title : "Preparation"
date : 2025-08-07
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

### AWS Environment Setup

![Overall Architecture Diagram](/images/2/0001.png)

This document guides the necessary steps to set up the AWS infrastructure before deploying services like ECS, RDS, and RDS Proxy.

### Contents

- [1. Create VPC and Subnet](#1-create-vpc-and-subnet)  
- [2. Create Internet Gateway and NAT Gateway](#2-create-internet-gateway-and-nat-gateway)  
- [3. Create Route Tables](#3-create-route-tables)  
- [4. Create Security Groups](#4-create-security-groups)  
- [5. Create RDS Instance](#5-create-rds-instance)  
- [6. Use AWS Secrets Manager](#6-use-secrets-manager)  
- [7. Assign IAM Role for ECS and RDS Proxy](#7-assign-iam-role)  
- [8. Create RDS Proxy](#8-create-rds-proxy)

---

#### 1. Create VPC and Subnet

- Step 1: Create a new VPC with CIDR `10.0.0.0/16`.
- Step 2: Create four subnets:
  - **PublicSubnet1**: used for ALB and NAT Gateway (e.g., `10.0.1.0/24`)
  - **PublicSubnet2**: used for ALB and NAT Gateway (e.g., `10.0.4.0/24`)
  - **PrivateSubnet1**: used for ECS, RDS, and RDS Proxy (e.g., `10.0.2.0/24`)
  - **PrivateSubnet2**: used for ECS, RDS, and RDS Proxy (e.g., `10.0.3.0/24`)

---

#### 2. Create Internet Gateway and NAT Gateway

- Step 1: Create and attach an Internet Gateway (IGW) to the VPC.
- Step 2: Create a NAT Gateway in **PublicSubnet1** and assign an Elastic IP.
- Step 3: Configure the route tables:
  - Public subnet → IGW for direct Internet access.
  - Private subnet → NAT Gateway for indirect Internet access.

---

#### 3. Create Route Tables

- Create two route tables:
  - **MyApp-Public-RT**: assign to public subnets and route to the Internet via IGW.
  - **MyApp-Private-RT**: assign to private subnets and route to the NAT Gateway.

---

#### 4. Create Security Groups

- Step 1: Create SG-ALB to allow HTTP/HTTPS access (port 80/443) from the Internet.
- Step 2: Create SG-ECS to allow ALB to access ECS (port 8080).
- Step 3: Create SG-RDSProxy to allow ECS to access RDS Proxy (port 3306).
- Step 4: Create SG-RDS to allow ECS and Proxy to access the Database.

---

#### 5. Create RDS Instance

- Step 1: Create a DB Subnet Group.
- Step 2: Deploy the database in PrivateSubnet1 and PrivateSubnet2.
- Step 3: Choose MySQL as the database engine.
- Step 4: Attach the SG-RDS Security Group.

---

#### 6. Use Secrets Manager

- Step 1: Create a secret in AWS Secrets Manager to store DB login credentials in JSON format.
- Step 2: Record the secret ARN to use in ECS Task configuration.

---

#### 7. Assign IAM Role

- Step 1: Create IAM roles: **ecsTaskExecutionRole**, **codeDeployServiceRole**, and **rdsProxyServiceRole**
- Step 2: Attach policies to allow:
  - Access to Secrets Manager
  - Connection to RDS Database
  - Write logs to CloudWatch

---

#### 8. Create RDS Proxy

- Step 1: Create an RDS Proxy to connect ECS to RDS.
- Step 2: Select the created RDS instance.
- Step 3: Assign the **rdsProxyServiceRole** IAM role to the RDS Proxy.
- Step 4: Select subnet group **PrivateSubnet1** and **PrivateSubnet2**.
- Step 5: Assign the **SG-RDSProxy** Security Group.

---

Make sure all the components above are properly configured before proceeding with ECS deployment and establishing connection to RDS Proxy.
