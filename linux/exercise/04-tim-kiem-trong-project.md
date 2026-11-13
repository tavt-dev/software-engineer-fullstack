# Bài tập 04: Tìm kiếm trong project

> GitHub issue: [#15 — Tìm kiếm trong project](https://github.com/tavt-dev/software-engineer-fullstack/issues/15)

## Câu hỏi

- Từ `dev-practice`, tìm tất cả tệp `.txt`, `.log` và tệp tên `package.json`.
- Tìm từ khóa `port` trong toàn bộ project.

## Câu trả lời

Chạy từ thư mục gốc của project:

```bash
cd ~/dev-practice
pwd
find . -name "*.txt"
find . -name "*.log"
find . -name "package.json"
grep -rni "port" .
```

| Tìm kiếm | Kết quả trong ảnh |
| --- | --- |
| Tệp `.txt` | `backend/config.txt`, `frontend/note.txt` |
| Tệp `.log` | `backend/app.log`, `backend/error.log` |
| Tệp `package.json` | `frontend/package.json` |
| Nội dung chứa `port` | Dòng 2 của `backend/app.log`: `INFO Server running on port 8080` |

Trong lệnh `find`, dấu `.` là thư mục hiện tại. Với `grep -rni`, `-r` tìm cả thư mục con, `-n` hiện số dòng và `-i` bỏ qua khác biệt chữ hoa/thường.

![Bài 4: tìm tệp bằng find và tìm nội dung bằng grep](img/bai-04.png)
