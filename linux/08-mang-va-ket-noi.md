# 8. Kiểm tra IP và kết nối mạng

[← Mục lục](README.md) · [← Kiểm tra hệ thống](07-kiem-tra-he-thong.md) · [Tiếp: Shell script →](09-shell-script.md)

## Xem nhanh IP với `hostname -I`

```bash
hostname -I
```

In nhanh các IP của máy. Chữ `I` viết hoa.

Dùng `ip a` khi cần xem chi tiết IP và trạng thái của từng card mạng.

## Kiểm tra kết nối với `ping`

```bash
ping -c 4 8.8.8.8
```

Thử nhận phản hồi từ một IP trên Internet. `-c 4` gửi 4 gói rồi dừng.

## Xem cổng đang mở với `ss`

```bash
ss -tuln
```

Xem các cổng TCP/UDP đang chờ nhận kết nối hoặc dữ liệu, ví dụ kiểm tra backend đã mở cổng `8080` chưa.

Các tùy chọn: `-t` TCP, `-u` UDP, `-l` các socket đang chờ nhận, `-n` hiện địa chỉ và cổng dạng số. Thêm `-p` thành `ss -tulnp` để xem tiến trình khi có quyền xem.

`127.0.0.1:8080` chỉ nhận qua loopback trên chính máy đó; `0.0.0.0:8080` lắng nghe trên mọi địa chỉ IPv4 cục bộ. Khi gọi từ máy khác, kiểm tra cả địa chỉ lắng nghe và firewall.

## Gọi thử API với `curl`

| Lệnh | Tác dụng |
| --- | --- |
| `curl http://localhost:8080` | Gửi GET tới backend local, xem nội dung trả về. |
| `curl -i http://localhost:8080` | Xem thêm mã trạng thái và header để kiểm tra lỗi API. |
| `curl -I https://facebook.com` | Gửi HEAD tới website, chỉ xem mã trạng thái và header. |

Dùng để test API local hoặc xem website có trả phản hồi HTTP mà không cần mở trình duyệt. `-i` hiện thêm header; `-I` viết hoa gửi yêu cầu HEAD.

`localhost` trỏ về chính máy đang gọi. Từ Windows muốn gọi backend trong máy ảo Linux, dùng IP máy ảo, ví dụ `http://192.168.1.20:8080`.

## Kết nối server bằng `ssh`

```bash
ssh user01@192.168.1.20
```

Đăng nhập vào máy có IP `192.168.1.20` bằng tài khoản `user01`. Thay tên và IP bằng thông tin server của bạn; máy đích cần có dịch vụ SSH đang chạy và bạn cần cách xác thực hợp lệ.

Sau khi đăng nhập, các lệnh chạy trên **server**. Dùng `hostname`, `whoami`, `pwd` để kiểm tra; nhập `exit` để kết thúc phiên SSH.

## Chép tệp lên server bằng `scp`

Chạy từ máy đang chứa tệp:

```bash
scp config.txt user01@192.168.1.20:/home/user01/
```

Sao chép `config.txt` từ máy hiện tại vào `/home/user01/` trên server. Dùng khi chuyển tệp cấu hình hoặc bản build; cần có quyền ghi ở thư mục đích.
