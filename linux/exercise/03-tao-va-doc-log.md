# Bài tập 03: Tạo và đọc log

> GitHub issue: [#14 — Tạo và đọc log](https://github.com/tavt-dev/software-engineer-fullstack/issues/14)

Mở `app.log` bằng nano:

```bash
cd ~/dev-practice/backend
nano app.log
```

Nhập log mẫu:

```text
INFO Application started
INFO Server running on port 8080
ERROR Database connection failed
INFO Retry connection
ERROR Timeout while connecting to database
```

Đọc và lọc log:

```bash
cat app.log
head -n 2 app.log
tail -n 2 app.log
grep "ERROR" app.log
```

**Kết quả:** xem được toàn bộ log, 2 dòng đầu, 2 dòng cuối và lọc được 2 dòng lỗi database.

![Bài 3: xem toàn bộ log, hai dòng đầu, hai dòng cuối và các dòng ERROR](img/bai-03.png)
