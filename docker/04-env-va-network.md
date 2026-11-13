# 4. Cấu hình và kết nối container

[← Mục lục](README.md) · [← Volume và bind mount](03-volume-va-bind-mount.md)

## Biến môi trường (environment variable)

Web app cần cấu hình như cổng, database, tài khoản, mật khẩu và API URL. Các giá trị thay đổi theo môi trường nên được tách khỏi code để dễ triển khai lại.

| Môi trường | Ví dụ cấu hình |
| --- | --- |
| Local | Database trên máy dev hoặc container dùng để học. |
| Staging | Database và API dành riêng cho việc kiểm thử. |
| Production | Database, domain và thông tin xác thực của hệ thống thật. |

**Biến môi trường (env)** là một cách truyền các giá trị này vào ứng dụng khi chạy. Trong Docker, dùng `-e TEN_BIEN=gia_tri`.

Ứng dụng phải được viết hoặc cấu hình để đọc biến đó; thêm `-e` không tự thay đổi code. Env cũng không tự bảo mật mật khẩu: không đưa thông tin xác thực thật vào Git; các mật khẩu dưới đây chỉ dùng cho lab.

## Ví dụ: MySQL có cấu hình riêng

Bài này dùng **Bash trên Linux**, network `tienkientoan`, container `mysql1011` và volume mới `mysql1011-data`, tách với bài trước.

```bash
docker network create tienkientoan
docker volume create mysql1011-data
docker run -d --name mysql1011 \
  --network tienkientoan \
  -e MYSQL_ROOT_PASSWORD='LabOnly_1011' \
  -e MYSQL_DATABASE=kienpham \
  -e MYSQL_USER=kienapp \
  -e MYSQL_PASSWORD='AppOnly_1011' \
  -v mysql1011-data:/var/lib/mysql \
  mysql:8.4
```

| Biến | MySQL dùng để làm gì? |
| --- | --- |
| `MYSQL_ROOT_PASSWORD` | Đặt mật khẩu tài khoản quản trị root. |
| `MYSQL_DATABASE` | Tạo database `kienpham`. |
| `MYSQL_USER` và `MYSQL_PASSWORD` | Tạo user `kienapp` và cấp quyền trên database `kienpham`. |

Các biến trên được image MySQL dùng khi **khởi tạo dữ liệu mới**. Khi gắn lại volume đã có database, chúng không tạo lại database hoặc đổi mật khẩu cũ.

Lệnh không có `-p`: container cùng network vẫn có thể kết nối vào cổng MySQL `3306`. Chỉ cần publish cổng khi muốn truy cập từ bên ngoài network đó, chẳng hạn công cụ database trên máy host.

## Docker network là gì?

**Network** cho phép các container kết nối với nhau. Với bridge network tự tạo trên cùng Docker host, các container tham gia network có thể gọi nhau bằng **tên container**, không cần ghi cứng IP.

Chạy thêm Nginx trong network đã tạo:

```bash
docker run -d --name web1011 --network tienkientoan nginx:latest
```

Xem các container thuộc network:

```bash
docker network inspect tienkientoan
```

`--network tienkientoan` đưa container vào mạng đó. Cùng network tạo đường kết nối; từng ứng dụng vẫn cần đúng địa chỉ, cổng, tài khoản và mật khẩu.

### Kiểm tra gọi web bằng tên container

Chạy một container tạm có sẵn `curl` trong cùng network:

```bash
docker run --rm --network tienkientoan curlimages/curl:latest http://web1011:80
```

Kết quả là HTML của Nginx. `web1011` là tên container web, `80` là cổng bên trong container. `--rm` tự xóa container kiểm tra sau khi lệnh kết thúc.

### Kiểm tra kết nối MySQL

Chờ MySQL khởi tạo xong, kiểm tra bằng `docker logs mysql1011`, rồi chạy container MySQL client tạm:

```bash
docker run --rm -it --network tienkientoan mysql:8.4 \
  mysql -h mysql1011 -P 3306 -u kienapp -p kienpham
```

Nhập mật khẩu `AppOnly_1011`. Khi vào được MySQL, thử `SELECT DATABASE();` để thấy `kienpham`, rồi nhập `exit;`.

Đây cũng là cách backend trong cùng network xác định database:

```text
DB_HOST=mysql1011
DB_PORT=3306
DB_NAME=kienpham
DB_USER=kienapp
```

Các tên `DB_*` là ví dụ; dùng tên biến mà backend của bạn hỗ trợ và truyền thêm mật khẩu tương ứng.

**Trong container backend, `localhost` trỏ về chính backend, không trỏ tới MySQL.** Để gọi MySQL ở ví dụ này, dùng `mysql1011:3306`.

Với frontend, phân biệt nơi chạy yêu cầu: code phía server trong container có thể dùng tên container; JavaScript chạy trong trình duyệt phải dùng API URL mà trình duyệt truy cập được, như domain hoặc IP/cổng host.

## Ghép các phần khi triển khai

- **Dữ liệu:** Giữ trong volume, có kế hoạch backup.
- **Tệp cần sửa trên host:** Gắn bằng bind mount.
- **Cấu hình theo môi trường:** Truyền bằng env mà ứng dụng đọc được.
- **Kết nối:** Đưa các container cần giao tiếp vào cùng network, dùng đúng tên và cổng dịch vụ.

Trước khi chạy web app, xác định dữ liệu ở đâu, cấu hình ở đâu và container nào cần gọi container nào. Khi thay container, gắn lại đúng volume; khi đổi môi trường, cấp đúng cấu hình.

**Tham khảo:** [Docker bridge network](https://docs.docker.com/engine/network/drivers/bridge/) · [Biến môi trường khi chạy container](https://docs.docker.com/engine/containers/run/#environment-variables) · [MySQL Official Image](https://hub.docker.com/_/mysql).
