# 5. Tạo, xem và quản lý tệp

[← Mục lục](README.md) · [← Điều hướng](04-dieu-huong.md) · [Tiếp: Quyền và gói →](06-quyen-va-goi.md)

## Tạo thư mục với `mkdir`

`mkdir` (*make directory*) tạo một hoặc nhiều thư mục. Cú pháp: `mkdir [tùy_chọn] <tên_thư_mục>...`. Tùy chọn `-p` tạo các thư mục cha còn thiếu.

- `mkdir my_dir1 my_dir2`: Tạo hai thư mục trong thư mục hiện tại.
- `mkdir -p dir3/dir4`: Tạo `dir4` trong `dir3`, đồng thời tạo `dir3` nếu chưa có.

## Xóa thư mục rỗng với `rmdir`

`rmdir` chỉ xóa được thư mục rỗng. Cú pháp: `rmdir [tùy_chọn] <tên_thư_mục>...`. Tùy chọn `-p` xóa thư mục được chỉ định rồi tiếp tục xóa các thư mục cha nếu chúng cũng rỗng.

- `rmdir my_dir1 my_dir2`: Xóa hai thư mục rỗng.
- `rmdir -p dir3/dir4`: Xóa `dir4`, sau đó xóa `dir3` nếu rỗng.

## Tạo tệp với `echo` và `touch`

`echo` in văn bản ra màn hình. Kết hợp với `>` để ghi vào tệp (tạo mới hoặc ghi đè nội dung cũ), hoặc `>>` để thêm vào cuối tệp. `touch` tạo tệp rỗng nếu tệp chưa tồn tại; nếu đã tồn tại, lệnh chỉ cập nhật thời gian truy cập và sửa đổi.

```bash
mkdir -p data
echo "Linux TITV" > data/vidu.txt
echo "Them mot dong" >> data/vidu.txt
touch data/a.txt
```

Dòng đầu tạo thư mục `data` nếu chưa có. Dấu `>` ở dòng thứ hai là phần cần thiết để nội dung được ghi vào `vidu.txt`.

## Xem nội dung tệp

| Lệnh | Tác dụng |
| --- | --- |
| `cat <tệp>` | In toàn bộ nội dung tệp ra Terminal; phù hợp với tệp ngắn. |
| `more <tệp>` | Xem nội dung theo từng màn hình, chủ yếu di chuyển về phía trước. |
| `less <tệp>` | Xem nội dung theo từng màn hình và di chuyển lên hoặc xuống. |
| `head <tệp>` | In 10 dòng đầu của tệp theo mặc định. |
| `tail <tệp>` | In 10 dòng cuối của tệp theo mặc định. |

Ví dụ với tệp đã tạo ở trên:

```bash
cat data/vidu.txt
less data/vidu.txt
head -n 5 data/vidu.txt
tail -n 5 data/vidu.txt
```

Với `more` hoặc `less`, nhấn **Space** để xuống một màn hình, **Enter** để xuống một dòng, **q** để thoát. Trong `less`, có thể dùng phím mũi tên để di chuyển lên hoặc xuống. Tùy chọn `-n` của `head` và `tail` chỉ định số dòng muốn xem.

## Tìm văn bản với `grep`

`grep` tìm các dòng khớp với một mẫu văn bản và in chúng ra Terminal. Cú pháp: `grep [tùy_chọn] <mẫu> [tệp]`. Mẫu có thể dùng biểu thức chính quy; nếu không ghi tệp, `grep` đọc từ đầu vào thông thường.

| Tùy chọn | Tác dụng |
| --- | --- |
| `-c` | Đếm số **dòng khớp**; một dòng có nhiều lần xuất hiện vẫn chỉ tính một. |
| `-i` | Không phân biệt chữ hoa và chữ thường. |
| `-v` | Hiển thị các dòng **không khớp**. |
| `-n` | Hiển thị số dòng trước mỗi dòng kết quả. |
| `-r` | Tìm đệ quy trong thư mục. |

```bash
grep "Linux" data/vidu.txt
grep -r "Linux" data/
grep 'root' /etc/passwd
```

Dòng đầu tìm trong một tệp; `-r` tìm dưới thư mục `data/`; dòng cuối tìm các dòng chứa `root` trong `/etc/passwd`.

## Sao chép với `cp`

`cp` sao chép tệp từ nguồn sang đích. Cú pháp: `cp [tùy_chọn] <nguồn> <đích>`. Để sao chép cả thư mục và nội dung bên trong, dùng `-R` (hoặc `-r` trên GNU/Linux).

- `cp data/vidu.txt data/ban_sao.txt`: Tạo bản sao của tệp `vidu.txt`.
- `cp -R data data_copy`: Sao chép thư mục `data` thành `data_copy` nếu thư mục đích chưa tồn tại.

Nếu tệp đích đã tồn tại, `cp` có thể ghi đè nội dung của tệp đó.

## Di chuyển hoặc đổi tên với `mv`

`mv` di chuyển hoặc đổi tên tệp và thư mục. Cú pháp: `mv [tùy_chọn] <nguồn> <đích>`.

| Tùy chọn | Tác dụng |
| --- | --- |
| `-i` | Hỏi trước khi thay thế tệp đích đã tồn tại. |
| `-f` | Không hỏi trước khi thay thế tệp đích. |

- `mv data/a.txt data/ghi_chu.txt`: Đổi tên tệp trong cùng thư mục.
- `mv data/ghi_chu.txt .`: Di chuyển tệp về thư mục hiện tại.

`mv` có thể ghi đè tệp đích ngay cả khi không dùng `-f`; dùng `-i` nếu muốn xác nhận trước.

## Xóa tệp và thư mục với `rm`

`rm` xóa tệp theo cú pháp `rm [tùy_chọn] <tệp>...`. Muốn xóa thư mục cùng nội dung bên trong, cần thêm `-r` (hoặc `-R`); thư mục rỗng có thể xóa bằng `rmdir` ở trên.

| Tùy chọn | Tác dụng |
| --- | --- |
| `-i` | Hỏi xác nhận trước khi xóa từng mục. |
| `-f` | Không hỏi xác nhận và bỏ qua mục không tồn tại. |
| `-r` | Xóa đệ quy thư mục và toàn bộ nội dung bên trong. |

Ví dụ: `rm -i data/ban_sao.txt` hỏi trước khi xóa tệp; `rm -r data_copy` xóa thư mục `data_copy` và các mục trong đó. `rm` không chuyển các mục đã xóa vào Thùng rác.
