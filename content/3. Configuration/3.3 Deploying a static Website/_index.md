---
title : "Deploying a static Website"
date : 2025-08-07
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

### 🛠️ Steps to Implement

#### 1. Preparation

- Go to [EC2](https://console.aws.amazon.com/ec2/)
    - Select **Load Balancers** > Open **MyApp-ALB**
    - Note the value of **Load balancer ARN**
  
  ![](/images/3.3/0003.png)

- Visit [Git](https://git-scm.com/downloads)
- Download and install Git
- After installation, open **Git Bash** or **Command Prompt**.

  ```bash
    git clone https://github.com/ThanhTran2k3/workshop_rds_proxy_FE.git
    cd workshop_rds_proxy_FE
  ```
- Open the **config.json** file

  ```bash
    notepad config.json
  ```

- Update the **API_URL** value: use the **Load balancer ARN**

  ![](/images/3.3/0004.png)

- Save the **config.json** file.

---

#### 2. Open S3 Service

- Search for and select **S3**

  ![](/images/3.3/0001.png)

---

#### 3. Create a Bucket

- Go to **Amazon S3** > select **General purpose buckets**
- Click **Create bucket**

  ![](/images/3.3/0002.png)

- Enter the following details:
    - Under **General configuration**:
      - **Bucket name**: `myapp-s3-3291`
      - ⚠️ **Note:** The bucket name must be **unique** across AWS and must not contain uppercase letters.

  ![](/images/3.3/0005.png)

    - Under **Block Public Access settings for this bucket**:
      - Uncheck **Block all public access**
      - Check **I acknowledge that the current settings might result in this bucket and the objects within becoming public.**
    
  ![](/images/3.3/0006.png)

- Click **Create bucket**

  ![](/images/3.3/0007.png)

---

#### 4. Upload Files

- Go to **Amazon S3** > select **General purpose buckets**
- Open **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Click **Upload**

  ![](/images/3.3/0009.png)

- Add files from the **workshop_rds_proxy_FE** folder

  ![](/images/3.3/0010.png)

- Click **Upload**

  ![](/images/3.3/0011.png)

---

#### 5. Configure Bucket for Website Hosting

- Open **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Go to the **Properties** tab

  ![](/images/3.3/0012.png)

- Under **Static website hosting** > click **Edit**

  ![](/images/3.3/0013.png)

  - Enter the following details:
    - **Static website hosting** > select **Enable**
    - **Hosting type** > select **Host a static website**
    - **Index document**: `index.html`
    - **Error document**: `index.html`
  - Click **Save changes**

  ![](/images/3.3/0014.png)

  ![](/images/3.3/0015.png)

---

#### 6. Grant Public Access to the Website

- Open **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Go to the **Permissions** tab

  ![](/images/3.3/0016.png)

- Under **Bucket policy** > click **Edit**

  ![](/images/3.3/0017.png)

- Paste the following JSON into the **Policy**
 
  ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "PublicReadGetObject",
          "Effect": "Allow",
          "Principal": "*",
          "Action": "s3:GetObject",
          "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
      ]
    }
  ```
- Click **Save changes**

  ![](/images/3.3/0018.png)

- Go to the **Properties** tab

  ![](/images/3.3/0012.png)

- Paste the **Bucket website endpoint** into a browser to verify

  ![](/images/3.3/0019.png)

  ![](/images/3.3/0020.png)
