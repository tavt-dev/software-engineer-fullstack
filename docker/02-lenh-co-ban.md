# 2. Các lệnh Docker cơ bản

[← Mục lục](README.md) · [← Tổng quan Docker](01-tong-quan.md) · [Tiếp: Volume và bind mount →](03-volume-va-bind-mount.md)

Ví dụ dùng Nginx để học cách chạy một dịch vụ web bằng container.

## Kiểm tra Docker

Docker cần được cài và Docker Engine đang chạy. Nếu dùng Docker Desktop, mở ứng dụng trước khi thực hành.

```bash
docker version
```

Kết quả cần có cả thông tin **Client** và **Server**. Ví dụ dưới đây dùng container Linux.

## Kéo image về máy

```bash
docker pull nginx
```

Tải image `nginx:latest` từ Docker Hub về máy. Lệnh chỉ tải image, chưa tạo hoặc chạy container.

## Xem các image đã có

```bash
docker images
```

Xem tên image, tag, ID và kích thước được liệt kê.

## Tạo và chạy container Nginx

```bash
docker run -d -p 8080:80 --name 03112026 nginx:latest
```

| Thành phần | Ý nghĩa |
| --- | --- |
| `docker run` | Tạo container mới từ image rồi khởi động nó. |
| `-d` | Chạy nền, trả lại dòng nhập lệnh cho bạn. |
| `-p 8080:80` | Chuyển tiếp cổng `8080` của host vào cổng `80` trong container. |
| `--name 03112026` | Đặt tên container là `03112026` để dùng ở các lệnh sau. |
| `nginx:latest` | Image và tag dùng để tạo container. |

### Hiểu ánh xạ cổng

Trong `-p HOST_PORT:CONTAINER_PORT`, số bên trái là cổng trên host, số bên phải là cổng dịch vụ bên trong container:

```text
http://localhost:8080 → cổng 8080 trên host → cổng 80 trong container → Nginx
```

Mở `http://localhost:8080` trên máy chạy Docker hoặc thử:

```bash
curl http://localhost:8080
```

Nếu Docker được cài trong máy ảo Linux, từ Windows dùng IP máy ảo, ví dụ `http://192.168.1.20:8080`.

`-p 8080:80` công bố cổng trên mọi giao diện mạng của host theo mặc định. Nếu chỉ cần truy cập từ chính host, dùng `-p 127.0.0.1:8080:80`.

## Xem container

| Lệnh | Tác dụng |
| --- | --- |
| `docker ps` | Xem các container đang chạy. |
| `docker ps -a` | Xem tất cả container, gồm cả container đã dừng. |

Đọc các cột **NAMES**, **STATUS** và **PORTS** để kiểm tra tên, trạng thái và ánh xạ cổng.

## Xem log container

```bash
docker logs 03112026
```

Xem log mà chương trình ghi ra đầu ra chuẩn hoặc đầu ra lỗi (stdout/stderr). Dùng `docker logs -f 03112026` để tiếp tục theo dõi log mới; nhấn `Ctrl + C` để dừng theo dõi.

## Mở shell bên trong container

```bash
docker exec -it 03112026 bash
```

- `docker exec`: Chạy thêm một lệnh trong container **đang chạy**.
- `-i`: Giữ đầu vào để bạn nhập lệnh.
- `-t`: Cấp terminal tương tác.
- `bash`: Shell được chạy bên trong container.

Bạn có thể dùng `pwd`, `ls` đã học. Nhập `exit` để thoát shell; container Nginx vẫn chạy. Nếu image không có Bash nhưng có `sh`, dùng `docker exec -it 03112026 sh`.

## Dừng và chạy lại container

Dừng container:

```bash
docker stop 03112026
```

Container vẫn còn trong `docker ps -a`. Muốn chạy lại chính container đó:

```bash
docker start 03112026
```

**`run` tạo container mới; `start` chạy lại container đã tồn tại và đang dừng.**

## Xóa container

```bash
docker stop 03112026
docker rm 03112026
```

`stop` không xóa container; `rm` xóa container, không xóa image. Các thay đổi chỉ lưu trong lớp ghi riêng của container sẽ mất khi xóa nó. Dữ liệu gắn bằng volume hoặc bind mount có vòng đời riêng, được giải thích ở bài tiếp theo.

Tên `03112026` phải chưa được container khác dùng khi chạy `docker run`. Nếu container này đã tồn tại, kiểm tra bằng `docker ps -a` và dùng `docker start` khi cần chạy lại. Nếu cổng host `8080` đang bận, chọn cổng khác, ví dụ `-p 8081:80`, rồi truy cập `http://localhost:8081`.
