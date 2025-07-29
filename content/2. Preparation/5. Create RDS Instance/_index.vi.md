---
title : "Tạo RDS Database"
date :  2025-07-03
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

### 🛠️ Bước 4: Tạo RDS Database (MySQL)

#### 1. Mở dịch vụ RDS

- Tìm kiếm và chọn dịch vụ **Aurora and RDS**

  ![](/images/2.5/0001.png)

---

#### 2. Tạo DB Subnet Group.

- Vào **Aurora and RDS** > chọn **Subnet groups**
- Nhấn **Create DB subnet group**

  ![](/images/2.5/0002.png)

- Nhập thông tin:
    - **Name**: `MyApp-RDSSG`
    - **Description**: `Subnet group for RDS MySQL`
    - **VPC** > chọn **MyApp-VPC**
    - **Availability Zone**  > chọn **ap-southeast-1b** và **ap-southeast-1c**
    - **Subnets**  > chọn **PrivateSubnet1** và **PrivateSubnet2**
- Nhấn **Create**  

    ![](/images/2.5/0003.png)

    ![](/images/2.5/0004.png)

---

#### 2. Tạo database
- Trong RDS Dashboard, chọn **Create database**

---

#### 3. Chọn phương thức tạo database
- **Engine options**:
  - Chọn **Standard Create**
- **Engine type**:
  - Chọn **MySQL**
- **Version**:
  - Chọn phiên bản phù hợp, ví dụ: `MySQL 8.0.35`

---

#### 4. Cấu hình thông tin database
- **Templates**:
  - Chọn `Dev/Test` (hoặc `Production` nếu cần HA)
- **DB instance identifier**:
  - Nhập tên ví dụ: `myapp-mysql-db`
- **Master username**:
  - Mặc định: `admin`
- **Master password**:
  - Nhập password và xác nhận

---

#### 5. Cấu hình instance
- **DB instance class**:
  - Chọn `db.t3.micro` (cho free-tier hoặc nhỏ gọn)
- **Storage**:
  - Chọn loại: `General Purpose (SSD)`
  - Dung lượng: Ví dụ `20 GiB`

---

#### 6. Connectivity
- **Virtual Private Cloud (VPC)**:
  - Chọn VPC đã tạo: `MyApp-VPC`
- **Subnet group**:
  - Chọn nhóm chứa subnet **PrivateSubnet1**
- **Public access**:
  - Chọn **No** (vì nằm trong subnet private)
- **VPC security group**:
  - Chọn `SG-RDS` (đã tạo trước đó)

---

#### 7. Cấu hình tùy chọn nâng cao
- **Database authentication**:
  - Tick **Enable IAM DB authentication** (tuỳ chọn)
- **Initial database name**:
  - Nhập tên DB nếu muốn tự tạo trước, ví dụ: `myappdb`

---

#### 8. Tạo DB
- Cuối trang, nhấn **Create database**

---

#### 9. Kiểm tra trạng thái
- Chờ vài phút, khi trạng thái `Available`, database đã sẵn sàng
- Nhấn vào DB để xem **Endpoint**, **Port**, sử dụng kết nối từ ECS

---
