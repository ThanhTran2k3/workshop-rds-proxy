---
title: "Introduction"
date: 2025-07-03
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

### 1. Overview

**Amazon RDS Proxy** is a fully managed database proxy service by AWS, designed to optimize connections to **Amazon RDS** and **Amazon Aurora**. This service helps applications:

- Improve database access performance.
- Optimize connection reuse (connection pooling).
- Reduce latency in serverless environments like AWS Lambda.
- Increase availability and failover resilience.

---

### 2. How It Works

RDS Proxy operates as an intelligent intermediary layer between applications and databases:

- **Connection Pooling**: RDS Proxy maintains a pool of connections to the backend database.
- When applications request access, RDS Proxy will reuse existing connections if available, or open new ones if needed.
- RDS Proxy manages connection lifetime, failover, and opening/closing connections based on system status.

---

### 3. Key Features

| Feature                              | Description                                                           |
|--------------------------------------|-----------------------------------------------------------------------|
| 🔁 Connection pooling               | Reduces concurrent database connections, avoiding connection limits    |
| ⚡ Lambda/Container optimization     | Reduces latency when connecting to databases in serverless environments |
| 🔐 IAM & TLS integration            | Strong security with IAM, Secrets Manager, and TLS encryption        |
| 🛡️ Automatic failover              | Supports automatic database backend switching during failures         |
| 📊 Monitoring integration           | Integrates with CloudWatch for performance monitoring                 |

---

### 4. Pricing and Regional Availability & Versions

#### 💰 Pricing
Amazon RDS Proxy pricing is based on:

- **Number of vCPUs** used by the proxy
- **Runtime** of the proxy (per hour)
- No additional charges based on number of connections or requests
- You still pay standard charges for Amazon RDS or Aurora

