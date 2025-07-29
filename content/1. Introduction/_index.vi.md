---
title : "Giới thiệu"
date : 2025-07-03
weight : 1
chapter : false
pre : " <b> 1. </b> "
---


### 1. Tổng quan

**Amazon RDS Proxy** là một dịch vụ proxy cơ sở dữ liệu được quản lý hoàn toàn bởi AWS, dùng để tối ưu kết nối đến **Amazon RDS** và **Amazon Aurora**. Dịch vụ này giúp ứng dụng:

- Tăng hiệu năng truy cập cơ sở dữ liệu.
- Tối ưu hóa việc tái sử dụng kết nối (connection pooling).
- Giảm độ trễ trong các môi trường serverless như AWS Lambda.
- Tăng tính sẵn sàng và khả năng phục hồi sau lỗi (failover).

---

### 2. Cách Hoạt Động

RDS Proxy hoạt động như một lớp trung gian thông minh giữa ứng dụng và cơ sở dữ liệu:

- **Connection Pooling**: RDS Proxy giữ sẵn một nhóm kết nối đến backend Database.
- Khi ứng dụng yêu cầu truy cập, RDS Proxy sẽ tái sử dụng kết nối nếu có, hoặc mở mới nếu cần.
- RDS Proxy quản lý thời gian sống của kết nối, failover, và đóng/mở kết nối theo trạng thái hệ thống.

---

### 3. Tính năng nổi bật

| Tính năng                         | Mô tả                                                                 |
|----------------------------------|-----------------------------------------------------------------------|
| 🔁 Connection pooling            | Giảm số lượng kết nối Database đồng thời, tránh vượt giới hạn              |
| ⚡ Tối ưu hóa Lambda/Container    | Giảm độ trễ khi kết nối Database trong môi trường serverless               |
| 🔐 Tích hợp IAM & TLS            | Bảo mật mạnh mẽ với IAM, Secrets Manager và mã hóa TLS                |
| 🛡️ Tự động failover              | Hỗ trợ chuyển đổi Database backend tự động khi có sự cố                    |
| 📊 Tích hợp giám sát             | Kết hợp với CloudWatch để theo dõi hiệu suất     

---

### 4. Chi phí và khả năng cung cấp khu vực & phiên bản

#### 💰 Chi phí
Chi phí của Amazon RDS Proxy được tính dựa trên:

- **Số lượng vCPU** sử dụng bởi proxy
- **Thời gian hoạt động** của proxy (theo giờ)
- Không tính thêm phí dựa trên số lượng kết nối hay request
- Bạn vẫn trả chi phí thông thường cho Amazon RDS hoặc Aurora

