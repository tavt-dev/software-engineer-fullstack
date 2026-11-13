# 3. Lưu dữ liệu với volume và bind mount

[← Mục lục](README.md) · [← Các lệnh cơ bản](02-lenh-co-ban.md) · [Tiếp: Env và network →](04-env-va-network.md)

## Web app thực tế cần những gì?

Một hệ thống có thể gồm Nginx, backend và MySQL, cùng dữ liệu và cấu hình của chúng. Để các thành phần hoạt động cùng nhau, cần hiểu:

| Thành phần | Vai trò |
| --- | --- |
| Volume | Giữ dữ liệu độc lập với vòng đời container. |
| Bind mount | Đưa tệp hoặc thư mục có sẵn trên host vào container. |
| Environment variable | Truyền cấu hình khi chạy, ví dụ mật khẩu khởi tạo MySQL. |
| Network | Cho phép các container kết nối với nhau, như backend gọi MySQL. |

Với dữ liệu quan trọng, cần biết **dữ liệu được lưu ở đâu và làm sao dùng lại khi thay container**.

## Volume là gì?

**Volume** là vùng lưu trữ do Docker quản lý, có vòng đời riêng với container. Khi gắn volume vào thư mục dữ liệu của ứng dụng, dữ liệu được ghi vào volume.

Ví dụ, MySQL ghi dữ liệu ở `/var/lib/mysql`. Gắn một volume có tên vào đó giúp bạn dùng lại dữ liệu khi tạo container MySQL khác.

## Bind mount là gì?

**Bind mount** gắn trực tiếp một tệp hoặc thư mục trên host vào container. Nó phù hợp khi dev muốn sửa mã nguồn, HTML hoặc cấu hình trên host và để container sử dụng những tệp đó.

### Ví dụ: Nginx phục vụ HTML trên host

Các lệnh dưới đây dùng **Bash trên Linux**, với cổng host `8080` còn trống. Dấu `\` cuối dòng nối các dòng thành một lệnh.

Tạo thư mục và trang web:

```bash
mkdir -p ~/05112026
echo '<h1>Hello Docker</h1>' > ~/05112026/index.html
```

Chạy Nginx và gắn thư mục đó vào nơi Nginx đọc nội dung web:

```bash
docker run -d --name web-demo05 \
  -p 8080:80 \
  -v "$HOME/05112026:/usr/share/nginx/html:ro" \
  nginx:latest
```

Trong `-v`:

- `$HOME/05112026` là thư mục trên host, tương ứng `~/05112026`.
- `/usr/share/nginx/html` là thư mục web bên trong container.
- `:ro` cho container đọc dữ liệu, không ghi sửa vào thư mục host qua mount này.

Mở `http://localhost:8080`, hoặc gọi `curl http://localhost:8080`.

Thử sửa HTML trên host:

```bash
echo '<h1>Da cap nhat tu host</h1>' > ~/05112026/index.html
curl http://localhost:8080
```

Với trang HTML tĩnh này, nội dung mới được Nginx phục vụ ngay, không cần build lại image. File vẫn nằm trên host, dù container nhìn thấy nó qua đường dẫn bên trong.

## Volume khác bind mount thế nào?

| Tiêu chí | Named volume | Bind mount |
| --- | --- | --- |
| Chọn nơi lưu | Docker quản lý; bạn dùng tên volume. | Bạn chỉ rõ đường dẫn trên host. |
| Phù hợp | Dữ liệu database hoặc dữ liệu ứng dụng cần giữ lại. | Mã nguồn, HTML, cấu hình cần sửa trực tiếp trên host. |
| Xóa container | Volume vẫn còn. | Tệp và thư mục host vẫn còn. |

Cả hai đều lưu dữ liệu ngoài lớp ghi riêng của container. Với Docker Desktop, vùng lưu trữ volume do Docker quản lý thường nằm trong môi trường Linux của Docker Desktop.

## Chạy MySQL với named volume

