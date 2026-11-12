# 7. Kiểm tra tài khoản và hệ thống

[← Mục lục](README.md) · [← Quyền và gói](06-quyen-va-goi.md) · [Tiếp: IP và kết nối mạng →](08-mang-va-ket-noi.md)

Khi làm việc trên server, cần biết **mình đang dùng tài khoản nào, trên máy nào và thời gian của máy đó**. Các thông tin này giúp bạn kiểm tra đúng môi trường và đối chiếu sự cố.

## Các lệnh thường dùng

| Lệnh | Cho biết | Dùng trong công việc |
| --- | --- | --- |
| `whoami` | Tên tài khoản đang thực thi lệnh. | Kiểm tra đang dùng user thường hay `root` khi gặp lỗi quyền truy cập. |
| `hostname` | Tên máy. | Xác nhận đang thao tác trên đúng server hoặc máy ảo. |
| `uname -a` | Thông tin kernel, tên máy và kiến trúc hệ thống. | Kiểm tra môi trường khi tìm nguyên nhân phần mềm không tương thích. |
| `date` | Ngày, giờ và múi giờ hiển thị của hệ thống. | Đối chiếu thời gian xảy ra lỗi với log ứng dụng. |
| `date -u` | Thời gian hiển thị theo UTC. | So sánh thời gian khi log hoặc hệ thống khác dùng UTC. |

## Kiểm tra thời gian khi debug

Giả sử bạn gọi API lúc 10:00 nhưng thấy log ghi 03:00. Hãy kiểm tra múi giờ: **10:00 ở UTC+7 tương ứng 03:00 UTC**.

Khi đối chiếu log, quy về cùng múi giờ. Đồng hồ hoặc cấu hình múi giờ sai có thể làm lệch thời gian ghi nhận sự kiện và ảnh hưởng tác vụ chạy theo lịch (cron).

## Kiểm tra CPU, RAM và tiến trình

**Tiến trình (process)** là một chương trình đang chạy, được nhận diện bằng số **PID**.

| Lệnh | Dùng khi nào? |
| --- | --- |
| `top` | Xem CPU, RAM và các tiến trình khi ứng dụng chậm hoặc máy quá tải. Nhấn `q` để thoát. |
| `free -h` | Xem tổng RAM và lượng có thể dùng thêm ở cột `available`. |

### Tìm process của backend

Ví dụ backend viết bằng Python:

```bash
ps aux | grep python
```

`ps aux` liệt kê tiến trình, `grep python` lọc các dòng có chữ `python`. Xem cột **USER, PID và COMMAND** để nhận ra đúng script hoặc đường dẫn backend của bạn. Kết quả có thể chứa cả dòng của lệnh `grep python`; dòng đó không phải backend.

Thấy process chỉ cho biết chương trình còn chạy. Dùng `curl http://localhost:8080` để kiểm tra backend có trả phản hồi HTTP hay không.

### Dừng một tiến trình

**Không kill bừa.** Trước khi dừng, phải biết PID đó thuộc ứng dụng nào, do user nào chạy và dừng nó sẽ ảnh hưởng gì.

Lấy PID từ `ps`, `top` hoặc `ss -tulnp`. Ví dụ dùng `ps -p 1234 -f` để xem lại thông tin trước khi gửi yêu cầu dừng:

```bash
kill 1234
```

`1234` là PID ví dụ; thay bằng PID đã xác định của ứng dụng bạn cần dừng. Lệnh gửi tín hiệu `SIGTERM` để ứng dụng có cơ hội xử lý trước khi thoát. Nếu ứng dụng đang chạy ngay trong Terminal, thường dùng `Ctrl + C` để ngắt.

Với ứng dụng do systemd quản lý, dùng `systemctl stop` để dừng dịch vụ tương ứng.

## Quản lý dịch vụ với `systemctl`

Với dịch vụ do **systemd** quản lý, ví dụ Nginx đã được cài trên Ubuntu:

| Lệnh | Tác dụng |
| --- | --- |
| `systemctl status nginx` | Xem trạng thái dịch vụ, như `active (running)` hoặc `failed`. |
| `sudo systemctl start nginx` | Chạy dịch vụ ngay. |
| `sudo systemctl stop nginx` | Dừng dịch vụ đang chạy. |
| `sudo systemctl restart nginx` | Dừng rồi chạy lại dịch vụ; nếu đang dừng thì khởi động nó. |
| `sudo systemctl enable nginx` | Cấu hình dịch vụ tự chạy khi máy khởi động. |
| `sudo journalctl -u nginx -n 50` | Xem 50 dòng nhật ký gần nhất của dịch vụ trong systemd journal. |

**Phân biệt:** `start` chạy ngay; `enable` bật tự chạy khi boot và không tự khởi động một dịch vụ đang dừng.

Khi Docker Engine hoặc MySQL được cài dưới dạng service, dùng tên dịch vụ `docker` hoặc `mysql` tương ứng thay cho `nginx`.

Trong `journalctl`, `-u` chọn dịch vụ, `-n 50` lấy 50 dòng cuối. Nhấn `q` để thoát màn hình xem. Khi dịch vụ không khởi động được, dùng `status` và log để tìm nguyên nhân.
