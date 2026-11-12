# 3. Terminal và cách dùng lệnh

[← Mục lục](README.md) · [← Hệ thống tệp](02-he-thong-tep.md) · [Tiếp: Điều hướng →](04-dieu-huong.md)

**Terminal** là ứng dụng để làm việc với máy tính bằng dòng lệnh. Bạn nhập lệnh trong Terminal; shell, chẳng hạn Bash, đọc và thực hiện lệnh đó.

## Mở và thoát Terminal

- **Dùng chuột:** Trên Ubuntu Desktop, mở danh sách ứng dụng, tìm và chọn **Terminal**.
- **Dùng bàn phím:** Nhấn `Ctrl + Alt + T`.
- Gõ lệnh rồi nhấn `Enter` để chạy.
- Nhập `exit` để kết thúc phiên shell hiện tại.

## Đọc dấu nhắc Terminal

Ví dụ dòng chờ nhập lệnh (prompt):

```text
ta-van-tien@ta-van-tien-VMware-Virtual-Platform:~/Desktop$
```

| Thành phần | Ý nghĩa |
| --- | --- |
| `ta-van-tien` trước dấu `@` | Tên tài khoản đang dùng. |
| `ta-van-tien-VMware-Virtual-Platform` | Tên máy (hostname). |
| `~/Desktop` | Thư mục hiện tại; `~` là thư mục cá nhân của bạn. |
| `$` | Theo quy ước, phiên shell của người dùng thường. |
| `#` | Theo quy ước, phiên shell của `root`. |

Dấu nhắc giúp bạn nhìn nhanh mình đang dùng tài khoản nào, trên máy nào và ở thư mục nào. Khi nhập lệnh, không gõ lại dấu `$` hoặc `#`. Dùng `whoami` để kiểm tra tài khoản thực tế đang chạy lệnh.

## Cú pháp lệnh

Cấu trúc thường gặp:

```text
command [options] [arguments]
```

Phần trong dấu `[]` có thể có hoặc không; không gõ dấu ngoặc khi chạy lệnh. Ví dụ:

```bash
ls -la /home
```

| Thành phần | Trong ví dụ | Ý nghĩa |
| --- | --- | --- |
| `command` | `ls` | **Làm gì?** Liệt kê tệp và thư mục. |
| `options` | `-la` | **Làm theo cách nào?** Xem chi tiết (`-l`), gồm cả mục ẩn (`-a`). |
| `arguments` | `/home` | **Áp dụng lên đâu?** Thư mục `/home`. |

Nếu tên tệp hoặc thư mục có dấu cách, đặt đường dẫn trong dấu nháy, ví dụ `cd "My Project"`.

## Phím tắt thường dùng

### Điều khiển phiên shell

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + C` | Ngắt lệnh đang chạy. |
| `Ctrl + D` | Thoát phiên shell khi dòng nhập lệnh đang trống. |

### Nhập và sửa lệnh

Trong Bash, bạn có thể dùng:

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + A` | Đưa con trỏ về đầu dòng. |
| `Ctrl + E` | Đưa con trỏ đến cuối dòng. |
| `Ctrl + U` | Xóa từ vị trí con trỏ về đầu dòng. |
| `Ctrl + K` | Xóa từ vị trí con trỏ đến cuối dòng. |
| `Alt + B` | Di chuyển sang trái một từ. |
| `Alt + F` | Di chuyển sang phải một từ. |

### Lịch sử và hoàn thành tự động

| Phím tắt | Tác dụng |
| --- | --- |
| `↑` / `↓` | Duyệt các lệnh đã nhập. |
| `Ctrl + R` | Tìm lệnh cũ: nhấn phím tắt rồi gõ một phần của lệnh. |
| `Tab` | Tự hoàn thành tên lệnh hoặc đường dẫn khi có thể. |

### Cửa sổ, sao chép và dán

Trong GNOME Terminal:

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + Shift + T` | Mở tab mới. |
| `Ctrl + Shift + W` | Đóng tab hiện tại. |
| `Ctrl + Shift + C` | Sao chép văn bản đã chọn. |
| `Ctrl + Shift + V` | Dán văn bản đã sao chép. |
| `Ctrl + +` | Tăng kích thước chữ. |
| `Ctrl + -` | Giảm kích thước chữ. |

### Xem lại lệnh và dọn màn hình

| Lệnh | Tác dụng |
| --- | --- |
| `history` | Xem lịch sử lệnh trong Bash, hữu ích khi cần kiểm tra lại thao tác đã làm. |
| `history 10` | Xem 10 lệnh gần nhất trong lịch sử. |
| `clear` | Dọn phần hiển thị trong Terminal để dễ đọc; không xóa lịch sử lệnh. |

## Chuyển hướng dữ liệu và nối lệnh

| Ký hiệu | Tác dụng | Ví dụ |
| --- | --- | --- |
| `>` | Ghi đầu ra vào tệp, **ghi đè** nội dung cũ. | `echo "Hello" > notes.txt` |
| `>>` | Ghi thêm đầu ra vào **cuối tệp**. | `echo "Linux" >> notes.txt` |
| `<` | Đọc dữ liệu từ tệp làm đầu vào cho lệnh. | `wc -l < notes.txt` |
| <code>&#124;</code> (pipe) | Chuyển đầu ra của lệnh bên trái thành đầu vào của lệnh bên phải. | <code>cat notes.txt &#124; wc -l</code> |
| `&&` | Chỉ chạy lệnh sau khi lệnh trước thành công. | `mkdir demo && cd demo` |

`>` và `>>` tạo tệp nếu chưa có. Trong các ví dụ trên, `echo` in một dòng văn bản, `cat` đọc nội dung tệp và `wc -l` đếm số dòng.

## Lệnh `man`

`man` (*manual*) mở trang hướng dẫn sử dụng lệnh. Ví dụ, xem cách dùng `ls`:

```bash
man ls
```

Trong trang hướng dẫn:

| Phím | Tác dụng |
| --- | --- |
| `↑` / `↓` | Cuộn từng dòng. |
| `Page Up` / `Page Down` | Cuộn từng trang. |
| `q` | Thoát về dòng lệnh. |
