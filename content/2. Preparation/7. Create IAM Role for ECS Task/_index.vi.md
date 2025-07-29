---
title : "Tạo IAM Role cho ECS Task"
date :  2025-07-03
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### 🛠️ Các bước thực hiện
#### 1. Tạo Policy

- Tìm và chọn dịch vụ **IAM**

   ![](/images/2.7/0001.png)

- Vào **Identity and Access Management (IAM)** > chọn **Policies**
- Nhấn **Create policy**.

   ![](/images/2.7/0002.png)

- Ở bước **Specify permissions**
   - Chuyển sang tab JSON và dán nội dung sau:
   ```json
   {
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
         "secretsmanager:GetSecretValue",
         "rds-db:connect",
         "logs:CreateLogStream",
         "logs:PutLogEvents"
         ],
         "Resource": "*"
      }
   ]
   }
   ```

   ![](/images/2.7/0003.png)

   - Nhấn **Next** để tiếp tục

   ![](/images/2.7/0004.png)

- Ở bước **Review and create**
   - Nhập thông tin:
      - **Policy name**: `ecsTaskRDSAccessPolicy`

   ![](/images/2.7/0005.png)

- Nhấn **Create policy**

   ![](/images/2.7/0006.png)

---

#### 2. Tạo Role

- Vào **Identity and Access Management (IAM)** > chọn **Roles**
- Nhấn **Create role**.

   ![](/images/2.7/0007.png)

- Ở bước **Select trusted entity**
   - Click chọn **AWS service**

   ![](/images/2.7/0008.png)

   - Ở phần **Use case**
      - **Service or use case**: chọn **Elastic Container Service**
      - **Use case**: chọn **Elastic Container Service Task**
   - Nhấn **Next** để tiếp tục.

   ![](/images/2.7/0009.png)

- Ở bước **Add permissions**
   - Tìm kiếm `ecsTaskRDSAccessPolicy` > click chọn
   - Nhấn **Next** để tiếp tục.

   ![](/images/2.7/0010.png)

- Ở bước **Name, review, and create**
   - Nhập thông tin:
      - **Role name**: `ecsTaskExecutionRole`
      - **Description**: `Secrets Manager and RDS Proxy access permissions for ECS Task`

   ![](/images/2.7/0011.png)

- Nhấn **Create role**

   ![](/images/2.7/0012.png)
