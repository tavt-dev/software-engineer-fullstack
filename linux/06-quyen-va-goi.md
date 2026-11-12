# 6. Quyền truy cập và gói phần mềm

[← Mục lục](README.md) · [← Thao tác tệp](05-tep-va-thu-muc.md) · [Tiếp: Kiểm tra hệ thống →](07-kiem-tra-he-thong.md)

## Chạy lệnh quản trị với `sudo`

**`root`** là tài khoản quản trị có quyền cao nhất trên hệ thống. **`sudo`** cho phép người dùng đã được cấp quyền chạy một lệnh dưới quyền tài khoản khác, mặc định là `root`.

Bạn thường dùng `sudo` khi cài phần mềm, quản lý dịch vụ hoặc sửa cấu hình hệ thống.

Khi được hỏi, thông thường bạn nhập **mật khẩu tài khoản đang dùng**. Terminal không hiển thị ký tự khi nhập; gõ xong thì nhấn `Enter`.

## Đọc và sửa quyền truy cập

Dùng `ls -l config.txt` để xem quyền và chủ sở hữu. Ví dụ phần quyền `-rw-r--r--`:

- Dấu `-` đầu tiên chỉ tệp thường.
- Ba nhóm `rw-`, `r--`, `r--` lần lượt là quyền của **chủ sở hữu, nhóm và người khác**. Dấu `-` trong mỗi nhóm nghĩa là không có quyền đó.

| Quyền | Với tệp | Với thư mục |
| --- | --- | --- |
| `r` (read) | Đọc nội dung. | Xem tên các mục bên trong. |
| `w` (write) | Sửa nội dung. | Tạo hoặc xóa mục bên trong, cần kèm quyền `x`. |
| `x` (execute) | Chạy như chương trình. | Đi vào và truy cập các mục qua thư mục đó. |

| Ví dụ | Tác dụng |
| --- | --- |
| `chmod u+x start.sh` | Thêm quyền thực thi cho chủ sở hữu (`u`) của script. |
| `./start.sh` | Chạy script trong thư mục hiện tại sau khi có quyền thực thi. |
| `sudo chown user01 config.txt` | Đổi chủ sở hữu tệp thành tài khoản `user01`. |

Khi gặp `Permission denied`, kiểm tra đúng tài khoản, chủ sở hữu và quyền trước khi thay đổi. `chmod` sửa quyền; `chown` đổi chủ sở hữu.

## Quản lý gói bằng APT

**APT** là công cụ cài đặt, nâng cấp và gỡ các gói phần mềm trên Ubuntu/Debian.

| Lệnh | Tác dụng |
| --- | --- |
| `sudo apt update` | Cập nhật danh sách gói và phiên bản có sẵn, chưa nâng cấp phần mềm. |
| `sudo apt upgrade` | Cài các bản cập nhật cho gói đã có. |
| `sudo apt install tree` | Cài gói `tree`. |
| `sudo apt remove tree` | Gỡ gói `tree`. |

Chạy `sudo apt update` trước khi cài hoặc nâng cấp để lấy danh sách gói mới.

## Đổi mật khẩu với `passwd`

`passwd` dùng để đổi mật khẩu tài khoản người dùng.

| Lệnh | Tác dụng |
| --- | --- |
| `passwd` | Đổi mật khẩu của bạn. |
| `sudo passwd user01` | Đặt lại mật khẩu cho người dùng `user01`. |
| `sudo passwd root` | Đặt mật khẩu cho tài khoản `root`. |

Với `passwd`, bạn nhập mật khẩu hiện tại, rồi nhập mật khẩu mới hai lần để xác nhận. Khi dùng `sudo passwd user01`, bạn đặt mật khẩu mới cho `user01` mà không cần biết mật khẩu cũ của tài khoản đó.

Trên Ubuntu, mật khẩu `root` bị khóa mặc định; việc quản trị thường thực hiện qua `sudo`. Đặt mật khẩu cho `root` sẽ cho phép tài khoản này xác thực bằng mật khẩu.
