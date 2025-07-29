---
title : "Tạo Secrets Manager"
date :  2025-07-03
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

### 🛠️ Các bước thực hiện

#### 1️. Truy cập AWS Management Console

- Truy cập [AWS Management Console](https://console.aws.amazon.com/)

   ![](/images/2.1/0001.png)

- Tìm và chọn dịch vụ **Secrets Manager**

   ![](/images/2.1/0002.png)

- Nhấn chọn **Store a new secret**

---

#### 2. Chọn loại secret

- Tại bước **Choose secret type**:
  - Chọn **Credentials for RDS database**
  - Nhập thông tin đăng nhập cho database:
    - **Username**: `admin`
    - **Password**: `your-password`

✅ **Hoặc** chọn **Other type of secrets** và nhập thủ công dữ liệu JSON:

```json
{
  "username": "admin",
  "password": "your-password"
}
```

---

#### 3. Chọn loại secret