> 📘 [View detailed pricing](https://aws.amazon.com/rds/proxy/pricing/)

---

### 5. Quotas and Limits for RDS Proxy

| Item                                 | Default                  |
|--------------------------------------|--------------------------|
| Number of Proxies per account       | 20                       |
| Target groups per Proxy             | 20                       |
| Database instances per Proxy        | 1                        |
| Endpoints per Proxy                 | 1                        |
| Connection acquisition timeout      | 120 seconds (adjustable) |
| Concurrent connections supported    | Thousands (auto scale)   |

> You can request limit increases through AWS Support if needed.

#### Additional Limitations for RDS for MariaDB

Limitations when using Amazon RDS Proxy with RDS for MariaDB:

- Proxy only listens on **port 3306**, but still connects to the database using the configured port.
- ❌ Does not support self-managed MariaDB on EC2.
- ❌ Does not work if `read_only = 1` in the database parameter group.
- ❌ Does not support **MariaDB compression** (`--compress`, `-C`).
- ❌ Does not support `auth_ed25519` authentication plugin.
- ❌ Does not support **TLS 1.3**.
- ⚠️ `GET DIAGNOSTICS` may return incorrect results if RDS Proxy reuses connections.
- ❌ Does not support `caching_sha2_password` (via ClientPasswordAuthType).
- ⚠️ Should not use `sql_auto_is_null = true` in proxy initialization queries — may cause application errors.

---

#### Additional Limitations for RDS for Microsoft SQL Server

Limitations when using RDS Proxy with RDS for SQL Server:

- ⚠️ Number of **Secrets** in AWS Secrets Manager may be high if SQL Server uses case-sensitive collation.
- ❌ Does not support connections using **Active Directory**.
- ❌ IAM authentication does not work with clients that don't support token attributes.
- ⚠️ System variables like `@@IDENTITY`, `@@ROWCOUNT`, `SCOPE_IDENTITY()` may return incorrect values if not retrieved within the same statement session.
- ❌ If using **MARS (Multiple Active Result Sets)**, proxy will not execute initialization queries.
- ❌ Does not support **SQL Server 2014** and **SQL Server 2022** versions.
- ❌ Does not support clients that cannot handle multiple TLS messages in one record.

---

#### Additional Limitations for RDS for MySQL

Limitations when using Amazon RDS Proxy with RDS for MySQL:

- Proxy only listens on **port 3306**.
- ❌ Does not support self-managed MySQL on EC2.
- ❌ Does not work if `read_only = 1` in the database parameter group.
- ❌ Does not support **MySQL compression** (`--compress`, `-C`).
- ❌ Does not support MySQL **dual password**.
- ❌ Does not support clients that cannot handle multiple responses in one TLS record.
- ⚠️ `GET DIAGNOSTICS` may return incorrect results when reusing connections.
- ⚠️ Some statements like `SET LOCAL` may change session state without causing pinning.
- ❌ `ROW_COUNT()` does not work correctly with multi-statement queries.
- ⚠️ With MySQL 8.4 C driver, `mysql_stmt_bind_named_param()` may create error packets if parameter count exceeds placeholders.
- ⚠️ `caching_sha2_password` requires TLS and may have issues with Go driver (`go-sql`).
- ⚠️ Should not use `sql_auto_is_null = true` in initialization queries.

---

#### Additional Limitations for RDS for PostgreSQL

Limitations when using Amazon RDS Proxy with RDS for PostgreSQL:

- Proxy only listens on **port 5432**.
- ❌ Does not support `CancelRequest` command from client (like Ctrl+C in `psql`).
- ⚠️ `lastval` results may be inaccurate — should use `INSERT ... RETURNING`.
- ❌ Does not support **streaming replication**.
- ⚠️ `scram_iterations` defaults to 4096 when client auth with proxy (PostgreSQL 16).
- ⚠️ Requires a default database.
- ⚠️ If using `ALTER ROLE ... SET ROLE`, need to set `SET ROLE` again in initialization query to avoid pinning errors.
- ❌ Does not support session pinning filters for PostgreSQL.

---

> ✅ **Note:** Limitations may change over time. Refer to the official [Amazon RDS Proxy documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html) for updates.

---

### 6. RDS Proxy Concepts and Terminology

| Term              | Description                                                           |
|-------------------|-----------------------------------------------------------------------|
| **Proxy endpoint** | Address that applications use instead of the original database endpoint |
| **Connection pool** | Group of pre-opened connections to serve multiple clients           |
| **Target group**  | Group of database instances associated with a Proxy                 |
| **IAM Role**      | Role assigned to grant Proxy access from Lambda or EC2              |
| **Secrets Manager** | Service for securely storing database login credentials             |

---

### 7. Security

Amazon RDS Proxy integrates multiple security layers:

- **IAM Authentication**: Applications authenticate using IAM roles, no need for hard-coded passwords.
- **TLS Encryption**: Encrypts entire transmission path from client → proxy → backend database.
- **Secrets Manager**: Manages, rotates, and protects login credentials.
- **VPC Integration**: Operates within Virtual Private Cloud (VPC), limiting access to internal networks.

---

### 8. Notes

- RDS Proxy **does not replace the database**, but serves as an intermediary layer for performance and security enhancement.
- Proxy must be **in the same VPC** as RDS or Aurora.
- Does not support all database versions or configurations (Oracle, SQL Server).
- Works best with applications using **short-term, high-concurrency connections** like Lambda or microservices.
- Should not use RDS Proxy if applications have **few connections** and **long-term persistent connections**.

### 9. Integration with Other AWS Services

Amazon RDS Proxy works efficiently when integrated with other AWS services:

| Service             | Primary Integration Role                                                   |
|---------------------|---------------------------------------------------------------------------|
| **AWS Lambda**      | Short-term, high-scale connections — reduces cold start and timeout when accessing database |
| **Amazon ECS / EKS** | Supports stable and secure database access via proxy from containers     |
| **Amazon CloudWatch** | Monitors metrics like `ConnectionCount`, `CurrentClientConnections`    |
| **AWS Secrets Manager** | Automatically rotates and protects authentication credentials         |
| **AWS IAM**         | IAM role-based authentication instead of hard-coded passwords           |

---

### 10. Monitoring Metrics (CloudWatch Metrics)

RDS Proxy provides several **important CloudWatch metrics** for monitoring and diagnosing performance:

| Metric                               | Meaning                                                                 |
|--------------------------------------|-------------------------------------------------------------------------|
| `DatabaseConnections`               | Number of connections to backend database currently in use              |
| `ClientConnections`                 | Number of client connections to proxy                                   |
| `CurrentSessionPercent`            | Percentage of sessions in use out of total possible                     |
| `DatabaseConnectionBorrowTimeouts` | Number of times clients failed to acquire connection within timeout     |
| `ActiveConnections`                | Total number of actively used connections                               |

> 👉 You can set up **automatic alerts** based on these metrics using Amazon **CloudWatch Alarms** for proactive monitoring and timely response to performance issues.

---

### 11. Best Practices for Using RDS Proxy

To achieve maximum performance and ensure stability when using Amazon RDS Proxy, you should apply the following practices:

- ✅ **Design applications to use short-term connections**: Avoid holding database connections unnecessarily long.
- ✅ **Use IAM or Secrets Manager**: Avoid hard-coding credentials in source code.
- ✅ **Regularly monitor CloudWatch metrics**: To detect issues in a timely manner.
- ✅ **Optimize database parameter configuration** in Parameter Group: such as connection timeout, autocommit,...
- ✅ **Configure initialization queries clearly** to ensure each connection starts with the desired state.

> 💡 Good design of initialization queries and connection state control helps reduce connection pinning and improves connection reuse efficiency.

---