> 📘 [Xem bảng giá chi tiết](https://aws.amazon.com/rds/proxy/pricing/)


---

### 5. Hạn ngạch và giới hạn cho RDS Proxy

| Hạng mục                             | Mặc định                 |
|--------------------------------------|--------------------------|
| Số lượng Proxy mỗi tài khoản         | 20                       |
| Target group mỗi Proxy               | 20                       |
| Database instance mỗi Proxy                | 1                        |
| Endpoint mỗi Proxy                   | 1                        |
| Timeout lấy kết nối                  | 120 giây (có thể điều chỉnh) |
| Kết nối đồng thời hỗ trợ             | Hàng ngàn (auto scale)   |

> Có thể yêu cầu tăng giới hạn qua AWS Support nếu cần.

#### Những hạn chế bổ sung cho RDS dành cho MariaDB

Các hạn chế khi dùng Amazon RDS Proxy với RDS for MariaDB:

- Proxy chỉ lắng nghe trên **cổng 3306**, nhưng vẫn kết nối với Database bằng cổng được cấu hình.
- ❌ Không hỗ trợ MariaDB tự quản lý trên EC2.
- ❌ Không hoạt động nếu `read_only = 1` trong Database parameter group.
- ❌ Không hỗ trợ chế độ **nén MariaDB** (`--compress`, `-C`).
- ❌ Không hỗ trợ plugin xác thực `auth_ed25519`.
- ❌ Không hỗ trợ **TLS 1.3**.
- ⚠️ `GET DIAGNOSTICS` có thể trả kết quả không chính xác nếu RDS Proxy tái sử dụng kết nối.
- ❌ Không hỗ trợ `caching_sha2_password` (qua ClientPasswordAuthType).
- ⚠️ Không nên dùng `sql_auto_is_null = true` trong truy vấn khởi tạo proxy — có thể gây lỗi ứng dụng.

---

#### Những hạn chế bổ sung cho RDS dành cho Microsoft SQL Server

Các hạn chế khi dùng RDS Proxy với RDS for SQL Server:

- ⚠️ Số lượng **Secrets** trong AWS Secrets Manager có thể nhiều nếu SQL Server dùng collation phân biệt chữ hoa/thường.
- ❌ Không hỗ trợ kết nối sử dụng **Active Directory**.
- ❌ IAM authentication không hoạt động với client không hỗ trợ thuộc tính token.
- ⚠️ Các biến hệ thống như `@@IDENTITY`, `@@ROWCOUNT`, `SCOPE_IDENTITY()` có thể trả sai nếu không được lấy trong cùng một câu lệnh phiên.
- ❌ Nếu dùng **MARS (Multiple Active Result Sets)**, proxy sẽ không thực thi truy vấn khởi tạo.
- ❌ Không hỗ trợ phiên bản **SQL Server 2014** và **SQL Server 2022**.
- ❌ Không hỗ trợ client không xử lý được nhiều thông điệp TLS trong một bản ghi.

---

#### Những hạn chế bổ sung cho RDS dành cho MySQL

Các hạn chế khi dùng Amazon RDS Proxy với RDS for MySQL:

- Proxy chỉ lắng nghe trên **cổng 3306**.
- ❌ Không hỗ trợ MySQL tự quản lý trên EC2.
- ❌ Không hoạt động nếu `read_only = 1` trong Database parameter group.
- ❌ Không hỗ trợ chế độ **nén MySQL** (`--compress`, `-C`).
- ❌ Không hỗ trợ **mật khẩu kép** (dual password) của MySQL.
- ❌ Không hỗ trợ client không xử lý nhiều phản hồi trong một bản ghi TLS.
- ⚠️ `GET DIAGNOSTICS` có thể trả kết quả sai khi tái sử dụng kết nối.
- ⚠️ Một số câu lệnh như `SET LOCAL` có thể thay đổi trạng thái phiên mà không gây ghim.
- ❌ `ROW_COUNT()` không hoạt động đúng với truy vấn nhiều câu lệnh.
- ⚠️ Với driver MySQL 8.4 C, `mysql_stmt_bind_named_param()` có thể tạo gói lỗi nếu số lượng tham số vượt placeholder.
- ⚠️ `caching_sha2_password` yêu cầu TLS và có thể gặp vấn đề với driver Go (`go-sql`).
- ⚠️ Không nên dùng `sql_auto_is_null = true` trong truy vấn khởi tạo.

---

#### Những hạn chế bổ sung cho RDS dành cho PostgreSQL

Các hạn chế khi dùng Amazon RDS Proxy với RDS for PostgreSQL:

- Proxy chỉ lắng nghe trên **cổng 5432**.
- ❌ Không hỗ trợ lệnh `CancelRequest` từ client (như Ctrl+C trong `psql`).
- ⚠️ Kết quả `lastval` có thể không chính xác — nên dùng `INSERT ... RETURNING`.
- ❌ Không hỗ trợ **streaming replication**.
- ⚠️ `scram_iterations` mặc định là 4096 khi client auth với proxy (PostgreSQL 16).
- ⚠️ Cần có database mặc định.
- ⚠️ Nếu dùng `ALTER ROLE ... SET ROLE`, cần thiết lập `SET ROLE` lại trong truy vấn khởi tạo để tránh lỗi ghim.
- ❌ Không hỗ trợ bộ lọc ghim phiên cho PostgreSQL.

---

> ✅ **Lưu ý:** Hạn chế có thể thay đổi theo thời gian. Nên tham khảo tài liệu chính thức [Amazon RDS Proxy documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html) để cập nhật.

---

### 6. Khái niệm và thuật ngữ RDS Proxy

| Thuật ngữ         | Mô tả                                                                 |
|-------------------|----------------------------------------------------------------------|
| **Proxy endpoint** | Địa chỉ mà ứng dụng sử dụng thay vì Database endpoint gốc                 |
| **Connection pool** | Nhóm kết nối được mở sẵn để phục vụ nhiều client                   |
| **Target group**  | Nhóm các Database instances được liên kết với một Proxy                    |
| **IAM Role**      | Vai trò được gán để cấp quyền truy cập Proxy từ Lambda hoặc EC2      |
| **Secrets Manager** | Dịch vụ lưu trữ thông tin đăng nhập Database một cách an toàn           |

---

### 7. Bảo mật

Amazon RDS Proxy tích hợp nhiều lớp bảo mật:

- **IAM Authentication**: ứng dụng xác thực bằng role IAM, không cần hard-code mật khẩu.
- **TLS Encryption**: mã hóa toàn bộ đường truyền từ client → proxy → database backend.
- **Secrets Manager**: quản lý, xoay vòng và bảo vệ thông tin đăng nhập.
- **VPC Integration**: hoạt động trong mạng riêng ảo (VPC), giới hạn truy cập từ nội bộ.

---

### 8. Lưu ý

- RDS Proxy **không thay thế database**, mà là lớp trung gian tăng hiệu năng và bảo mật.
- Proxy phải nằm **trong cùng VPC** với RDS hoặc Aurora.
- Không hỗ trợ tất cả các phiên bản hoặc cấu hình database (Oracle, SQL Server).
- Hỗ trợ tốt nhất với ứng dụng sử dụng **kết nối ngắn hạn, đồng thời cao** như Lambda hoặc microservices.
- Không nên sử dụng RDS Proxy nếu ứng dụng có **ít kết nối** và **kết nối dài hạn liên tục**.

### 9. Tích hợp với dịch vụ AWS khác

Amazon RDS Proxy hoạt động hiệu quả khi được tích hợp với các dịch vụ AWS khác:

| Dịch vụ             | Vai trò tích hợp chính                                                                 |
|---------------------|----------------------------------------------------------------------------------------|
| **AWS Lambda**       | Kết nối ngắn hạn, scale cao — giảm cold start và timeout khi truy cập Database              |
| **Amazon ECS / EKS** | Hỗ trợ truy cập Database ổn định và bảo mật qua proxy từ container                          |
| **Amazon CloudWatch**| Theo dõi các chỉ số như `ConnectionCount`, `CurrentClientConnections`                 |
| **AWS Secrets Manager** | Tự động xoay vòng và bảo vệ thông tin xác thực                                    |
| **AWS IAM**          | Xác thực dựa trên vai trò IAM thay vì mật khẩu hard-code          
---

### 10. Các chỉ số giám sát (CloudWatch Metrics)

RDS Proxy cung cấp một số **chỉ số CloudWatch quan trọng** để giám sát và chẩn đoán hiệu năng:

| Chỉ số                                 | Ý nghĩa                                                                 |
|----------------------------------------|-------------------------------------------------------------------------|
| `DatabaseConnections`                 | Số lượng kết nối tới Database backend đang được sử dụng                      |
| `ClientConnections`                   | Số lượng client kết nối tới proxy                                       |
| `CurrentSessionPercent`              | % số phiên đang sử dụng trên tổng số có thể                            |
| `DatabaseConnectionBorrowTimeouts`   | Số lần client không lấy được kết nối trong thời gian timeout           |
| `ActiveConnections`                  | Tổng số kết nối đang được sử dụng tích cực                             |

> 👉 Có thể thiết lập **cảnh báo tự động** dựa trên các chỉ số này bằng Amazon **CloudWatch Alarms** để giám sát chủ động và phản ứng kịp thời với các vấn đề hiệu năng.

---

### 11. Các phương pháp tối ưu khi sử dụng RDS Proxy

Để đạt hiệu suất tối đa và đảm bảo độ ổn định khi sử dụng Amazon RDS Proxy, bạn nên áp dụng các phương pháp sau:

- ✅ **Thiết kế ứng dụng sử dụng kết nối ngắn hạn**: Tránh giữ kết nối database lâu một cách không cần thiết.
- ✅ **Sử dụng IAM hoặc Secrets Manager**: Tránh hard-code thông tin đăng nhập trong mã nguồn.
- ✅ **Theo dõi thường xuyên các chỉ số CloudWatch**: Để phát hiện kịp thời các vấn đề.
- ✅ **Tối ưu các tham số cấu hình Database** trong Parameter Group: như thời gian timeout kết nối, autocommit,....
- ✅ **Cấu hình truy vấn khởi tạo (Initialization Query)** một cách rõ ràng để đảm bảo mỗi kết nối được khởi động với trạng thái mong muốn.

> 💡 Việc thiết kế tốt các truy vấn khởi tạo và kiểm soát trạng thái kết nối giúp giảm hiện tượng ghim kết nối (pinning) và cải thiện khả năng tái sử dụng kết nối hiệu quả.

---


