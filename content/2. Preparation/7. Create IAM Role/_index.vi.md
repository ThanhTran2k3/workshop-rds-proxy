---
title : "Tạo IAM Role"
date : 2025-08-07
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### 🛠️ Các bước thực hiện
#### 1. Tạo Policy
##### 1.1. Tạo Policy cho ECS Task

- Tìm và chọn dịch vụ **IAM**

   ![](/images/2.7/0001.png)

- Vào **Identity and Access Management (IAM)** > chọn **Policies**
- Nhấn **Create policy**.

   ![](/images/2.7/0002.png)

- Bước **Specify permissions**
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

- Bước **Review and create**
   - Nhập thông tin:
      - **Policy name**: `ecsTaskRDSAccessPolicy`

   ![](/images/2.7/0005.png)

- Nhấn **Create policy**

   ![](/images/2.7/0006.png)

---

##### 1.2. Tạo Policy cho RDS Proxy

- Vào **Identity and Access Management (IAM)** > chọn **Policies**
- Nhấn **Create policy**.

   ![](/images/2.7/0002.png)

- Bước **Specify permissions**
   - Chuyển sang tab JSON và dán nội dung sau:
   ```json
   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Effect": "Allow",
            "Action": [
                  "secretsmanager:GetSecretValue",
                  "secretsmanager:DescribeSecret"
            ],
            "Resource": "*"
         }
      ]
   }
   ```

   ![](/images/2.7/0003.png)

   - Nhấn **Next** để tiếp tục

   ![](/images/2.7/0014.png)

- Bước **Review and create**
   - Nhập thông tin:
      - **Policy name**: `rdsProxyAccessPolicy`

   ![](/images/2.7/0015.png)

- Nhấn **Create policy**

   ![](/images/2.7/0006.png)


---

#### 2. Tạo Role cho ECS Task

- Vào **Identity and Access Management (IAM)** > chọn **Roles**
- Nhấn **Create role**.

   ![](/images/2.7/0007.png)

- Bước **Select trusted entity**
   - Click chọn **AWS service**

   ![](/images/2.7/0008.png)

   - Phần **Use case**
      - **Service or use case**: chọn **Elastic Container Service**
      - **Use case**: chọn **Elastic Container Service Task**
   - Nhấn **Next** để tiếp tục.

   ![](/images/2.7/0009.png)

- Bước **Add permissions**
   - Tìm kiếm `ecsTaskRDSAccessPolicy` > click chọn
   - Nhấn **Next** để tiếp tục.

   ![](/images/2.7/0010.png)

- Bước **Name, review, and create**
   - **Role name**: `ecsTaskExecutionRole`
   - **Description**: `Secrets Manager and RDS Proxy access permissions for ECS Task`

   ![](/images/2.7/0011.png)

- Nhấn **Create role**

   ![](/images/2.7/0012.png)

---

#### 3. Tạo CodeDeploy Role

- Vào **Identity and Access Management (IAM)** > chọn **Roles**
- Nhấn **Create role**.

   ![](/images/2.7/0007.png)

- Bước **Select trusted entity**
   - Click chọn **AWS service**

   ![](/images/2.7/0008.png)

   - Phần **Use case**
      - **Service or use case**: chọn **CodeDeploy**
      - **Use case**: chọn **CodeDeploy - ECS**
   - Nhấn **Next** để tiếp tục.
   
   ![](/images/2.7/0013.png)

- Bước **Add permissions**
   - Nhấn **Next** để tiếp tục.

- Bước **Name, review, and create**
   - **Role name**: `codeDeployServiceRole`
   - **Description**: `Allows AWS CodeDeploy to manage deployments to Amazon ECS services, including creating and updating task definitions and ECS service configurations.`

   ![](/images/2.7/0019.png)

   - Nhấn **Create role**

   ![](/images/2.7/0012.png)


---

#### 4. Tạo Role cho RDS Proxy

- Vào **Identity and Access Management (IAM)** > chọn **Roles**
- Nhấn **Create role**.

   ![](/images/2.7/0007.png)

- Bước **Select trusted entity**
   - Click chọn **AWS service**

   ![](/images/2.7/0008.png)

   - Phần **Use case**
      - **Service or use case**: chọn **RDS**
      - **Use case**: chọn **RDS - Add Role to Database**
   - Nhấn **Next** để tiếp tục.
   
   ![](/images/2.7/0016.png)

- Bước **Add permissions**
   - Tìm kiếm `rdsProxyAccessPolicy` > click chọn
   - Nhấn **Next** để tiếp tục.

   ![](/images/2.7/0017.png)

- Bước **Name, review, and create**
   - **Role name**: `rdsProxyServiceRole`
   - **Description**: `Role used by Amazon RDS Proxy to retrieve database credentials from AWS Secrets Manager `

   ![](/images/2.7/0018.png)

   - Nhấn **Create role**

   ![](/images/2.7/0012.png)

---