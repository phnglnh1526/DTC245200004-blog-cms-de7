\# HỆ THỐNG BLOG/CMS – ĐỀ TÀI SỐ 7



\## 1. Giới thiệu



\*\*Học phần:\*\* Triển khai và Quản trị Hệ thống Phần mềm



\*\*Đề tài:\*\* Triển khai hệ thống Blog/CMS sử dụng Docker Compose, MySQL, phpMyAdmin, Nginx, Prometheus, Grafana và Loki.



\*\*Mã số sinh viên:\*\* DTC245200004



\### Mục tiêu



Xây dựng và triển khai hệ thống quản lý nội dung Blog/CMS bằng WordPress.



Hệ thống sử dụng Docker Compose để quản lý các dịch vụ, MySQL để lưu trữ dữ liệu và Nginx làm Reverse Proxy.



Các giai đoạn tiếp theo sẽ tích hợp hệ thống giám sát Prometheus, Grafana và hệ thống log tập trung Loki, Promtail.



Dự án đồng thời áp dụng các biện pháp bảo mật và quản lý mã nguồn trên GitHub.



\---



\## 2. Công nghệ sử dụng



| Công nghệ | Chức năng |

|---|---|

| Docker | Đóng gói và chạy container |

| Docker Compose | Quản lý các dịch vụ |

| WordPress | Website Blog/CMS |

| MySQL 8.4 | Lưu trữ cơ sở dữ liệu |

| phpMyAdmin | Quản trị MySQL |

| Nginx | Reverse Proxy |

| Git | Quản lý phiên bản |

| GitHub | Lưu trữ mã nguồn và cấu hình |



\### Công nghệ dự kiến tích hợp



| Công nghệ | Chức năng |

|---|---|

| Prometheus | Thu thập Metrics |

| Grafana | Trực quan hóa giám sát |

| cAdvisor | Giám sát Docker Containers |

| Nginx Exporter | Giám sát Nginx |

| MySQL Exporter | Giám sát MySQL |

| Loki | Lưu trữ và truy vấn Logs |

| Promtail | Thu thập Logs |

| LogQL | Ngôn ngữ truy vấn Logs |



\---



\## 3. Kiến trúc hệ thống



Kiến trúc hiện tại:



```text

&#x20;            Người dùng

&#x20;                 |

&#x20;                 |

&#x20;         http://localhost:8080

&#x20;                 |

&#x20;                 v

&#x20;         +---------------+

&#x20;         |     NGINX     |

&#x20;         | Reverse Proxy |

&#x20;         +---------------+

&#x20;                 |

&#x20;                 v

&#x20;         +---------------+

&#x20;         |   WORDPRESS   |

&#x20;         |    Blog CMS   |

&#x20;         +---------------+

&#x20;                 |

&#x20;                 v

&#x20;         +---------------+

&#x20;         |     MYSQL     |

&#x20;         |   Database    |

&#x20;         +---------------+

&#x20;                 ^

&#x20;                 |

&#x20;         +---------------+

&#x20;         |  phpMyAdmin   |

&#x20;         |  Quản trị DB  |

&#x20;         +---------------+

```



\### Nguyên lý hoạt động



1\. Người dùng truy cập website qua Nginx.

2\. Nginx chuyển tiếp HTTP Request đến WordPress.

3\. WordPress xử lý yêu cầu và truy vấn MySQL.

4\. MySQL trả dữ liệu về WordPress.

5\. WordPress gửi phản hồi thông qua Nginx.

6\. Người quản trị sử dụng phpMyAdmin để kiểm tra và quản lý cơ sở dữ liệu.



\---



\## 4. Chức năng hệ thống Blog/CMS



Hệ thống sử dụng WordPress với các chức năng:



\- Đăng nhập và quản trị website.

\- Tạo và xuất bản bài viết.

\- Chỉnh sửa và xóa bài viết.

\- Quản lý danh mục.

\- Quản lý người dùng.

\- Phân quyền người dùng theo vai trò.

\- Quản lý nội dung và hình ảnh.

\- Tìm kiếm bài viết.



\---



\## 5. Cấu trúc thư mục dự án



```text

blog-cms-de7/

|

|-- docker-compose.yml

|-- .env.example

|-- .gitignore

|-- README.md

|

|-- nginx/

|   |-- conf.d/

|       |-- default.conf

|

|-- prometheus/

|-- grafana/

|   |-- dashboards/

|   |-- provisioning/

|       |-- datasources/

|

|-- loki/

|-- promtail/

|-- database/

|-- scripts/

|-- docs/

&#x20;   |-- screenshots/

```



Các thư mục Prometheus, Grafana, Loki và Promtail được chuẩn bị cho những giai đoạn triển khai tiếp theo.



