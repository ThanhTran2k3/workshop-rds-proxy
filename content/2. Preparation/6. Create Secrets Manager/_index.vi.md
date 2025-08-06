---
title : "Tạo Secrets Manager"
date : 2025-08-07
weight : 6
chapter : false
pre : " <b> 2.6 </b> "
---

### 🛠️ Các bước thực hiện

#### 1. Mở dịch vụ Secrets Manager

- Tìm kiếm và chọn dịch vụ **Secrets Manager**

  ![](/images/2.6/0001.png)

---

#### 2. Tạo Secrets

- Vào **AWS Secrets Manager** > chọn **Secrets**
- Nhấn **Store a new secret**

  ![](/images/2.6/0002.png)

- Bước **Choose secret type**:
  - **Secret type** > chọn **Credentials for RDS database**
  - Phần **Credentials**:
    - **User name**: `admin`
    - **Password**: `your-password`

  ![](/images/2.6/0003.png)

    ✅ **Hoặc** chọn **Other type of secrets** và nhập thủ công dữ liệu JSON:

    ```json
    {
      "username": "admin",
      "password": "your-password"
    }
    ```
  - Phần **Database** > chọn **MyApp-RDS**

  - Nhấn **Next** để tiếp tục

  ![](/images/2.6/0004.png)

---

- Bước **Configure secret**:
  - **Secret name**: `MyAPP-Secret`
  - **Description**: `MySQL database credentials`

  ![](/images/2.6/0005.png)

  - Nhấn **Next** để tiếp tục

  ![](/images/2.6/0006.png)

---

- Bước **Configure rotation**:
  - Nhấn **Next** để tiếp tục

  ![](/images/2.6/0007.png)
---

- Bước **Review**:
  - Nhấn **Store** để tạo

  ![](/images/2.6/0008.png)

---