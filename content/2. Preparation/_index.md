---
title : "Prerequisite Steps"
date : 2025-07-03
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### AWS Environment Setup

   ![Sơ đồ kiến trúc tổng thể](/images/2/awsAttributes.png)

This guide outlines the necessary infrastructure setup steps before deploying services like ECS, RDS, and RDS Proxy.

## Content

- [1. Create VPC & Subnet](#1-create-vpc--subnet)
- [2. Create Internet Gateway & NAT Gateway](#2-create-internet-gateway--nat-gateway)
- [3. Create Security Groups](#3-create-security-groups)
- [4. Create RDS Instance](#4-create-rds-instance)
- [5. Using Secrets Manager](#5-using-secrets-manager)
- [6. Assign IAM Role](#6-assign-iam-role)

---

## 1. Create VPC & Subnet

- Step 1: Create a new VPC with CIDR block `10.0.0.0/16`.
- Step 2: Create two subnets:
  - **PublicSubnet1**: for ALB and NAT Gateway (e.g., `10.0.1.0/24`)
  - **PrivateSubnet1**: for ECS, RDS, and RDS Proxy (e.g., `10.0.2.0/24`)

---

## 2. Create Internet Gateway & NAT Gateway

- Step 1: Create and attach an Internet Gateway (IGW) to the VPC.
- Step 2: Create a NAT Gateway in `PublicSubnet1`, associate with an Elastic IP.
- Step 3: Configure route tables:
  - Public subnet → IGW for outbound internet access.
  - Private subnet → NAT Gateway to access internet indirectly.

---

## 3. Create Security Groups

- Step 1: Create SG-ALB to allow HTTP/HTTPS (80/443) from the internet.
- Step 2: Create SG-ECS to allow traffic from ALB on ports like 3000, 8080.
- Step 3: Create SG-RDSProxy to allow ECS access on DB ports (3306, 5432).
- Step 4: Create SG-RDS to allow only RDS Proxy to access the DB.

---

## 4. Create RDS Instance

- Step 1: Choose a database engine (MySQL, PostgreSQL, or Aurora).
- Step 2: Deploy the DB in `PrivateSubnet1`.
- Step 3: Attach SG-RDS and (optional) enable IAM authentication.

---

## 5. Using Secrets Manager

- Step 1: Create a secret in AWS Secrets Manager to store DB credentials in JSON format.
- Step 2: Note the ARN of the secret to reference in ECS Task definitions.

---

## 6. Assign IAM Role

- Step 1: Create an IAM role (e.g., `ecsTaskExecutionRole`).
- Step 2: Attach a policy allowing:
  - Access to Secrets Manager
  - RDS database connection
  - Logging to CloudWatch

---

Ensure all these components are in place before proceeding to ECS deployment and RDS Proxy connection setup.
