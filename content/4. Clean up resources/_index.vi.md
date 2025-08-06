---
title : "Dọn dẹp tài nguyên"
date : 2025-08-07
pre : " <b> 4. </b> "
---

### Tài nguyên dọn dẹp
ℹ️ **Thông tin**: Việc dọn dẹp tài nguyên AWS đúng cách sau khi hoàn thành bài lab là điều cần thiết để tránh phát sinh chi phí bất ngờ cho tài khoản AWS của bạn. Hãy làm theo các bước sau để đảm bảo tất cả tài nguyên đã được chấm dứt và xóa.

### 🛠️ Các bước thực hiện

#### 1. Dọn dẹp CloudFront

- Truy cập [AWS CloudFont](https://console.aws.amazon.com/cloudfront/)
   - Chọn **Distributions** > chọn **distribution** đã tạo
   - Nhấn chọn **Disable**, thao tác này sẽ mất vài phút.
   - Sau khi tắt hoàn tất nhấn chọn **Delete**

   ![](/images/4/0001.png)

   ![](/images/4/0002.png)

   - Chọn **Origin access** > chọn **Origin access controls** đã sinh
   - Nhấn chọn **Delete**

   ![](/images/4/0038.png)

---

#### 2. Dọn dẹp S3 bucket
- Truy cập [AWS S3](https://console.aws.amazon.com/s3/)
   - Chọn **General purpose buckets** > mở **bucket** đã tạo
   - Chọn tất cả file trong bucket
   - Nhấn chọn **Delete**
   - **To confirm deletion, type permanently delete in the text input field.**: `permanently delete`
   - Nhấn chọn **Delete objects**

   ![](/images/4/0004.png)

   ![](/images/4/0005.png)

   - Quay về **General purpose buckets** > chọn **bucket** đã tạo
   - Nhấn chọn **Delete**
   - **To confirm deletion, enter the name of the bucket in the text input field.**: `your-bucket-name`
   - Nhấn chọn **Delete bucket**

   ![](/images/4/0003.png)

   ![](/images/4/0006.png)

---

#### 3. Dọn dẹp ECS
- Truy cập [AWS ECS](https://console.aws.amazon.com/ecs/)
   - Chọn **Clusters** > mở **Myapp-Cluster**
   - Trong tab **Services**
   - Chọn **MyApp-Service** > Nhấn chọn **Delete service**

   ![](/images/4/0007.png)

   - Chọn **Force delete**
   - **To confirm deletion, enter delete in the text input field.**: `delete`
   - Nhấn chọn **Delete**

   ![](/images/4/0008.png)

   - Nhấn chọn **Delete cluster**

   ![](/images/4/0009.png)

   - **Enter the phrase "delete Myapp-Cluster" to confirm deletion.**: `delete Myapp-Cluster`
   - Nhấn chọn **Delete**, thao tác này sẽ mất vài phút.

   ![](/images/4/0010.png)

   - Chọn **Task definitions** > mở **MyApp-Task**
   - Chọn tất cả **Task definition: revision**
   - Chọn **Actions** > nhấn chọn **Deregister**

   ![](/images/4/0011.png)

---

#### 4. Dọn dẹp Application Load Balancer và Target Groups
- Truy cập [EC2](https://console.aws.amazon.com/ec2/)
   - Chọn **Load Balancers** > chọn **MyApp-ALB**
   - Nhấn chọn **Actions** > chọn **Delete load balancer**

   ![](/images/4/0012.png)

   - **Type confirm to agree.**: `confirm`
   - Nhấn chọn **Delete**

   ![](/images/4/0013.png)

   - Chọn **Target Groups** > chọn **MyApp-TG**
   - Nhấn chọn **Actions** > chọn **Delete**

   ![](/images/4/0014.png)
  
---

#### 5. Dọn dẹp RDS Proxy
- Truy cập [AWS RDS](https://console.aws.amazon.com/rds/)
   - Chọn **Proxies** > chọn **MyApp-RDS-Proxy**
   - Nhấn chọn **Actions** > chọn **Delete**

   ![](/images/4/0015.png)

   - **To confirm deletion, type delete me into the field**: `delete me`
   - Nhấn chọn **Delete**, thao tác này sẽ mất vài phút.

   ![](/images/4/0016.png)

---

#### 6. Dọn dẹp IAM Role
- Truy cập [IAM](https://console.aws.amazon.com/iam/)
   - Chọn **Roles** > chọn **codeDeployServiceRole**, **ecsTaskExecutionRole** và **rdsProxyServiceRole**
   - Nhấn chọn **Delete**

   ![](/images/4/0017.png)

   - **To confirm deletion, enter delete in the text input field.**: `delete`
   - Nhấn chọn **Delete**

   ![](/images/4/0018.png)

   - Chọn **Policies** >  chọn **rdsProxyAccessPolicy**
   - Nhấn chọn **Delete**

   ![](/images/4/0019.png)

   - **To confirm deletion, enter the policy name in the text input field.**: `rdsProxyAccessPolicy `
   - Nhấn chọn **Delete**

   ![](/images/4/0020.png)

   - Chọn **ecsTaskRDSAccessPolicy**
   - Nhấn chọn **Delete**

   ![](/images/4/0021.png)

   - **To confirm deletion, enter the policy name in the text input field.**: `ecsTaskRDSAccessPolicy `
   - Nhấn chọn **Delete**

   ![](/images/4/0022.png)

---

#### 7. Dọn dẹp Secrets Manager
- Truy cập [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/)
   - Mở **MyAPP-Secret** 
   - Nhấn chọn **Actions** > chọn **Delete secret**

   ![](/images/4/0023.png)

   - Nhấn chọn **Schedule deletion**

---

#### 8. Dọn dẹp RDS Instance
- Truy cập [AWS RDS](https://console.aws.amazon.com/rds/)
   - Chọn **Databases** > chọn **MyApp-RDS**
   - Nhấn chọn **Actions** > chọn **Delete**

   ![](/images/4/0024.png)

   - **To confirm deletion, type delete me into the field.**: `delete me`
   - Nhấn chọn **Delete**, thao tác này sẽ mất vài phút.

   ![](/images/4/0025.png)

   - Chọn **Subnet groups** > chọn **MyApp-RDSSG**
   - Nhấn chọn **Delete**

   ![](/images/4/0039.png)

---

#### 9. Dọn dẹp Security Group
- Truy cập [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Chọn **Security groups** > chọn **SG-ALB**, **SG-ECS**, **SG-RDSProxy** và **SG-RDS**
   - Nhấn chọn **Actions** > chọn **Delete security group**

   ![](/images/4/0026.png)

   - **To confirm deletion, enter delete below.**: `delete`
   - Nhấn chọn **Delete**

   ![](/images/4/0027.png)

---

#### 10. Dọn dẹp Internet Gateway và NAT Gateway
- Truy cập [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Chọn **NAT gateways** > chọn **MyApp-NAT**
   - Nhấn chọn **Actions** > chọn **Delete NAT gateway**
   
   ![](/images/4/0031.png)

   - **To confirm deletion, type delete in the field:**: `delete`
   - Nhấn chọn **Delete**, thao tác này sẽ mất vài phút.

   ![](/images/4/0032.png)

   - Chọn **Elastic IPs** > chọn **Elastic IP** đã được sinh ra
   - Nhấn chọn **Actions** > chọn **Release Elastic IP addresses**
   - Nhấn chọn **Release**

   ![](/images/4/0033.png)

   - Chọn **Internet gateways** > chọn **MyApp-IGW**
   - Nhấn chọn **Actions** > chọn **Detach from VPC**
   - Nhấn chọn **Detach internet gateway**
   
   ![](/images/4/0029.png)

   - Chọn **MyApp-IGW**
   - Nhấn chọn **Actions** > chọn **Delete internet gateway**

   ![](/images/4/0034.png)

   - **To confirm deletion, type delete in the field:**: `delete `
   - Nhấn chọn **Delete internet gateway**

   ![](/images/4/0035.png)


---

#### 11. Dọn dẹp VPC và Subnet
- Truy cập [AWS VPC](https://console.aws.amazon.com/vpc/)
   - Chọn **Subnets** > chọn **PublicSubnet1**, **PublicSubnet2**, **PrivateSubnet1** và **PrivateSubnet2**
   - Nhấn chọn **Actions** > chọn **Delete subnet**
   
   ![](/images/4/0030.png)

   - **To confirm deletion, type delete in the field**: `delete`
   - Nhấn chọn **Delete**

   - Chọn **Your VPCs** > chọn **MyApp-VPC**
   - Nhấn chọn **Actions** > chọn **Delete VPC**

   ![](/images/4/0036.png)

   - **To confirm deletion, type delete in the field:**: `delete `
   - Nhấn chọn **Delete**

   ![](/images/4/0037.png)

---


