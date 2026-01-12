# Bài tập: Tìm hiểu Hệ thống cơ sở dữ liệu phân tán (PostgreSQL & MongoDB)

## 1. Thành phần bài tập
- **Báo cáo:** `01_Bao_cao_chi_tiet.pdf`
- **Thuyết trình:** `02_Slide_thuyet_trinh.pdf`
- **Mã nguồn:** Thư mục `MongoDB_Cluster` và `Postgres_Cluster`.

## 2. Yêu cầu hệ thống
- Đã cài đặt Docker và Docker Compose.

## 3. Cách triển khai và kiểm tra

### MongoDB Replica Set (3 nodes)
1. Di chuyển vào thư mục: `cd MongoDB_Cluster`
2. Khởi chạy: `docker-compose up -d`
3. Kiểm tra trạng thái: `docker exec -it mongo1 mongosh --eval "rs.status()"`

### PostgreSQL Replication (Master-Slave)
1. Di chuyển vào thư mục: `cd Postgres_Cluster`
2. Khởi chạy: `docker-compose up -d`
3. Kiểm tra log: `docker logs -f pg-slave` (Nếu thấy "database system is ready to accept read only connections" là thành công).

## 4. Demo bằng công cụ GUI

- MongoDB được demo bằng **MongoDB Compass**
  - Kết nối vào các node trong Replica Set
  - Quan sát Primary / Secondary và quá trình Failover

- PostgreSQL được demo bằng **DBeaver**
  - Kết nối Master và Replica
  - Thực hiện ghi dữ liệu trên Master
  - Kiểm tra dữ liệu trên Replica (read-only)