# 4. Điều hướng và xem thư mục

[← Mục lục](README.md) · [← Terminal](03-terminal.md) · [Tiếp: Thao tác tệp →](05-tep-va-thu-muc.md)

## In thư mục hiện tại với `pwd`

`pwd` (*print working directory*) hiển thị đường dẫn tuyệt đối của thư mục làm việc hiện tại.

```bash
pwd
```

## Chuyển thư mục với `cd`

`cd` (*change directory*) thay đổi thư mục làm việc hiện tại.

| Ví dụ | Ý nghĩa |
| --- | --- |
| `cd /path/to/directory` | Chuyển đến thư mục theo đường dẫn tuyệt đối. |
| `cd ~` | Chuyển về thư mục cá nhân của người dùng hiện tại. |
| `cd ..` | Chuyển lên thư mục cha. |
| `cd -` | Quay lại thư mục làm việc trước đó. |
| `cd $HOME` | Chuyển đến thư mục được chỉ bởi biến môi trường `HOME`. |
| `cd ~john` | Chuyển đến thư mục cá nhân của người dùng `john`, nếu tài khoản tồn tại và có quyền truy cập. |

## Xem danh sách tệp với `ls`

`ls` liệt kê các mục trong thư mục hiện tại. Không có tùy chọn, lệnh hiển thị tên các mục không bị ẩn.

| Tùy chọn | Tác dụng |
| --- | --- |
| `-l` | Hiển thị chi tiết quyền truy cập, chủ sở hữu, kích thước và thời gian sửa đổi. |
| `-a` | Hiển thị cả các mục có tên bắt đầu bằng dấu chấm. |
| `-h` | Hiển thị kích thước dễ đọc theo bội số 1024 khi có cột kích thước, ví dụ `ls -lh`. |
| `-t` | Sắp xếp theo thời gian sửa đổi, mới nhất trước. |
| `-r` | Đảo ngược thứ tự sắp xếp hiện tại. |
| `-R` | Liệt kê đệ quy nội dung các thư mục con. |
| `--color=auto` | Tô màu khi hiển thị trên Terminal. |
| `-S` | Sắp xếp theo kích thước tệp, lớn nhất trước. |
| `-G` | Ẩn tên **nhóm** trong chế độ chi tiết, ví dụ `ls -lG`. |
| `-i` | Hiển thị số inode của từng mục. |
| `--group-directories-first` | Hiển thị thư mục trước các loại tệp khác. |
| `-L` | Khi hiển thị thông tin về liên kết tượng trưng, dùng thông tin của mục mà liên kết trỏ tới. |

## Xem cây thư mục với `tree`

`tree` hiển thị tệp và thư mục con theo dạng cây. Không ghi đường dẫn thì lệnh hiển thị từ thư mục hiện tại:

```bash
tree
```

Ví dụ kết quả:

```text
project/
├── src/
│   └── app.js
└── README.md
```

Trên Ubuntu/Debian, nếu chưa có `tree`, cài bằng `sudo apt install tree`.

## Xem dung lượng với `du`

`du` (*disk usage*) ước tính dung lượng đĩa mà tệp và thư mục sử dụng. Không có đối số, lệnh thống kê thư mục hiện tại và các thư mục con.

```bash
du
```

| Cách dùng | Kết quả |
| --- | --- |
| `du -h .` | Hiển thị dung lượng từng thư mục ở dạng dễ đọc (`K`, `M`, `G`). |
| `du -sh .` | Chỉ hiển thị tổng dung lượng của thư mục hiện tại ở dạng dễ đọc. |
| `du -sk .` | Hiển thị tổng dung lượng theo KiB. |
| `du -sm .` | Hiển thị tổng dung lượng theo MiB. |
| `du -s -BG .` | Hiển thị tổng dung lượng theo GiB. |
| `du -s --si .` | Hiển thị tổng dung lượng theo đơn vị thập phân (KB, MB, GB). |

Dấu `.` là thư mục hiện tại; có thể thay bằng đường dẫn khác. Các đơn vị `K`, `M`, `G` mặc định của `du -h` dùng bội số 1024 (KiB, MiB, GiB); `--si` dùng bội số 1000. Khi ép về một đơn vị cố định, kết quả được làm tròn lên.
