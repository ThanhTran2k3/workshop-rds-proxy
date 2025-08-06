---
title : "Create Secrets Manager"
date : 2025-08-07
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

### 🛠️ Steps to Follow

#### 1. Open Secrets Manager Service

- Search and select **Secrets Manager**

  ![](/images/2.6/0001.png)

---

#### 2. Create a Secret

- Go to **AWS Secrets Manager** > select **Secrets**
- Click **Store a new secret**

  ![](/images/2.6/0002.png)

- In the **Choose secret type** step:
  - **Secret type** > select **Credentials for RDS database**
  - Under **Credentials**:
    - **User name**: `admin`
    - **Password**: `your-password`

  ![](/images/2.6/0003.png)

    ✅ **Or** select **Other type of secrets** and manually enter JSON data:

    ```json
    {
      "username": "admin",
      "password": "your-password"
    }
    ```
  - Under **Database**, select **MyApp-RDS**

  - Click **Next** to continue

  ![](/images/2.6/0004.png)

---

- In the **Configure secret** step:
  - **Secret name**: `MyAPP-Secret`
  - **Description**: `MySQL database credentials`

  ![](/images/2.6/0005.png)

  - Click **Next** to continue

  ![](/images/2.6/0006.png)

---

- In the **Configure rotation** step:
  - Click **Next** to continue

  ![](/images/2.6/0007.png)
---

- In the **Review** step:
  - Click **Store** to create the secret

  ![](/images/2.6/0008.png)

---