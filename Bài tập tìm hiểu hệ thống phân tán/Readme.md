BÁO CÁO BÀI TẬP LỚN: HỆ THỐNG CƠ SỞ DỮ LIỆU PHÂN TÁN
1. Thành phần bài tập
Báo cáo: BÁO CÁO TÌM HIỂU HỆ THỐNG PHÂN TÁN.pdf
Mã nguồn:
Thư mục MongoDB_Cluster (Cấu hình Replica Set)
Thư mục Postgres_Cluster (Cấu hình Master-Slave)
2. Yêu cầu hệ thống
Đã cài đặt Docker Desktop và Docker Compose.
Công cụ quản lý GUI: MongoDB Compass và DBeaver.
3. Hướng dẫn Triển khai & Kiểm tra
A. MongoDB Cluster (Replica Set - 3 Nodes)
Mô hình: 1 Primary (Ghi/Đọc) - 2 Secondary (Chỉ đọc).
Bước 1: Khởi chạy
code
Bash
cd MongoDB_Cluster
docker-compose -f docker-compose-mongo.yml up -d
Bước 2: Kích hoạt Replica Set (Chỉ chạy lần đầu)
code
Bash
docker exec -it mongo1 mongosh --eval "rs.initiate({_id:'rs0', members:[{_id:0,host:'mongo1:27017'},{_id:1,host:'mongo2:27017'},{_id:2,host:'mongo3:27017'}]})"
Bước 3: Kiểm tra trạng thái
code
Bash
docker exec -it mongo1 mongosh --eval "rs.status()"
(Tìm dòng "Primary" và "Secondary" trong kết quả trả về).
B. PostgreSQL Cluster (Master - Slave)
Mô hình: Master (Port 5432) - Slave (Port 5433).
Bước 1: Khởi chạy
code
Bash
cd Postgres_Cluster
docker-compose -f docker-compose-pg.yml up -d
Bước 2: Thông tin kết nối (Credentials)
User: minh_admin
Password: minh_password
Database: distributed_db
Master Port: 5432 (Ghi/Đọc)
Slave Port: 5433 (Chỉ đọc)
Bước 3: Kiểm tra Log
code
Bash
docker logs -f pg_slave
(Nếu thấy dòng: "database system is ready to accept read only connections" là thành công).
4. Kịch bản Demo (GUI)
A. MongoDB (Sử dụng MongoDB Compass)
Kết nối Node Primary (để Ghi dữ liệu):
URI: mongodb://localhost:27018/?directConnection=true (Lưu ý: Port có thể thay đổi tùy vào node nào được bầu làm Primary, kiểm tra bằng lệnh status).
Thao tác: Tạo Collection, Insert dữ liệu mới.
Kết nối Node Secondary (để kiểm tra Đồng bộ):
URI: mongodb://localhost:27017/?directConnection=true
Thao tác: Refresh kiểm tra dữ liệu vừa tạo.
Demo Failover (Chịu lỗi):
Stop container Primary (docker stop mongo...).
Quan sát Node Secondary tự động lên chức Primary.
B. PostgreSQL (Sử dụng DBeaver)
Kết nối Master: Host localhost, Port 5432.
Thực hiện lệnh INSERT / UPDATE.
Kết nối Slave: Host localhost, Port 5433.
Thực hiện SELECT để thấy dữ liệu đồng bộ.
Thực hiện INSERT để chứng minh cơ chế bảo vệ (Sẽ báo lỗi Read-only transaction).
5. Dọn dẹp hệ thống (Tắt server)
Để tắt và xóa container (giữ lại dữ liệu):
code
Bash
docker-compose -f docker-compose-mongo.yml down
docker-compose -f docker-compose-pg.yml down