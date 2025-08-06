---
title : "Clean Up Resources"
date : 2025-08-07
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

### Resource Cleanup
ℹ️ **Note**: Properly cleaning up AWS resources after completing the lab is essential to avoid unexpected charges to your AWS account. Follow the steps below to ensure all resources have been terminated and deleted.

### 🛠️ Steps to Implement

#### 1. Clean up CloudFront

- Go to [AWS CloudFront](https://console.aws.amazon.com/cloudfront/)
   - Select **Distributions** > choose the created **distribution**
   - Click **Disable**, this action will take a few minutes.
   - Once disabled, click **Delete**

   ![](/images/4/0001.png)

   ![](/images/4/0002.png)

   - Go to **Origin access** > select the generated **Origin access controls**
   - Click **Delete**

   ![](/images/4/0038.png)

---

#### 2. Clean up S3 bucket
- Go to [AWS S3](https://console.aws.amazon.com/s3/)
   - Select **General purpose buckets** > open the created **bucket**
   - Select all files inside the bucket
   - Click **Delete**
   - **To confirm deletion, type permanently delete in the text input field.**: `permanently delete`
   - Click **Delete objects**

   ![](/images/4/0004.png)

   ![](/images/4/0005.png)

   - Return to **General purpose buckets** > select the created **bucket**
   - Click **Delete**
   - **To confirm deletion, enter the name of the bucket in the text input field.**: `your-bucket-name`
   - Click **Delete bucket**

   ![](/images/4/0003.png)

   ![](/images/4/0006.png)

---

#### 3. Clean up ECS
- Go to [AWS ECS](https://console.aws.amazon.com/ecs/)
   - Select **Clusters** > open **Myapp-Cluster**
   - Under the **Services** tab
   - Select **MyApp-Service** > click **Delete service**

   ![](/images/4/0007.png)

   - Choose **Force delete**
   - **To confirm deletion, enter delete in the text input field.**: `delete`
   - Click **Delete**

   ![](/images/4/0008.png)

   - Click **Delete cluster**

   ![](/images/4/0009.png)

   - **Enter the phrase "delete Myapp-Cluster" to confirm deletion.**: `delete Myapp-Cluster`
   - Click **Delete**, this will take a few minutes.

   ![](/images/4/0010.png)

   - Go to **Task definitions** > open **MyApp-Task**
   - Select all **Task definition: revision**
   - Click **Actions** > choose **Deregister**

   ![](/images/4/0011.png)

---

#### 4. Clean up Application Load Balancer and Target Groups
- Go to [EC2](https://console.aws.amazon.com/ec2/)
   - Select **Load Balancers** > choose **MyApp-ALB**
   - Click **Actions** > choose **Delete load balancer**

   ![](/images/4/0012.png)

   - **Type confirm to agree.**: `confirm`
   - Click **Delete**

   ![](/images/4/0013.png)

   - Select **Target Groups** > choose **MyApp-TG**
   - Click **Actions** > choose **Delete**

   ![](/images/4/0014.png)

---

#### 5. Clean up RDS Proxy
- Go to [AWS RDS](https://console.aws.amazon.com/rds/)
   - Select **Proxies** > choose **MyApp-RDS-Proxy**
   - Click **Actions** > choose **Delete**

   ![](/images/4/0015.png)

   - **To confirm deletion, type delete me into the field**: `delete me`
   - Click **Delete**, this may take a few minutes.

   ![](/images/4/0016.png)

---

#### 6. Clean up IAM Role
- Go to [IAM](https://console.aws.amazon.com/iam/)
   - Select **Roles** > choose **codeDeployServiceRole**, **ecsTaskExecutionRole**, and **rdsProxyServiceRole**
   - Click **Delete**

   ![](/images/4/0017.png)

   - **To confirm deletion, enter delete in the text input field.**: `delete`
   - Click **Delete**

   ![](/images/4/0018.png)

   - Select **Policies** > choose **rdsProxyAccessPolicy**
   - Click **Delete**

   ![](/images/4/0019.png)

   - **To confirm deletion, enter the policy name in the text input field.**: `rdsProxyAccessPolicy`
   - Click **Delete**

   ![](/images/4/0020.png)

   - Select **ecsTaskRDSAccessPolicy**
   - Click **Delete**

   ![](/images/4/0021.png)

   - **To confirm deletion, enter the policy name in the text input field.**: `ecsTaskRDSAccessPolicy`
   - Click **Delete**

   ![](/images/4/0022.png)

---

#### 7. Clean up Secrets Manager
- Go to [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
   - Open **MyAPP-Secret**
   - Click **Actions** > choose **Delete secret**

   ![](/images/4/0023.png)

   - Click **Schedule deletion**

---

#### 8. Clean up RDS Instance
- Go to [AWS RDS](https://console.aws.amazon.com/rds/)
   - Select **Databases** > choose **MyApp-RDS**
   - Click **Actions** > choose **Delete**

   ![](/images/4/0024.png)

   - **To confirm deletion, type delete me into the field.**: `delete me`
   - Click **Delete**, this will take a few minutes.

   ![](/images/4/0025.png)

   - Select **Subnet groups** > choose **MyApp-RDSSG**
   - Click **Delete**

   ![](/images/4/0039.png)

---

#### 9. Clean up Security Groups
- Go to [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Select **Security groups** > choose **SG-ALB**, **SG-ECS**, **SG-RDSProxy**, and **SG-RDS**
   - Click **Actions** > choose **Delete security group**

   ![](/images/4/0026.png)

   - **To confirm deletion, enter delete below.**: `delete`
   - Click **Delete**

   ![](/images/4/0027.png)

---

#### 10. Clean up Internet Gateway and NAT Gateway
- Go to [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Select **NAT gateways** > choose **MyApp-NAT**
   - Click **Actions** > choose **Delete NAT gateway**

   ![](/images/4/0031.png)

   - **To confirm deletion, type delete in the field:**: `delete`
   - Click **Delete**, this will take a few minutes.

   ![](/images/4/0032.png)

   - Select **Elastic IPs** > choose the generated **Elastic IP**
   - Click **Actions** > choose **Release Elastic IP addresses**
   - Click **Release**

   ![](/images/4/0033.png)

   - Select **Internet gateways** > choose **MyApp-IGW**
   - Click **Actions** > choose **Detach from VPC**
   - Click **Detach internet gateway**

   ![](/images/4/0029.png)

   - Select **MyApp-IGW**
   - Click **Actions** > choose **Delete internet gateway**

   ![](/images/4/0034.png)

   - **To confirm deletion, type delete in the field:**: `delete`
   - Click **Delete internet gateway**

   ![](/images/4/0035.png)

---

#### 11. Clean up VPC and Subnet
- Go to [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Select **Subnets** > choose **PublicSubnet1**, **PublicSubnet2**, **PrivateSubnet1**, and **PrivateSubnet2**
   - Click **Actions** > choose **Delete subnet**

   ![](/images/4/0030.png)

   - **To confirm deletion, type delete in the field**: `delete`
   - Click **Delete**

   - Select **Your VPCs** > choose **MyApp-VPC**
   - Click **Actions** > choose **Delete VPC**

   ![](/images/4/0036.png)

   - **To confirm deletion, type delete in the field:**: `delete`
   - Click **Delete**

   ![](/images/4/0037.png)

---
