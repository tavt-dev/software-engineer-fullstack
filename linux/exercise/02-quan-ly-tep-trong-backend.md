# Bài tập 02: Quản lý tệp trong backend

> GitHub issue: [#13 — Quản lý tệp trong backend](https://github.com/tavt-dev/software-engineer-fullstack/issues/13)

## Câu hỏi

- Trong `backend`, đổi tên `README.md` thành `guide.md`.
- Sao chép `config.txt` thành `config.backup.txt`, rồi xóa bản sao.
- Tạo thêm tệp `error.log`.

## Câu trả lời

```bash
cd ~/dev-practice/backend
mv README.md guide.md
cp config.txt config.backup.txt
ls
rm config.backup.txt
touch error.log
ls
```

- `mv`: đổi tên `README.md` thành `guide.md`.
- `cp`: tạo bản sao `config.backup.txt` từ `config.txt`.
- `rm`: xóa bản sao vừa tạo.
- `touch`: tạo tệp rỗng `error.log`.

**Kết quả cuối:** thư mục `backend` có `app.log`, `config.txt`, `error.log` và `guide.md`; không còn `config.backup.txt`.

![Bài 2: đổi tên, sao chép, xóa và tạo tệp trong backend](img/bai-02.png)
