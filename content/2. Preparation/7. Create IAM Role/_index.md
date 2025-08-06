---
title : "Create IAM Role"
date : 2025-08-07
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### 🛠️ Steps to Implement

#### 1. Create Policies

##### 1.1. Create Policy for ECS Task

- Search and select the **IAM** service.

  ![](/images/2.7/0001.png)

- Go to **Identity and Access Management (IAM)** > select **Policies**
- Click **Create policy**.

  ![](/images/2.7/0002.png)

- In the **Specify permissions** step:
  - Switch to the JSON tab and paste the following content:
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

- Click **Next** to continue.

  ![](/images/2.7/0004.png)

- In the **Review and create** step:
  - Enter:
    - **Policy name**: `ecsTaskRDSAccessPolicy`

  ![](/images/2.7/0005.png)

- Click **Create policy**.

  ![](/images/2.7/0006.png)

##### 1.2. Create Policy for RDS Proxy

- Go to **Identity and Access Management (IAM)** > select **Policies**
- Click **Create policy**.

  ![](/images/2.7/0002.png)

- In the **Specify permissions** step:
  - Switch to the JSON tab and paste the following content:
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

- Click **Next** to continue.

  ![](/images/2.7/0014.png)

- In the **Review and create** step:
  - Enter:
    - **Policy name**: `rdsProxyAccessPolicy`

  ![](/images/2.7/0015.png)

- Click **Create policy**.

  ![](/images/2.7/0006.png)

---

#### 2. Create Role for ECS Task

- Go to **Identity and Access Management (IAM)** > select **Roles**
- Click **Create role**.

  ![](/images/2.7/0007.png)

- In the **Select trusted entity** step:
  - Select **AWS service**.

  ![](/images/2.7/0008.png)

  - In **Use case**:
    - **Service or use case**: Select **Elastic Container Service**
    - **Use case**: Select **Elastic Container Service Task**
  - Click **Next**.

  ![](/images/2.7/0009.png)

- In the **Add permissions** step:
  - Search for `ecsTaskRDSAccessPolicy` > select it.
  - Click **Next**.

  ![](/images/2.7/0010.png)

- In the **Name, review, and create** step:
  - **Role name**: `ecsTaskExecutionRole`
  - **Description**: `Secrets Manager and RDS Proxy access permissions for ECS Task`

  ![](/images/2.7/0011.png)

- Click **Create role**.

  ![](/images/2.7/0012.png)

---

#### 3. Create CodeDeploy Role

- Go to **Identity and Access Management (IAM)** > select **Roles**
- Click **Create role**.

  ![](/images/2.7/0007.png)

- In the **Select trusted entity** step:
  - Select **AWS service**.

  ![](/images/2.7/0008.png)

  - In **Use case**:
    - **Service or use case**: Select **CodeDeploy**
    - **Use case**: Select **CodeDeploy - ECS**
  - Click **Next**.

  ![](/images/2.7/0013.png)

- In the **Add permissions** step:
  - Click **Next**.

- In the **Name, review, and create** step:
  - **Role name**: `codeDeployServiceRole`
  - **Description**: `Allows AWS CodeDeploy to manage deployments to Amazon ECS services, including creating and updating task definitions and ECS service configurations.`

  ![](/images/2.7/0019.png)

- Click **Create role**.

  ![](/images/2.7/0012.png)

---

#### 4. Create Role for RDS Proxy

- Go to **Identity and Access Management (IAM)** > select **Roles**
- Click **Create role**.

  ![](/images/2.7/0007.png)

- In the **Select trusted entity** step:
  - Select **AWS service**.

  ![](/images/2.7/0008.png)

  - In **Use case**:
    - **Service or use case**: Select **RDS**
    - **Use case**: Select **RDS - Add Role to Database**
  - Click **Next**.

  ![](/images/2.7/0016.png)

- In the **Add permissions** step:
  - Search for `rdsProxyAccessPolicy` > select it.
  - Click **Next**.

  ![](/images/2.7/0017.png)

- In the **Name, review, and create** step:
  - **Role name**: `rdsProxyServiceRole`
  - **Description**: `Role used by Amazon RDS Proxy to retrieve database credentials from AWS Secrets Manager`

  ![](/images/2.7/0018.png)

- Click **Create role**.

  ![](/images/2.7/0012.png)

---