# 6. Quyền quản trị và gói phần mềm

[← Mục lục](README.md) · [← Thao tác tệp](05-tep-va-thu-muc.md)

## Lệnh `sudo`

`sudo` cho phép người dùng đã được cấp quyền chạy một lệnh dưới quyền của người dùng khác, mặc định là `root`.

Lệnh này thường dùng để cài hoặc gỡ gói phần mềm, quản lý dịch vụ và chỉnh sửa tệp cấu hình hệ thống. Chính sách `sudoers` giúp giới hạn người dùng nào được chạy lệnh nào với quyền quản trị.

Khi cần xác thực, `sudo` thường hỏi **mật khẩu của người dùng hiện tại**, không phải mật khẩu `root`; lúc nhập, Terminal thường không hiển thị ký tự.

`sudo` có thể tạm ghi nhớ lần xác thực, nên không nhất thiết hỏi mật khẩu ở mỗi lần chạy. Việc có hỏi mật khẩu hay không phụ thuộc vào cấu hình hệ thống.

**Ví dụ trên Ubuntu/Debian (dùng APT):**

- `sudo apt update`: Làm mới danh sách gói và phiên bản có sẵn từ kho phần mềm; chưa nâng cấp các gói đã cài.
- `sudo apt upgrade`: Nâng cấp các gói đã cài đủ điều kiện lên phiên bản mới có sẵn. APT sẽ hiển thị các thay đổi và có thể yêu cầu xác nhận.

## Đặt mật khẩu với `passwd`

`passwd` dùng để đổi mật khẩu tài khoản người dùng. Cú pháp:

```text
passwd [tên_người_dùng]
```

Không ghi tên người dùng thì lệnh đổi mật khẩu của chính mình. Để đặt lại mật khẩu cho tài khoản khác cần quyền quản trị, thường dùng `sudo passwd <tên_người_dùng>`. Dấu `[]` nghĩa là tham số tùy chọn; phần trong `<>` cần thay bằng tên tài khoản, không gõ các dấu này.

### Đặt mật khẩu cho tài khoản `root`

`root` là tài khoản có quyền quản trị cao nhất. Trên Ubuntu, mật khẩu của `root` bị khóa theo mặc định và các tác vụ quản trị thường được thực hiện qua `sudo`. Nếu cần đặt mật khẩu cho `root`, người dùng có quyền quản trị dùng:

```bash
sudo passwd root
```

Lệnh có thể yêu cầu xác thực người dùng hiện tại, sau đó nhập và xác nhận mật khẩu mới cho `root`. Việc đặt mật khẩu sẽ mở khóa khả năng xác thực bằng mật khẩu của tài khoản `root`.