Ví dụ dùng volume mới `mysql-0611`, container `mysql-demo` và cổng host `3306` còn trống. Mật khẩu bên dưới chỉ là ví dụ cho bài lab. Dùng cùng tag `mysql:8.4` khi tạo và tạo lại container trong bài này.

### Tạo volume và chạy container

```bash
docker volume create mysql-0611
docker run -d --name mysql-demo \
  -e MYSQL_ROOT_PASSWORD='LabOnly_0611' \
  -v mysql-0611:/var/lib/mysql \
  -p 127.0.0.1:3306:3306 \
  mysql:8.4
```

| Thành phần | Ý nghĩa |
| --- | --- |
| `--name mysql-demo` | Tên container. |
| `-e MYSQL_ROOT_PASSWORD=...` | Truyền biến môi trường để đặt mật khẩu root khi khởi tạo dữ liệu mới. |
| `-v mysql-0611:/var/lib/mysql` | Gắn đúng volume `mysql-0611` vào thư mục MySQL lưu dữ liệu. |
| `-p 127.0.0.1:3306:3306` | Công bố cổng MySQL để truy cập từ localhost của host. |

Đường dẫn là **`/var/lib/mysql`**, viết thường. Tên ở `docker volume create` và phần trước dấu `:` của `-v` phải khớp nhau.

Xem volume đã tạo bằng `docker volume ls`; xem thông tin của nó bằng `docker volume inspect mysql-0611`.

Image MySQL chính thức khai báo volume cho `/var/lib/mysql`. Nếu không chỉ định mount, Docker tạo volume ẩn danh; container mới không tự gắn lại volume ẩn danh cũ. Đặt tên volume giúp bạn chủ động tìm và dùng lại dữ liệu.

### Kiểm chứng dữ liệu còn sau khi tạo lại container

Chờ MySQL khởi động xong, kiểm tra bằng `docker logs mysql-demo`. Mở MySQL client:

```bash
docker exec -it mysql-demo mysql -u root -p
```

Nhập mật khẩu lab đã đặt. Trong MySQL, tạo một database để kiểm tra:

```sql
CREATE DATABASE volume_lab;
SHOW DATABASES;
exit;
```

Dừng và xóa container, giữ nguyên volume:

```bash
docker stop mysql-demo
docker rm mysql-demo
```

Tạo lại container, gắn **đúng volume cũ**:

```bash
docker run -d --name mysql-demo \
  -e MYSQL_ROOT_PASSWORD='LabOnly_0611' \
  -v mysql-0611:/var/lib/mysql \
  -p 127.0.0.1:3306:3306 \
  mysql:8.4
```

Chờ MySQL chạy xong, mở client bằng lệnh `docker exec` ở trên rồi chạy:

```sql
SHOW DATABASES;
exit;
```

Database `volume_lab` vẫn xuất hiện vì dữ liệu nằm trong volume `mysql-0611`.

`MYSQL_ROOT_PASSWORD` chỉ thiết lập mật khẩu khi khởi tạo dữ liệu mới. Đổi giá trị `-e` không tự đổi mật khẩu của database đã tồn tại trong volume.

## Dữ liệu còn hay mất?

| Thao tác | Dữ liệu trong named volume |
| --- | --- |
| Restart container | Vẫn còn. |
| Stop rồi xóa container | Vẫn còn trong volume. |
| Tạo container mới và gắn lại đúng volume | Dùng lại dữ liệu cũ. |
| Xóa volume | Mất dữ liệu lưu trong volume đó. |

**Volume không thay thế backup.** Nó tách dữ liệu khỏi vòng đời container; dữ liệu quan trọng vẫn cần được sao lưu để phục hồi khi vùng lưu trữ bị xóa hoặc hỏng.

**Tham khảo:** [Docker volumes](https://docs.docker.com/engine/storage/volumes/) · [Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/) · [MySQL Official Image](https://hub.docker.com/_/mysql).
