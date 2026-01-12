# Bài tập: Tìm hiểu Hệ thống cơ sở dữ liệu phân tán (PostgreSQL & MongoDB)

## 1. Thành phần bài tập
- **Báo cáo:** `BÁO CÁO TÌM HIỂU HỆ THỐNG PHÂN TÁN.pdf`
- **Mã nguồn:** Thư mục `MongoDB_Cluster` và `Postgres_Cluster`.

## 2. Yêu cầu hệ thống
- Đã cài đặt Docker và Docker Compose.
- Công cụ GUI: MongoDB Compass và DBeaver.

## 3. Cách triển khai và kiểm tra

### MongoDB Replica Set (3 nodes)
1. Di chuyển vào thư mục: 
   cd MongoDB_Cluster
   
Khởi chạy:
```powershell
docker-compose -f docker-compose-mongo.yml up -d
Kích hoạt Cluster (Chạy lệnh này 1 lần duy nhất lúc đầu):
code
Powershell
docker exec -it mongo1 mongosh --eval "rs.initiate({_id:'rs0', members:[{_id:0,host:'mongo1:27017'},{_id:1,host:'mongo2:27017'},{_id:2,host:'mongo3:27017'}]})"
Kiểm tra trạng thái:
code
Powershell
docker exec -it mongo1 mongosh --eval "rs.status()"
(Tìm dòng stateStr: 'PRIMARY' để biết node nào đang là chủ)
PostgreSQL Replication (Master-Slave)
Di chuyển vào thư mục:
code
Powershell
cd Postgres_Cluster
Khởi chạy:
code
Powershell
docker-compose -f docker-compose-pg.yml up -d
Kiểm tra log:
code
Powershell
docker logs -f pg_slave
(Nếu thấy dòng "database system is ready to accept read only connections" là thành công).
4. Demo bằng công cụ GUI
MongoDB (Demo bằng MongoDB Compass)
Chuỗi kết nối (URI): mongodb://localhost:27018/?directConnection=true
(Lưu ý: Port 27018 là ví dụ, hãy kết nối vào Port của node đang giữ vai trò PRIMARY).
Kịch bản Demo:
Kết nối vào node Primary, thêm dữ liệu mới.
Kết nối sang node Secondary (Port 27017 hoặc 27019) để kiểm tra dữ liệu đã tự động đồng bộ.
Test Failover: Chạy lệnh docker stop mongo2 (hoặc node Primary hiện tại) và quan sát node khác tự động lên làm Primary.
PostgreSQL (Demo bằng DBeaver)
Thông tin kết nối:
User: minh_admin
Password: minh_password
Database: distributed_db
Kịch bản Demo:
Kết nối Master (Port 5432): Thực hiện lệnh INSERT / UPDATE dữ liệu.
Kết nối Replica (Port 5433):
Thực hiện SELECT để chứng minh dữ liệu đã đồng bộ.
Thực hiện INSERT để chứng minh cơ chế bảo vệ (Hệ thống sẽ báo lỗi Read-only transaction).
Lệnh dọn dẹp (Tắt server):
code
Powershell
docker-compose -f docker-compose-mongo.yml down
docker-compose -f docker-compose-pg.yml down