---
title : "Configure CloudFront"
date : 2025-08-07
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

### 🛠️ Steps to Implement

#### 1. Block All Public Access to S3

- Open **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Go to the **Permissions** tab  
- In the **Block public access (bucket settings)** section > click **Edit**

  ![](/images/3.4/0001.png)

- Select **Block all public access**  
- Click **Save changes**

  ![](/images/3.4/0002.png)

#### 2. Open Amazon CloudFront Service

- Search for and select **CloudFront**

  ![](/images/3.4/0003.png)

---

#### 3. Create a Distribution

- Go to **Amazon CloudFront** > choose **Distributions**  
- Click **Create distribution**

  ![](/images/3.4/0004.png)

- Enter the following details:
  - **Get started** step:
    - **Distribution name**: `MyApp-Distribution`
    - Click **Next** to continue

  ![](/images/3.4/0005.png)

  - **Configure distribution** step:
    - **Origin type** > select **Amazon S3**
    - **S3 origin** > click **Browse S3**, choose **myapp-s3-3291**

  ![](/images/3.4/0006.png)  
  ![](/images/3.4/0007.png)

    - Click **Next** to continue

  ![](/images/3.4/0008.png)

  - **Enable security** step:
    - **Web Application Firewall (WAF)** > choose **Do not enable security protections**
    - Click **Next** to continue

  ![](/images/3.4/0009.png)

  - **Review and create** step:
    - Click **Create distribution** to create

  ![](/images/3.4/0010.png)

---

### 4. Update Distribution

- Go to **Amazon CloudFront** > choose **Distributions**  
- Open the newly created distribution

  ![](/images/3.4/0013.png)

- Go to the **Origins** tab:
  - Select **http://myapp-s3-3291.s3-website-ap-southeast-1.amazonaws.com**
  - Click **Edit**

  ![](/images/3.4/0014.png)

  - In **Settings**:
    - **Origin access** > choose **Origin access control settings (recommended)**
    - Click **Create new OAC**

  ![](/images/3.4/0015.png)

    - Click **Create**

  ![](/images/3.4/0016.png)

    - Click **Save changes**

  ![](/images/3.4/0017.png)

- Go to the **General** tab:
  - Click **Edit**

  ![](/images/3.4/0018.png)

  - In **Settings**:
    - **Default root object - optional**: `index.html`
    - Click **Save changes**

  ![](/images/3.4/0019.png)

- Go to the **Error pages** tab:
  - Click **Create custom error response**

  ![](/images/3.4/0026.png)

  - In **Error response**:
    - **HTTP error code** > choose **403: Forbidden**
    - **Customize error response** > choose **Yes**
    - **Response page path**: `/index.html`
    - **HTTP Response code** > choose **200: OK**
    - Click **Create custom error response**

  ![](/images/3.4/0027.png)

---

#### 5. Update S3 Bucket Policy

- Go to **Amazon S3** > choose **General purpose buckets**  
- Open **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Go to the **Permissions** tab

  ![](/images/3.3/0016.png)

- In the **Bucket policy** section > click **Edit**

  ![](/images/3.4/0011.png)

- Paste the following JSON into **Policy**:

   ```json
   {
   "Version": "2008-10-17",
   "Id": "PolicyForCloudFrontPrivateContent",
   "Statement": [
      {
         "Sid": "AllowCloudFrontServicePrincipal",
         "Effect": "Allow",
         "Principal": {
         "Service": "cloudfront.amazonaws.com"
         },
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*",
         "Condition": {
         "StringEquals": {
            "AWS:SourceArn": "arn:aws:cloudfront::329178086719:distribution/your-distributions-ID"
         }
         }
      }
   ]
   }
   ```
- Click **Save changes**

  ![](/images/3.4/0012.png)

---

#### 6. Configure AWS ALB Access via CloudFront
- Go to **Amazon CloudFront** > select **Distributions**
- Open the newly created **distribution**
 
  ![](/images/3.4/0013.png)

- Go to the **Origins** tab:
  - Click **Create origin**

  ![](/images/3.4/0028.png)

  - In **Settings**
    - **Origin domain** > select **MyApp-ALB**
    - **Protocol** > select **HTTP only**  
    - **HTTP port**: `80`
    - Click **Create origin** to proceed

  ![](/images/3.4/0029.png)

  ![](/images/3.4/0030.png)

- Go to the **Behaviors** tab:
  - Click **Create behavior**

  ![](/images/3.4/0031.png)

  - In **Settings**
    - **Path pattern** > choose **/api/***
    - **Origin and origin groups** > select **MyApp-ALB**  
    - **Viewer protocol policy** > choose **Redirect HTTP to HTTPS**
    - **Allowed HTTP methods** > choose **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**
    - **Cache policy** > choose **CachingDisabled**
    - **Origin request policy - optional** > choose **AllViewerExceptHostHeader**
    - Click **Create behavior** to proceed

  ![](/images/3.4/0032.png)

  ![](/images/3.4/0033.png)

  ![](/images/3.4/0034.png)

---

#### 7. Update config.json File
- Preparation:
  - Go to [AWS Cloud Front](https://console.aws.amazon.com/cloudfront/)
    - Select **Distributions** > Open the newly created **distribution**
    - Note down the **Distribution domain name**
  
  ![](/images/3.4/0035.png)

- Open **Amazon S3** > select **General purpose buckets**
- Open **myapp-s3-3291**
- In the **Objects** tab
  - Select the **config.json** file > click **Download**

  ![](/images/3.4/0036.png)

- Open the downloaded **config.json** file
- Replace the value of **API_URL** with the noted **Distribution domain name**

  ![](/images/3.4/0037.png)

- Save the file and re-upload it to S3

- In the **Objects** tab:
  - Click **Upload**

  ![](/images/3.4/0038.png)

  - Select the modified **config.json** file
  - Click **Upload** to upload

  ![](/images/3.4/0039.png)

---

#### 8. Verify the Website
- Paste the **Distribution domain name** into the browser to verify

  ![](/images/3.4/0021.png)

- Check some features that make requests to the backend on ECS
 
  ![](/images/3.4/0022.png)

  ![](/images/3.4/0023.png)

  ![](/images/3.4/0024.png)

  ![](/images/3.4/0025.png)

---