WordPress sử dụng Docker Image chính thức. Dữ liệu phát sinh trong quá trình sử dụng được lưu trong Docker Volumes, không đưa lên GitHub.



\---



\## 6. Yêu cầu môi trường



Hệ thống được triển khai và kiểm thử trên:



\- Windows 11.

\- Docker Desktop.

\- WSL 2.

\- Docker Compose.

\- Git.

\- Google Chrome.



Docker Desktop cần được khởi động ở chế độ Linux Containers.



\---



\## 7. Hướng dẫn triển khai



\### Bước 1. Clone Repository



Mở PowerShell và chạy:



```powershell

git clone https://github.com/phnglnh1526/DTC245200004-blog-cms-de7.git

```



Di chuyển vào thư mục:



```powershell

cd DTC245200004-blog-cms-de7

```



\### Bước 2. Cấu hình biến môi trường



Tạo file `.env` từ file mẫu:



```powershell

Copy-Item .env.example .env

```



Mở file:



```powershell

notepad .env

```



Thay các mật khẩu mẫu bằng mật khẩu mạnh.



Các biến cần thiết:



```env

MYSQL\_ROOT\_PASSWORD=CHANGE\_ME\_ROOT\_PASSWORD

MYSQL\_DATABASE=blogcms

MYSQL\_USER=bloguser

MYSQL\_PASSWORD=CHANGE\_ME\_USER\_PASSWORD

```



Không sử dụng nguyên các mật khẩu mẫu khi triển khai.



\### Bước 3. Kiểm tra Docker Compose



```powershell

docker compose config --quiet

```



Nếu không có lỗi, tiếp tục triển khai.



\### Bước 4. Khởi động hệ thống



```powershell

docker compose up -d

```



Kiểm tra trạng thái:



```powershell

docker compose ps

```



\### Bước 5. Cài đặt WordPress lần đầu



Truy cập:



```text

http://localhost:8080

```



Nếu là lần khởi chạy mới, WordPress sẽ hiển thị giao diện cài đặt.



Thực hiện:



1\. Chọn ngôn ngữ Tiếng Việt.

2\. Nhập tên website.

3\. Tạo tài khoản quản trị.

4\. Thiết lập mật khẩu mạnh.

5\. Nhập email quản trị.

6\. Hoàn tất cài đặt WordPress.



\---



\## 8. Địa chỉ truy cập



| Dịch vụ | Địa chỉ |

|---|---|

| Website Blog/CMS | http://localhost:8080 |

| WordPress Admin | http://localhost:8080/wp-admin |

| phpMyAdmin | http://localhost:8081 |



MySQL chỉ được truy cập qua mạng Docker nội bộ.



WordPress không công bố trực tiếp cổng HTTP ra máy Windows.



\---



\## 9. Nginx Reverse Proxy



Nginx được sử dụng để chuyển tiếp HTTP Request đến WordPress.



Địa chỉ truy cập chính:



```text

http://localhost:8080

```



Nginx chuyển request đến:



```text

http://wordpress:80

```



\### Security Headers



Hệ thống sử dụng các HTTP Security Headers:



```nginx

add\_header X-Content-Type-Options "nosniff" always;

add\_header X-Frame-Options "SAMEORIGIN" always;

add\_header Referrer-Policy "strict-origin-when-cross-origin" always;

```



Kiểm tra bằng PowerShell:



```powershell

curl.exe -I http://localhost:8080

```



Cấu hình Nginx được lưu tại:



```text

nginx/conf.d/default.conf

```



\---



\## 10. Bảo mật hệ thống



Các biện pháp bảo mật đã được cấu hình ở giai đoạn đầu:



\- Sử dụng Nginx Unprivileged Image.

\- Phân chia mạng frontend và backend.

\- Không công khai cổng MySQL ra host.

\- Không công khai trực tiếp cổng WordPress.

\- Sử dụng HTTP Security Headers.

\- Sử dụng mật khẩu riêng cho Database.

\- Không đưa file `.env` chứa mật khẩu lên GitHub.



Các biện pháp bảo mật sẽ tiếp tục được kiểm thử và hoàn thiện ở giai đoạn Hardening.



\---



\## 11. Kiểm tra và quản lý Containers



Kiểm tra các dịch vụ:



```powershell

docker compose ps

```



Xem log:



```powershell

docker compose logs

```



Xem log WordPress:



```powershell

docker compose logs wordpress

```



Xem log Nginx:



```powershell

docker compose logs nginx

```



Dừng hệ thống:



```powershell

docker compose down

```



Khởi động lại:



```powershell

docker compose up -d

```



Lưu ý: Không sử dụng tùy chọn `-v` với lệnh `docker compose down` nếu chưa sao lưu dữ liệu, vì tùy chọn này có thể xóa Docker Volumes của dự án.



