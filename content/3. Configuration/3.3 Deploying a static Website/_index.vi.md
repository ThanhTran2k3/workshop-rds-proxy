---
title : "Triển khai static website"
date : 2025-08-07
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Chuẩn bị 

- Truy cập [EC2](https://console.aws.amazon.com/ec2/)
    - Chọn **Load Balancers** > Mở **MyApp-ALB**
    - Ghi nhớ giá trị **Load balancer ARN**
  
  ![](/images/3.3/0003.png)

- Truy cập [Git](https://git-scm.com/downloads)
- Tải về rồi tiến hành cài đặt
- Sau khi cài xong, mở **Git Bash** hoặc **Command Prompt** để sử dụng.

  ```bash
    git clone https://github.com/ThanhTran2k3/workshop_rds_proxy_FE.git
    cd workshop_rds_proxy_FE
  ```
- Mở file **config.json**

  ```bash
    notepad config.json
  ```

- Thay đổi giá trị **API_URL**: giá trị **Load balancer ARN**

  ![](/images/3.3/0004.png)

- Lưu lại file **config.json**.

---

#### 1. Mở dịch vụ S3

- Tìm kiếm và chọn dịch vụ **S3**

  ![](/images/3.3/0001.png)

---

#### 2. Tạo Bucket

- Vào **Amazon S3** > chọn **General purpose buckets**
- Nhấn **Create bucket** 

  ![](/images/3.3/0002.png)

- Nhập thông tin:
    - Phần **General configuration**:
      - **Bucket name**: `myapp-s3-3291`
      - ⚠️ **Lưu ý:** Bucket name phải **duy nhất**, không được trùng với bất kỳ tên bucket nào khác trên AWS và không được viết hoa.

  ![](/images/3.3/0005.png)

    - Phần **Block Public Access settings for this bucket**:
      - Bỏ chọn **Block all public access**
      - Chọn **I acknowledge that the current settings might result in this bucket and the objects within becoming public.**
    
  ![](/images/3.3/0006.png)

- Nhấn **Create bucket**

  ![](/images/3.3/0007.png)

---

#### 3. Upload file

- Vào **Amazon S3** > chọn **General purpose buckets**
- Mở **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Chọn **Upload**

  ![](/images/3.3/0009.png)

- Add các file trong thư mục **workshop_rds_proxy_FE**

  ![](/images/3.3/0010.png)

- Nhấn **Upload**

  ![](/images/3.3/0011.png)

---

#### 4. Cấu hình bucket cho website hosting

- Mở **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Vào tab **Properties**

  ![](/images/3.3/0012.png)

- Phần **Static website hosting** > nhấn **Edit**

  ![](/images/3.3/0013.png)

  - Nhập thông tin:
    - **Static website hosting** > chọn **Enable**
    - **Hosting type** > chọn **Host a static website**
    - **Index document** > nhập `index.html`
    - **Error document** > nhập `index.html`
  - Nhấn **Save changes**

  ![](/images/3.3/0014.png)

  ![](/images/3.3/0015.png)

---

#### 5. Cấp quyền public cho website

- Mở **myapp-s3-3291**

  ![](/images/3.3/0008.png)

- Vào tab **Permissions**

  ![](/images/3.3/0016.png)

- Phần **Bucket policy** > nhấn chọn **Edit**

  ![](/images/3.3/0017.png)

- Dán đoạn JSON vào **Policy**
 
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
- Nhấn **Save changes**

  ![](/images/3.3/0018.png)

- Vào tab **Properties**

  ![](/images/3.3/0012.png)

- Dán **Bucket website endpoint** vào trình duyệt để kiểm tra

  ![](/images/3.3/0019.png)

  ![](/images/3.3/0020.png)



---



