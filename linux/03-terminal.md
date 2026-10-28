# 3. Terminal và cách dùng lệnh

[← Mục lục](README.md) · [← Hệ thống tệp](02-he-thong-tep.md) · [Tiếp: Điều hướng →](04-dieu-huong.md)

## Mở Terminal

Trên Ubuntu Desktop, tìm **Terminal** hoặc **Terminal Emulator** trong danh sách ứng dụng (Dash/Activities). Cách mở có thể khác tùy bản phân phối và môi trường desktop.

## Thoát Terminal

Nhập `exit` để kết thúc phiên shell hiện tại. Cửa sổ Terminal thường đóng nếu đó là phiên cuối cùng.

## Phím tắt thường dùng

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + Alt + T` | Mở Terminal trên Ubuntu Desktop và một số môi trường khác. |
| `Ctrl + Shift + T` | Mở tab mới trong nhiều ứng dụng Terminal. |
| `Ctrl + D` | Kết thúc phiên shell hiện tại khi đang ở dấu nhắc trống. |
| `Ctrl + C` | Gửi tín hiệu ngắt đến tác vụ đang chạy ở phía trước; thường dùng để dừng lệnh. |

Phím tắt có thể khác tùy ứng dụng Terminal và cấu hình hệ thống.

### Điều hướng và chỉnh sửa dòng lệnh

Khi đang nhập lệnh trong Bash, các phím tắt mặc định giúp di chuyển con trỏ và sửa nội dung:

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + A` | Đưa con trỏ về đầu dòng lệnh. |
| `Ctrl + E` | Đưa con trỏ đến cuối dòng lệnh. |
| `Ctrl + U` | Xóa từ vị trí con trỏ về đầu dòng. |
| `Ctrl + K` | Xóa từ vị trí con trỏ đến cuối dòng. |
| `Alt + B` | Di chuyển con trỏ sang trái một từ. |
| `Alt + F` | Di chuyển con trỏ sang phải một từ. |

### Lịch sử và hoàn thành tự động

| Phím tắt | Tác dụng |
| --- | --- |
| `↑` / `↓` | Xem lệnh trước hoặc sau trong lịch sử. |
| `Ctrl + R` | Tìm ngược một lệnh đã nhập trong lịch sử. |
| `Tab` | Hoàn thành tên lệnh, đường dẫn hoặc đối số khi có thể. |

### Tùy chỉnh cửa sổ Terminal

Các phím tắt mặc định sau áp dụng cho GNOME Terminal:

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + +` | Tăng kích thước chữ. |
| `Ctrl + -` | Giảm kích thước chữ. |
| `Ctrl + Shift + W` | Đóng tab hiện tại. |

Tùy bố cục bàn phím, để gõ dấu `+` có thể cần giữ thêm `Shift`.

### Sao chép và dán

| Phím tắt | Tác dụng |
| --- | --- |
| `Ctrl + Shift + C` | Sao chép văn bản đã chọn. |
| `Ctrl + Shift + V` | Dán văn bản từ clipboard. |

## Cú pháp lệnh

Cấu trúc cơ bản của một câu lệnh trên dòng lệnh:

```text
command [options] [arguments]
```

- **`command`**: Tên lệnh cần thực hiện.
- **`options`**: Tùy chọn điều chỉnh cách lệnh hoạt động.
- **`arguments`**: Giá trị hoặc đối tượng mà lệnh xử lý, chẳng hạn tên tệp hay đường dẫn.

## Chuyển hướng dữ liệu và nối lệnh

| Ký hiệu | Tác dụng | Ví dụ |
| --- | --- | --- |
| `>` | Ghi đầu ra của lệnh vào tệp; **ghi đè** nội dung cũ. | `tree data > cay.txt` |
| `>>` | Ghi thêm đầu ra vào **cuối tệp**. | `tree data >> cay.txt` |
| `<` | Lấy nội dung tệp làm đầu vào cho lệnh. | `cat < cay.txt` |
| <code>&#124;</code> (pipe) | Nối đầu ra của lệnh bên trái với đầu vào của lệnh bên phải. | <code>ls -l &#124; wc -l</code> |

`>` và `>>` đều tạo tệp nếu chưa có. `echo < cay.txt` không đọc nội dung tệp vì `echo` không sử dụng đầu vào thông thường. `ls -l | wc -l` đếm **số dòng đầu ra**, không nhất thiết là số tệp: kết quả có thể gồm dòng `total` và mặc định không hiện tệp ẩn.

## Lệnh `man`

`man` (viết tắt của *manual*) mở trang hướng dẫn cho lệnh và một số thành phần hệ thống, như lời gọi hệ thống hoặc định dạng tệp cấu hình.

```text
man <tên_lệnh>
```

Thay `<tên_lệnh>` bằng tên cần tra cứu, ví dụ `man ls` để xem hướng dẫn của `ls`.

**Điều hướng trong trang hướng dẫn:**

- Nhấn `↑` hoặc `↓` để di chuyển từng dòng.
- Nhấn `Page Up` hoặc `Page Down` để di chuyển từng trang.
- Nhấn `q` để thoát và trở về dòng lệnh.
