[readme_md.md](https://github.com/user-attachments/files/33905244/readme_md.md)
# 📝 HỆ THỐNG BLOG/CMS – ĐỀ TÀI SỐ 7

## 1. Giới thiệu

* **Học phần:** Triển khai và Quản trị Hệ thống Phần mềm
* **Đề tài:** Triển khai hệ thống Blog/CMS sử dụng Docker Compose, MySQL, phpMyAdmin, Nginx, Prometheus, Grafana và Loki.
* **Mã số sinh viên:** DTC245200004

### 🎯 Mục tiêu
Xây dựng và triển khai một hệ thống quản lý nội dung Blog/CMS hoàn chỉnh dựa trên **WordPress**. Hệ thống ứng dụng container hóa toàn diện với **Docker Compose**, sử dụng **Nginx** làm Reverse Proxy để định tuyến và bảo mật.

Dự án được thiết kế để dễ dàng mở rộng, tích hợp sẵn hệ thống giám sát (Prometheus, Grafana), hệ thống quản lý log tập trung (Loki, Promtail) và áp dụng các tiêu chuẩn bảo mật cơ bản (HTTPS, Security Headers).

---

## 2. Công nghệ sử dụng

### 🛠 Hệ thống cốt lõi
| Công nghệ | Chức năng |
|---|---|
| **Docker / Compose** | Đóng gói, chạy container và quản lý các dịch vụ |
| **WordPress** | Nền tảng Website Blog/CMS |
| **MySQL 8.4** | Hệ quản trị cơ sở dữ liệu |
| **phpMyAdmin** | Giao diện quản trị cơ sở dữ liệu MySQL |
| **Nginx** | Web Server & Reverse Proxy |
| **Git & GitHub** | Quản lý phiên bản, lưu trữ mã nguồn và cấu hình |

### 📊 Hệ thống giám sát & Logs (Monitoring & Logging)
| Công nghệ | Chức năng |
|---|---|
| **Prometheus** | Thu thập Metrics từ các hệ thống |
| **Grafana** | Bảng điều khiển (Dashboard) trực quan hóa giám sát |
| **cAdvisor / Exporters**| Giám sát Docker Containers, Nginx và MySQL |
| **Loki** | Lưu trữ và truy vấn Logs tập trung |
| **Promtail** | Thu thập Logs từ các container chuyển về Loki |
| **LogQL** | Ngôn ngữ truy vấn Logs chuyên dụng |

---

## 3. Kiến trúc hệ thống

```text
            Người dùng
                 |
                 v (HTTPS: 8443 / HTTP: 8080)
         +---------------+
         |     NGINX     |
         | Reverse Proxy |
         +---------------+
                 |
                 v
         +---------------+
         |   WORDPRESS   |
         |    Blog CMS   |
         +---------------+
                 |
                 v
         +---------------+         +---------------+
         |     MYSQL     | <------ |  phpMyAdmin   |
         |   Database    |         |  Quản trị DB  |
         +---------------+         +---------------+
```

**Nguyên lý hoạt động:**
1. Người dùng truy cập website qua Nginx (hỗ trợ tự động chuyển hướng HTTP sang HTTPS).
2. Nginx đóng vai trò Reverse Proxy, chuyển tiếp HTTP Request đến container WordPress.
3. WordPress xử lý yêu cầu, truy vấn dữ liệu từ MySQL và trả phản hồi lại cho người dùng thông qua Nginx.
4. Quản trị viên có thể sử dụng phpMyAdmin để quản lý trực tiếp cơ sở dữ liệu qua một cổng riêng biệt.

---

## 4. Cấu trúc thư mục

```text
blog-cms-de7/
├── docker-compose.yml
├── .env.example
├── .gitignore
├── README.md
├── nginx/
│   ├── conf.d/
│   │   └── default.conf
│   └── ssl/                 # Nơi lưu trữ chứng chỉ SSL
├── prometheus/
├── grafana/
│   ├── dashboards/
│   └── provisioning/
├── loki/
├── promtail/
├── database/
├── scripts/
└── docs/
```
*(Lưu ý: Dữ liệu phát sinh của WordPress và MySQL được lưu trong Docker Volumes và không được đẩy lên GitHub để đảm bảo an toàn).*

---

## 5. Hướng dẫn triển khai

### Yêu cầu môi trường
* Windows 11 / WSL 2 / Docker Desktop (chế độ Linux Containers).
* Git, Trình duyệt web (Chrome/Edge).

### Bước 1: Clone Repository
```powershell
git clone https://github.com/phnglnh1526/DTC245200004-blog-cms-de7.git
cd DTC245200004-blog-cms-de7
```

### Bước 2: Cấu hình biến môi trường
Tạo file `.env` từ file mẫu và thiết lập mật khẩu an toàn:
```powershell
Copy-Item .env.example .env
notepad .env
```
*Lưu ý: Đổi các giá trị `CHANGE_ME_*` thành mật khẩu thực tế của bạn.*

### Bước 3: Tạo chứng chỉ SSL (Tự ký cho localhost)
Chạy các lệnh sau để tạo thư mục chứa SSL và sinh chứng chỉ:
```powershell
New-Item -ItemType Directory -Path .\nginx\ssl -Force

docker run --rm -v "${PWD}/nginx/ssl:/certs" alpine/openssl:3.5.9 req -x509 -newkey rsa:2048 -sha256 -noenc -days 365 -keyout /certs/localhost.key -out /certs/localhost.crt -subj "/CN=localhost" -addext "subjectAltName=DNS:localhost,IP:127.0.0.1"
```

### Bước 4: Khởi động hệ thống
```powershell
docker compose config --quiet  # Kiểm tra lỗi cú pháp
docker compose up -d           # Khởi động ở chế độ chạy ngầm
docker compose ps              # Kiểm tra trạng thái các container
```

### Bước 5: Thiết lập WordPress lần đầu
1. Truy cập: `https://localhost:8443` (Chấp nhận cảnh báo bảo mật do dùng SSL tự ký).
2. Hoàn tất các bước cài đặt WordPress trên giao diện (chọn ngôn ngữ, tạo tài khoản quản trị).

---

## 6. Địa chỉ truy cập & Quản lý

| Dịch vụ | Địa chỉ truy cập |
|---|---|
| **Website Blog** | `https://localhost:8443` (Chính) hoặc `http://localhost:8080` |
| **WordPress Admin**| `https://localhost:8443/wp-admin` |
| **phpMyAdmin** | `http://localhost:8081` |
| **Grafana** | `http://localhost:3000` |

### Các lệnh quản lý cơ bản
* **Xem log toàn hệ thống:** `docker compose logs`
* **Xem log từng dịch vụ:** `docker compose logs wordpress` hoặc `docker compose logs nginx`
* **Dừng hệ thống:** `docker compose down` (Không dùng `-v` nếu không muốn mất dữ liệu).

---

## 7. Giám sát & Logging Tập Trung (Monitoring & Centralized Logging)

Hệ thống sử dụng **Loki** và **Promtail** để thu thập log từ tất cả các container.
* Loki tiếp nhận log tại cổng `3100` (chỉ nội bộ localhost).
* Truy cập **Grafana** (`http://localhost:3000`) -> **Explore** -> Chọn Data Source là `loki`.

**Một số câu lệnh LogQL hữu ích:**
* Log Nginx: `{job="blog-cms", service="nginx"}`
* Lọc lỗi MySQL: `{job="blog-cms", service="db"} |~ "(?i)error"`

---

## 8. Bảo mật & Hardening (HTTPS & Security)

Dự án áp dụng các tiêu chuẩn bảo mật nhằm hạn chế rủi ro:
1. **HTTPS Enforced:** Hỗ trợ TLS 1.2/1.3, tự động chuyển hướng HTTP (8080) sang HTTPS (8443).
2. **Security Headers Nginx:** Được cấu hình sẵn (`X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`).
3. **Ẩn thông tin Server:** Ẩn phiên bản Nginx và header `X-Powered-By`.
4. **Bảo vệ file nhạy cảm:** Nginx tự động chặn (`404`) các truy cập vào `.env`, `.git`, và `wp-config.php`.
5. **Network Isolation:** MySQL chỉ giao tiếp nội bộ trong Docker Network, không mở cổng ra Host.

**Kiểm tra bảo mật:**
```powershell
curl.exe -k -I https://localhost:8443/.env         # Phải trả về 404
curl.exe -I http://localhost:8080                  # Phải trả về 301 Moved Permanently
```

---

## 9. Các giai đoạn triển khai (Roadmap)
- [x] **Giai đoạn 1:** Triển khai Website (WordPress, MySQL, Nginx Proxy), cấu hình GitHub, viết README.
- [x] **Giai đoạn 2:** Tích hợp HTTPS & Security Hardening cơ bản.
- [x] **Giai đoạn 3:** Centralized Logging (Loki, Promtail) tích hợp Grafana.
- [ ] **Giai đoạn 4 (Sắp tới):** Hoàn thiện Prometheus & cấu hình cảnh báo tài nguyên (Alerts).
