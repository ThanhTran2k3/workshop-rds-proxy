---
title : "Cấu hình CloudFront"
date : 2025-08-07
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Chặn tất cả truy cập công cộng vào S3

- Mở **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Vào tab **Permissions**
- Phần **Block public access (bucket settings)** > nhấn chọn **Edit**

  ![](/images/3.4/0001.png)

- Chọn **Block all public access**
- Nhấn **Save changes**

  ![](/images/3.4/0002.png)

#### 2. Mở dịch vụ Amazon CloudFront
- Tìm kiếm và chọn dịch vụ **CloudFront**

  ![](/images/3.4/0003.png)

---

#### 3. Tạo Distribution
- Vào **Amazon CloudFront** > chọn **Distributions**
- Nhấn **Create distribution**

  ![](/images/3.4/0004.png)

- Nhập thông tin:
  - Bước **Get started**:
    - **Distribution name**: `MyApp-Distribution`
    - Nhấn **Next** để tiếp tục 
  
  ![](/images/3.4/0005.png)

  - Bước **Configure distribution**:
    - **Origin type** > chọn **Amazon S3**
    - **S3 origin** > nhấn chọn **Browse S3** chọn **myapp-s3-3291**

  ![](/images/3.4/0006.png)

  ![](/images/3.4/0007.png)

    - Bấm **Next** để tiếp tục 

  ![](/images/3.4/0008.png)

  - Bước **Enable security**:
    - **Web Application Firewall (WAF)** > chọn **Do not enable security protections**
    - Nhấn **Next** để tiếp tục

  ![](/images/3.4/0009.png)

  - Bước **Review and create**:
    - Nhấn **Create distribution** để tạo 
  
  ![](/images/3.4/0010.png)

### 4. Cập nhập Distribution
- Vào **Amazon CloudFront** > chọn **Distributions**
- Mở **distribution** vừa tạo
 
  ![](/images/3.4/0013.png)

- Vào tab **Origins**:
  - Chọn **http://myapp-s3-3291.s3-website-ap-southeast-1.amazonaws.com**
  - Nhấn **Edit**

  ![](/images/3.4/0014.png)

  - Phần **Settings**:
    - **Origin access** > chọn **Origin access control settings (recommended)**
    - Nhấn chọn **Create new OAC**
  
  ![](/images/3.4/0015.png)

    - Nhấn **Create**
  
  ![](/images/3.4/0016.png)

    - Nhấn **Save changes**

  ![](/images/3.4/0017.png)

- Vào tab **General**:
  - Nhấn chọn **Edit**

  ![](/images/3.4/0018.png)

  - Phần **Settings**:
    - **Default root object - optional**: `index.html`
    - Nhấn **Save changes**

  ![](/images/3.4/0019.png)

- Vào tab **Error pages**:
  - Nhấn chọn **Create custom error response**

  ![](/images/3.4/0026.png)

  - Phần **Error response**:
    - **HTTP error code** > chọn **403: Forbidden**
    - **Customize error response** > chọn **Yes**
    - **Response page path**: `/index.html`
    - **HTTP Response code** > chọn **200: OK**
    - Nhấn **Create custom error response**

  ![](/images/3.4/0027.png)

#### 5. Cập nhập S3 Policy

- Vào **Amazon S3** > chọn **General purpose buckets**
- Mở **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Vào tab **Permissions**

  ![](/images/3.3/0016.png)

- Phần **Bucket policy** > nhấn chọn **Edit**

  ![](/images/3.4/0011.png)

- Dán đoạn JSON vào **Policy**

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

- Nhấn **Save changes**

  ![](/images/3.4/0012.png)

---

#### 6. Cấu hình truy cập AWS ALB qua CloudFront
- Vào **Amazon CloudFront** > chọn **Distributions**
- Mở **distribution** vừa tạo
 
  ![](/images/3.4/0013.png)

- Vào tab **Origins**:
  - Nhấn **Create origin**

  ![](/images/3.4/0028.png)

  - Trong **Settings**
    - **Origin domain** > chọn **MyApp-ALB**
    - **Protocol** > chọn **HTTP only**  
    - **HTTP port**: `80`
    - Nhấn **Create origin** để tạo 

  ![](/images/3.4/0029.png)

  ![](/images/3.4/0030.png)

- Vào tab **Behaviors**:
  - Nhấn **Create behavior**

  ![](/images/3.4/0031.png)

  - Trong **Settings**
    - **Path pattern** > chọn **/api/***
    - **Origin and origin groups** > chọn **MyApp-ALB**  
    - **Viewer protocol policy** > chọn **Redirect HTTP to HTTPS**
    - **Allowed HTTP methods** > chọn **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**
    - **Cache policy** > chọn **CachingDisabled**
    - **Origin request policy - optional** > chọn **AllViewerExceptHostHeader**
    - Nhấn **Create behavior** để tạo 

  ![](/images/3.4/0032.png)

  ![](/images/3.4/0033.png)

  ![](/images/3.4/0034.png)

---

#### 7. Cập nhật file config.json
- Chuẩn bị:
  - Truy cập [AWS Cloud Front](https://console.aws.amazon.com/cloudfront/)
    - Chọn **Distributions** > Mở **distribution** vừa tạo
    - Ghi nhớ giá trị **Distribution domain name**
  
  ![](/images/3.4/0035.png)

- Mở **Amazon S3** > chọn **General purpose buckets**
- Mở **myapp-s3-3291**
- Trong tab **Objects**
  - Chọn file **config.json** > nhấn chọn **Download**

  ![](/images/3.4/0036.png)

- Mở file **config.json** đã tải về 
- Thay đổi giá trị **API_URL**: giá trị **Distribution domain name**

  ![](/images/3.4/0037.png)

- Lưu file và upload lên S3 lại

- Trong tab **Objects**
  - Nhấn chọn **Upload**

  ![](/images/3.4/0038.png)

  - Chọn file **config.json** đã sửa
  - Nhấn **Upload** để tải lên

  ![](/images/3.4/0039.png)

---

#### 8. Kiểm tra Website
- Dán **Distribution domain name** vào trình duyệt để kiểm tra

  ![](/images/3.4/0021.png)

- Kiểm tra một số tính năng gọi đến backend trên ecs
 
  ![](/images/3.4/0022.png)

  ![](/images/3.4/0023.png)

  ![](/images/3.4/0024.png)

  ![](/images/3.4/0025.png)

---

