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
```powershell
docker exec -it mongo1 mongosh --eval "rs.initiate({_id:'rs0', members:[{_id:0,host:'mongo1:27017'},{_id:1,host:'mongo2:27017'},{_id:2,host:'mongo3:27017'}]})"