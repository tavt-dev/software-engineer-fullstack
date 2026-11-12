# 4. Điều hướng và xem thư mục

[← Mục lục](README.md) · [← Terminal](03-terminal.md) · [Tiếp: Thao tác tệp →](05-tep-va-thu-muc.md)

## Xem thư mục hiện tại với `pwd`

`pwd` (*print working directory*) cho biết bạn đang ở thư mục nào bằng đường dẫn tuyệt đối.

```bash
pwd
```

Ví dụ kết quả:

```text
/home/user01/project
```

Trong công việc, dùng `pwd` để kiểm tra đúng project trước khi chạy lệnh tạo, sao chép hoặc xóa dữ liệu.

## Chuyển thư mục với `cd`

`cd` (*change directory*) chuyển sang thư mục khác.

| Ví dụ | Ý nghĩa |
| --- | --- |
| `cd /home/user01/project` | Chuyển đến `project` bằng đường dẫn tuyệt đối. |
| `cd src` | Chuyển vào thư mục con `src` của thư mục hiện tại. |
| `cd ..` | Chuyển lên thư mục cha. |
| `cd -` | Quay lại thư mục vừa rời khỏi. |
| `cd` hoặc `cd ~` | Về thư mục cá nhân của bạn. |

## Xem danh sách tệp với `ls`

`ls` liệt kê tệp và thư mục con, mặc định không hiện các mục ẩn. Thêm đường dẫn để xem thư mục khác, ví dụ `ls /home`.

| Ví dụ | Tác dụng |
| --- | --- |
| `ls -l` | Xem chi tiết quyền, chủ sở hữu, kích thước và thời gian sửa đổi. |
| `ls -a` | Hiện cả các mục ẩn. |
| `ls -la` | Xem chi tiết cả các mục ẩn, thường dùng để kiểm tra tệp như `.env`, `.gitignore`. |
| `ls -lh` | Xem chi tiết, kích thước dễ đọc như `1K`, `20M`, `2G`. |
| `ls -t` | Sắp xếp theo thời gian sửa đổi, mới nhất trước. |
| `ls -S` | Sắp xếp theo kích thước, lớn nhất trước. |

Có thể gộp các tùy chọn: `ls -lah` vừa xem chi tiết, vừa hiện mục ẩn và kích thước dễ đọc.

## Xem cây thư mục với `tree`

`tree` hiển thị cấu trúc tệp và thư mục con dưới dạng cây:

```bash
tree
```

Ví dụ kết quả:

```text
.
├── README.md
└── src
    └── app.js

1 directory, 2 files
```

Dấu `.` là thư mục hiện tại. Dùng `tree project` để xem cây của thư mục `project`.

Nếu chưa có `tree`, trên Ubuntu/Debian có thể cài bằng `sudo apt install tree`.

## Xem dung lượng với `du`

`du` (*disk usage*) ước tính dung lượng đĩa mà tệp hoặc thư mục sử dụng. Để xem tổng dung lượng của thư mục hiện tại:

```bash
du -sh .
```

| Cách dùng | Kết quả |
| --- | --- |
| `du -h .` | Dung lượng thư mục hiện tại và các thư mục con, dạng `K`, `M`, `G`. |
| `du -sh .` | Chỉ lấy tổng dung lượng của thư mục hiện tại, dạng dễ đọc. |

`-s` chỉ lấy tổng, `-h` hiển thị kích thước dễ đọc. Thay `.` bằng đường dẫn khác để xem thư mục đó, ví dụ `du -sh project`.

## Xem dung lượng còn trống với `df`

```bash
df -h
```

Xem dung lượng đã dùng và còn trống của các hệ thống tệp đang gắn. Đọc cột `Avail` (còn trống) và `Use%` (phần trăm đã dùng) khi nghi ổ đĩa đầy.

**`du` xem thư mục chiếm bao nhiêu; `df` xem hệ thống tệp còn bao nhiêu chỗ trống.**
