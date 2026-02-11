# Docker Compose – Portfolio (BE + FE + DB)

Chạy toàn bộ stack: MySQL, Backend (NestJS), Frontend (React + nginx).

## Chỉ chạy DB (để dev backend/frontend local)

```bash
# Ở thư mục gốc repo (chứa docker-compose.yml)
docker compose up -d db
```

Backend chạy trên máy (không trong Docker) cần kết nối tới MySQL trong container:

- Trong **portfolio-backend** tạo/cập nhật `.env`:
  - `DB_HOST=localhost`
  - `DB_PORT=10003` (port map của service `db` ra host)
  - `DB_USER=portfolio`
  - `DB_PASSWORD=portfolio`
  - `DB_NAME=portfolio`

Sau khi DB chạy, có thể chạy migration: `cd portfolio-backend && npm run migration:run`.

Nếu gặp lỗi Docker credential (ví dụ "One or more parameters passed to the function were not valid"), thử đăng xuất/đăng nhập lại Docker Desktop hoặc sửa cấu hình credential helper trong `~/.docker/config.json`.

## Ports (> 10k)

| Service   | Host port | Mô tả              |
|-----------|-----------|--------------------|
| Frontend  | 10002     | Web app (nginx)    |
| Backend   | 10001     | API + Swagger      |
| MySQL     | 10003     | DB (optional map)  |

## Chạy

```bash
# Tạo .env từ example (tùy chọn)
cp .env.example .env

# Build và chạy
docker compose up -d --build

# Xem log
docker compose logs -f
```

- **App**: http://localhost:10002  
- **API**: http://localhost:10001/api  
- **Swagger**: http://localhost:10001/api/docs  

Frontend gọi API qua cùng origin (nginx proxy `/api` → backend), không cần cấu hình `VITE_API_URL` khi chạy bằng Docker.

## Dừng & xóa volume

```bash
docker compose down
# Xóa cả data MySQL:
docker compose down -v
```

## Biến môi trường (.env)

| Biến         | Mặc định    | Ghi chú                    |
|--------------|-------------|----------------------------|
| DB_USER      | portfolio   | User MySQL                 |
| DB_PASSWORD  | portfolio   | Mật khẩu MySQL (và root)  |
| DB_NAME      | portfolio   | Tên database               |
| DB_PORT      | 10003       | Port map MySQL ra host     |
| JWT_SECRET   | change-me…  | Secret JWT (đổi khi deploy)|
