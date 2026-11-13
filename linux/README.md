# Linux căn bản cho Software Engineer

Ứng dụng thường chạy trên Linux. Bộ ghi chú này giúp bạn hiểu môi trường chạy ứng dụng và dùng các lệnh cơ bản để làm việc với tệp, thư mục và hệ thống.

## Học theo thứ tự

| Bài | Nội dung | Đọc khi cần |
| --- | --- | --- |
| [1. Tổng quan Linux](01-tong-quan.md) | OS, kernel, distro, kiến trúc, máy ảo | Hiểu Linux gồm những gì. |
| [2. Hệ thống tệp](02-he-thong-tep.md) | Cây thư mục, kiểu tệp, đường dẫn | Tìm và hiểu vị trí tệp. |
| [3. Terminal](03-terminal.md) | Dấu nhắc, cú pháp, phím tắt, `history`, `clear`, `man` | Hiểu dòng lệnh và xem lại thao tác đã làm. |
| [4. Điều hướng](04-dieu-huong.md) | `pwd`, `cd`, `ls`, `tree`, `du`, `df` | Kiểm tra đúng project, xem thư mục và dung lượng đĩa. |
| [5. Tệp và thư mục](05-tep-va-thu-muc.md) | Thao tác tệp, `nano`, `find`, `grep`, `tail -f` | Sửa cấu hình, đọc log và debug backend. |
| [6. Quyền và gói](06-quyen-va-goi.md) | `sudo`, `chmod`, `chown`, APT, `passwd` | Sửa lỗi quyền truy cập và cài phần mềm. |
| [7. Kiểm tra hệ thống](07-kiem-tra-he-thong.md) | Tài khoản, CPU/RAM, `ps`, `kill`, `systemctl`, log dịch vụ | Tìm backend đang chạy và quản lý dịch vụ. |
| [8. IP và kết nối mạng](08-mang-va-ket-noi.md) | `hostname -I`, `ip a`, `ping`, `ss`, `curl`, SSH/SCP | Kiểm tra kết nối và làm việc với server. |
| [9. Shell script](09-shell-script.md) | Shebang, cách chạy `.sh`, script kiểm tra server và tạo project | Tự động hóa các thao tác lặp lại. |

**Cách dùng:** Người mới nên đọc theo thứ tự. Khi cần tra cứu, chọn bài trong bảng. Kiểm tra đường dẫn trước khi chạy lệnh thay đổi hoặc xóa dữ liệu.

## Tài liệu PDF

- [Linux cheatsheet (PDF)](tai-lieu/linux-cheatsheet.pdf)

## Sơ đồ tổng hợp lệnh Linux

[![Sơ đồ lệnh Linux cho dev: Terminal, tệp, quyền, gói phần mềm, process, mạng và service](../img/linux-commands-mindmap.png)](../img/linux-commands-mindmap.png)

Khi dừng tiến trình, kiểm tra đúng PID và dùng `kill PID` trước; chỉ dùng `kill -9 PID` khi tiến trình không dừng bằng cách thông thường.
