# 5. Tạo, xem và quản lý tệp

[← Mục lục](README.md) · [← Điều hướng](04-dieu-huong.md) · [Tiếp: Quyền và gói →](06-quyen-va-goi.md)

## Tạo thư mục với `mkdir`

`mkdir` (*make directory*) tạo thư mục. Thêm `-p` để tạo cả các thư mục cha còn thiếu.

| Ví dụ | Tác dụng |
| --- | --- |
| `mkdir demo1 demo2` | Tạo hai thư mục `demo1` và `demo2`. |
| `mkdir -p demo/child` | Tạo `child` bên trong `demo`, đồng thời tạo `demo` nếu chưa có. |

## Xóa thư mục rỗng với `rmdir`

`rmdir` chỉ xóa được **thư mục rỗng**.

| Ví dụ | Tác dụng |
| --- | --- |
| `rmdir demo1 demo2` | Xóa hai thư mục rỗng vừa tạo. |
| `rmdir -p demo/child` | Xóa `child`, rồi xóa tiếp `demo` nếu `demo` rỗng. |

## Tạo tệp với `echo` và `touch`

- `echo` in văn bản; dùng `>` để ghi vào tệp và `>>` để thêm vào cuối tệp.
- `touch` tạo tệp rỗng nếu chưa có. Nếu tệp đã tồn tại, lệnh cập nhật thời gian truy cập và sửa đổi, giữ nguyên nội dung.

```bash
mkdir -p data
echo "Hello Linux" > data/notes.txt
echo "Learning Linux" >> data/notes.txt
touch data/empty.txt
```

Các lệnh trên tạo thư mục `data`, ghi hai dòng vào `notes.txt` và tạo tệp rỗng `empty.txt`. Dấu `>` **ghi đè** nội dung nếu tệp đã có.

## Sửa nội dung tệp với `nano`

`nano` là trình soạn thảo chạy trong Terminal, dùng để sửa tệp văn bản hoặc cấu hình:

```bash
nano data/notes.txt
```

Sửa nội dung, nhấn `Ctrl + O` rồi `Enter` để lưu vào tên tệp hiện tại; nhấn `Ctrl + X` để thoát.

## Xem nội dung tệp

| Lệnh | Tác dụng |
| --- | --- |
| `cat data/notes.txt` | In toàn bộ nội dung tệp ra Terminal. |
| `more data/notes.txt` | Xem từng màn hình, chủ yếu di chuyển về phía trước. |
| `less data/notes.txt` | Xem từng màn hình, có thể cuộn lên và xuống. |
| `head data/notes.txt` | In 10 dòng đầu theo mặc định. |
| `tail data/notes.txt` | In 10 dòng cuối theo mặc định. |

Để chọn số dòng, dùng `-n`: `head -n 1 data/notes.txt` xem dòng đầu, `tail -n 1 data/notes.txt` xem dòng cuối.

Trong `more` hoặc `less`: **Space** xuống một màn hình, **Enter** xuống một dòng, **q** để thoát. Trong `less`, dùng thêm `↑` / `↓` để cuộn.

### Đọc log và theo dõi backend

Giả sử backend ghi log vào tệp `app.log`:

| Lệnh | Tác dụng |
| --- | --- |
| `tail -n 100 app.log` | Xem 100 dòng log cuối để tìm lỗi gần nhất. |
| `tail -f app.log` | Hiện các dòng cuối, rồi tiếp tục hiển thị dữ liệu mới được ghi thêm. |
| `tail -n 100 -f app.log` | Xem 100 dòng cuối và tiếp tục theo dõi log mới. |

Dùng `tail -f` khi gọi thử API để xem backend vừa ghi gì. Nhấn `Ctrl + C` để dừng theo dõi; thao tác này không dừng backend.

## Tìm tệp với `find`

`find` tìm tệp hoặc thư mục theo tên và điều kiện. Lệnh tìm từ vị trí chỉ định, bao gồm các thư mục con.

| Ví dụ | Tác dụng |
| --- | --- |
| `find . -type f -name "*.js"` | Tìm các tệp có đuôi `.js` từ thư mục hiện tại. |
| `find . -type f -name "config.txt"` | Tìm tệp cấu hình có tên `config.txt`. |
| `find . -type d -name "src"` | Tìm các thư mục có tên `src`. |

`-type f` chọn tệp thường, `-type d` chọn thư mục. Giữ mẫu `"*.js"` trong dấu nháy để shell chuyển nguyên mẫu cho `find`.

## Tìm văn bản với `grep`

`grep` tìm và in các dòng khớp với một mẫu văn bản.

| Ví dụ | Tác dụng |
| --- | --- |
| `grep "Linux" data/notes.txt` | Tìm các dòng chứa `Linux`. |
| `grep -i "linux" data/notes.txt` | Tìm không phân biệt chữ hoa và chữ thường. |
| `grep -n "Linux" data/notes.txt` | Hiện cả số dòng trong kết quả. |
| `grep -c "Linux" data/notes.txt` | Đếm số dòng khớp, mỗi dòng chỉ tính một lần. |
| `grep -v "Hello" data/notes.txt` | Hiện các dòng không chứa `Hello`. |
| `grep -r "Linux" data/` | Tìm trong các tệp của `data` và các thư mục con. |

Khi debug, dùng `grep -n "port" config.txt` để tìm dòng cấu hình cổng, hoặc `grep "ERROR" app.log` để lọc các dòng log báo lỗi. **`find` tìm tệp, `grep` tìm nội dung trong tệp.**

## Sao chép với `cp`

`cp` sao chép tệp. Dùng `-R` hoặc `-r` trên Linux để sao chép cả thư mục và nội dung bên trong.

| Ví dụ | Tác dụng |
| --- | --- |
| `cp data/notes.txt data/notes-copy.txt` | Tạo bản sao của `notes.txt`. |
| `cp -R data data_copy` | Sao chép `data` thành `data_copy` nếu đích chưa có. |

Tệp đích có thể bị **ghi đè**; thêm `-i` để hỏi trước khi ghi đè.

## Di chuyển hoặc đổi tên với `mv`

`mv` di chuyển hoặc đổi tên tệp và thư mục.

| Ví dụ | Tác dụng |
| --- | --- |
| `mv data/empty.txt data/draft.txt` | Đổi tên `empty.txt` thành `draft.txt`. |
| `mv data/draft.txt .` | Chuyển `draft.txt` ra thư mục hiện tại. |

| Tùy chọn | Tác dụng |
| --- | --- |
| `-i` | Hỏi trước khi ghi đè tệp đích. |
| `-f` | Không hỏi trước khi ghi đè tệp đích. |

`mv` có thể **ghi đè** tệp đích ngay cả khi không thêm `-f`.

## Xóa tệp và thư mục với `rm`

`rm` xóa tệp. Thêm `-r` để xóa cả thư mục và nội dung bên trong.

| Tùy chọn | Tác dụng |
| --- | --- |
| `-i` | Hỏi trước khi xóa từng mục. |
| `-f` | Không hỏi và bỏ qua mục không tồn tại. |
| `-r` | Xóa thư mục cùng toàn bộ nội dung bên trong. |

```bash
rm -i data/notes-copy.txt
rm -r data_copy
```

Lệnh đầu hỏi trước khi xóa bản sao của tệp; lệnh sau xóa thư mục bản sao. **`rm` không chuyển dữ liệu vào Thùng rác.**
