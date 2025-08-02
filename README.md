# Supabase Docker

## Giới thiệu
Supabase Docker là bộ thiết lập tối giản sử dụng Docker Compose để tự triển khai Supabase trên máy chủ riêng. Dự án này giúp bạn nhanh chóng khởi tạo môi trường Supabase đầy đủ các dịch vụ: Postgres, Realtime, Storage, API, và các chức năng mở rộng.

## Tính năng
- Tự động khởi tạo các dịch vụ Supabase
- Hỗ trợ cấu hình S3 Storage
- Tích hợp các hàm serverless (Functions)
- Dễ dàng reset dữ liệu và khởi tạo lại môi trường
- Tùy chỉnh cấu hình qua các file YAML

## Cấu trúc thư mục
```
├── docker-compose.yml
├── docker-compose.s3.yml
├── README.md
├── reset.sh
├── dev/
│   ├── data.sql
│   └── docker-compose.dev.yml
├── volumes/
│   ├── api/
│   │   └── kong.yml
│   ├── db/
│   │   ├── _supabase.sql
│   │   ├── jwt.sql
│   │   ├── logs.sql
│   │   ├── pooler.sql
│   │   ├── realtime.sql
│   │   ├── roles.sql
│   │   ├── webhooks.sql
│   │   └── init/
│   │       └── data.sql
│   ├── functions/
│   │   ├── hello/
│   │   │   └── index.ts
│   │   └── main/
│   │       └── index.ts
│   ├── logs/
│   │   └── vector.yml
│   ├── pooler/
│   │   └── pooler.exs
│   └── storage/
│       └── stub/
```

## Yêu cầu hệ thống
- Docker >= 20.10
- Docker Compose >= 1.29
- macOS, Linux hoặc Windows

## Hướng dẫn cài đặt
1. Clone repository:
   ```bash
   git clone <repo-url>
   cd supabase-docker
   ```
2. Khởi động các dịch vụ:
   ```bash
   docker-compose up -d
   ```
   Hoặc với S3 Storage:
   ```bash
   docker-compose -f docker-compose.s3.yml up -d
   ```
3. Truy cập Supabase Studio tại: `http://localhost:3000`

## Reset dữ liệu
Chạy script sau để reset database:
```bash
./reset.sh
```

## Các lệnh thường dùng
- Dừng dịch vụ: `docker-compose down`
- Xem logs: `docker-compose logs -f`
- Khởi động lại: `docker-compose restart`

## Troubleshooting
- Kiểm tra cổng đã mở: 5432 (Postgres), 3000 (Studio), 8000 (API)
- Nếu gặp lỗi khởi động, kiểm tra file cấu hình và logs
- Đảm bảo Docker và Compose đã được cập nhật

## Tài liệu tham khảo
- [Supabase Docker Guide](https://supabase.com/docs/guides/hosting/docker)
- [Supabase Documentation](https://supabase.com/docs)

## Liên hệ & Hỗ trợ
- Email: your@email.com
- Issues: [GitHub Issues](<repo-issues-url>)

---
Bạn có thể tùy chỉnh thêm các dịch vụ hoặc cấu hình theo nhu cầu dự án.
