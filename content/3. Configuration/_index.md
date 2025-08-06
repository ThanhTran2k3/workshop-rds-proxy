---
title : "Configuration"
date : 2025-07-03
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

### Table of Contents

- [1. Create Application Load Balancer](#1-create-application-load-balancer)
- [2. Deploy application on ECS](#2-deploy-application-on-ecs)
- [3. Deploy static website](#3-deploy-static-website)
- [4. Configure CloudFront](#4-configure-cloudfront)

---

#### 1. Create Application Load Balancer

- Step 1: Create a Target Group.
- Step 2: Create an ALB to distribute traffic to ECS containers.
- Step 3: Select **PublicSubnet1** and **PublicSubnet2**.
- Step 4: Attach the **SG-ALB** Security Group.

---

#### 2. Deploy application on ECS

- Step 1: Create an ECS Cluster.
- Step 2: Create a **Task Definition**, specifying container, port, CPU, and memory.
- Step 3: Create a **Service**, choose `FARGATE` launch type, and assign the Task Definition.
- Step 4: Attach the Application Load Balancer to the Service to handle traffic.

---

#### 3. Deploy static website

- Step 1: Go to the **S3** service and create a new bucket.
- Step 2: Upload the static source files (HTML/CSS/JS) to the S3 bucket.
- Step 3: Enable static website hosting for the bucket.
- Step 4: Configure the **Bucket Policy** to make files public if needed.

---

#### 4. Configure CloudFront

- Step 1: Go to the **CloudFront** service and create a new Distribution.
- Step 2: Choose origins as S3 and ALB.
- Step 3: Configure custom error responses.
- Step 4: Copy the CloudFront domain name to use as the main website URL.

---