\---



\## 12. Các giai đoạn triển khai



\### Commit 1 – Website và Reverse Proxy



\- Triển khai WordPress.

\- Triển khai MySQL.

\- Tích hợp phpMyAdmin.

\- Cấu hình Nginx Reverse Proxy.

\- Cấu hình Security Headers.

\- Quản lý cấu hình bằng GitHub.

\- Viết README.



\### Commit 2 – Monitoring



Dự kiến triển khai:



\- Prometheus.

\- Grafana.

\- cAdvisor.

\- Nginx Exporter.

\- MySQL Exporter.

\- Dashboard giám sát Container, Web Server và Database.



\### Commit 3 – Centralized Logging



Dự kiến triển khai:



\- Loki.

\- Promtail.

\- Tích hợp Loki với Grafana.

\- Truy vấn Logs bằng LogQL.

\- Hoàn thiện Hardening hệ thống.



\---



\## 13. Kiểm thử



Các nội dung kiểm thử bao gồm:



\- Truy cập website Blog/CMS.

\- Đăng nhập WordPress Administrator.

\- Tạo và xuất bản bài viết.

\- Quản lý danh mục.

\- Kiểm tra dữ liệu bài viết bằng phpMyAdmin.

\- Kiểm tra kết nối MySQL.

\- Kiểm tra Nginx Reverse Proxy.

\- Kiểm tra Security Headers.

\- Kiểm tra trạng thái Docker Containers.

\- Kiểm tra dữ liệu sau khi khởi động lại Containers.



Các giai đoạn tiếp theo sẽ bổ sung kiểm thử Metrics, Dashboard, LogQL và Hardening.



\---



\## 14. Kết luận



Dự án hướng đến xây dựng một hệ thống Blog/CMS hoàn chỉnh, có khả năng triển khai và vận hành bằng Docker Compose.



Việc tích hợp Nginx, Prometheus, Grafana và Loki giúp hệ thống đáp ứng các yêu cầu về Reverse Proxy, giám sát, quản lý log và bảo mật trong học phần Triển khai và Quản trị Hệ thống Phần mềm.

\## Centralized Logging – Loki \& Promtail



\### 1. Giới thiệu



Hệ thống Blog CMS sử dụng Loki và Promtail để thu thập, lưu trữ và truy vấn log tập trung từ các Docker Container.



Các thành phần:



\- \*\*Promtail:\*\* Thu thập log từ Docker Containers.

\- \*\*Loki:\*\* Tiếp nhận và lưu trữ log.

\- \*\*Grafana:\*\* Hiển thị và tìm kiếm log bằng LogQL.



\### 2. Khởi động hệ thống



Chạy:



```powershell

docker compose up -d

```



Kiểm tra trạng thái:



```powershell

docker compose ps

```



Kiểm tra Loki:



```powershell

curl.exe -i http://localhost:3100/ready

```



Kết quả mong đợi: `HTTP/1.1 200 OK`.



\### 3. Truy cập Grafana



Địa chỉ:



http://localhost:3000



Vào \*\*Explore\*\*, chọn Data Source `loki`.



\### 4. Truy vấn LogQL



\*\*Log WordPress:\*\*



```logql

{job="blog-cms", service="wordpress"}

```



\*\*Log MySQL:\*\*



```logql

{job="blog-cms", service="db"}

```



\*\*Log Nginx:\*\*



```logql

{job="blog-cms", service="nginx"}

```



\*\*Lọc HTTP GET của Nginx:\*\*



```logql

{job="blog-cms", service="nginx"} |= "GET"

```



\*\*Lọc các thông báo lỗi của MySQL:\*\*



```logql

{job="blog-cms", service="db"} |\~ "(?i)error"

```



\### 5. Kết quả kiểm thử



\- WordPress: Đã truy vấn thành công log trong Grafana.

\- Nginx: Đã truy vấn thành công log HTTP trong Grafana.

\- MySQL: Docker có log khởi động, nhưng chưa xác nhận được các dòng log MySQL trong kết quả truy vấn Grafana tại thời điểm kiểm thử.



Lưu ý: MySQL không mặc định ghi mọi truy vấn SQL ra Docker logs. Vì vậy, việc không có log mới không đồng nghĩa MySQL ngừng hoạt động.



\### 6. Lưu ý bảo mật



Loki chỉ mở cổng `3100` trên `127.0.0.1` của máy chạy Docker.



Promtail sử dụng Docker socket để thu thập log. Docker socket là tài nguyên có quyền truy cập cao, chỉ phù hợp với môi trường thực hành đáng tin cậy trong cấu hình hiện tại.



Không đưa file `.env` chứa mật khẩu thật lên GitHub.

