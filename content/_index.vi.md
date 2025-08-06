---
title: "Amazon RDS Proxy"
date : 2025-08-07
draft : false
categories : ["AWS Services", "Database Optimization"]
---

# Amazon Relational Database Service Proxy (Amazon RDS Proxy) 

## Tổng quan về Amazon RDS Proxy

**Amazon Relational Database Service Proxy (Amazon RDS Proxy)** là một dịch vụ proxy cơ sở dữ liệu được quản lý hoàn toàn dành cho Amazon RDS, giúp cải thiện hiệu năng, tăng tính sẵn sàng và khả năng mở rộng cho các ứng dụng sử dụng Amazon RDS hoặc Aurora.


---

## RDS Proxy Là Gì?

RDS Proxy đóng vai trò như một lớp trung gian giữa ứng dụng và Amazon RDS hoặc Aurora. RDS Proxy quản lý connection pooling giảm tải kết nối đến cơ sở dữ liệu và tái sử dụng các kết nối hiện có giúp tối ưu hóa tài nguyên và cải thiện hiệu suất ứng dụng.


## Lợi Ích Chính

| Tính năng                     | Lợi ích mang lại                                  |
|------------------------------|---------------------------------------------------|
| Connection Pooling           | Giảm số kết nối mới và tiết kiệm tài nguyên     |
| Tự động failover             | Tăng tính sẵn sàng cho ứng dụng                   |
| Tối ưu cho Lambda/Container  | Giảm chi phí và độ trễ kết nối trong serverless   |
| Bảo mật cao                  | Hỗ trợ IAM, TLS và Secrets Manager                |
| Giảm lỗi "too many connections" | Hạn chế vượt giới hạn kết nối Database       |

---

## Tích Hợp Bảo Mật

RDS Proxy hỗ trợ:

- **IAM authentication**: không cần lưu username/password trong ứng dụng
- **TLS encryption**: mã hóa kết nối end-to-end từ ứng dụng đến database
- **Secrets Manager**: lưu trữ và xoay vòng thông tin đăng nhập an toàn

---

## Tình Huống Sử Dụng Phù Hợp

- Ứng dụng serverless (AWS Lambda) cần kết nối nhanh
- Microservices có nhiều phiên bản cùng truy cập DB
- Giảm chi phí và độ trễ khi kết nối Database nhiều lần
- Hệ thống yêu cầu **tự động phục hồi khi failover**

---

## Cách Hoạt Động

1. Ứng dụng kết nối đến **RDS Proxy endpoint** thay vì RDS endpoint
2. RDS Proxy kiểm tra connection pool, nếu có sẵn tái sử dụng kết nối
3. Nếu không có, RDS Proxy mở kết nối mới đến RDS endpoint
4. Quản lý lifecycle kết nối và failover tự động

---

## Lưu ý

- RDS Proxy không thay thế RDS hay Aurora mà nó chỉ là **lớp kết nối trung gian**.
- Bạn cần bật **IAM role** cho Lambda/EC2 để kết nối đến proxy.
- Sử dụng cùng với **Multi-AZ RDS** để tăng độ sẵn sàng cho trường hợp fallover.
- RDS Proxy phải nằm trong cùng một VPC với RDS.

---

Nếu bạn đang xây dựng một hệ thống cần tính sẵn sàng cao và hiệu năng tốt khi truy cập database thì **RDS Proxy là dịch vụ nên cân nhắc**.

