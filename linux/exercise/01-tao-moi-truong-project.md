# Bài tập 01: Tạo môi trường project

> GitHub issue: [#12 — Tạo môi trường project](https://github.com/tavt-dev/software-engineer-fullstack/issues/12)

## Câu hỏi

- Tạo project `dev-practice` trong thư mục home bằng Terminal, gồm `backend`, `frontend` và `scripts`.
- Tạo đầy đủ các tệp theo cấu trúc đề bài và kiểm tra bằng `ls -la`.

## Câu trả lời

Tạo thư mục `dev-practice` trong home, sau đó tạo các thư mục và tệp bằng Terminal:

```bash
cd ~
pwd
mkdir dev-practice
cd dev-practice
mkdir backend frontend scripts

cd backend
touch app.log config.txt README.md
cd ../frontend
touch package.json note.txt
cd ../scripts
touch deploy.sh
cd ..

tree
```

**Kết quả:** project nằm tại `/home/ta-van-tien/dev-practice`, có cấu trúc:

```text
dev-practice/
├── backend/
│   ├── app.log
│   ├── config.txt
│   └── README.md
├── frontend/
│   ├── note.txt
│   └── package.json
└── scripts/
    └── deploy.sh
```

`tree` hiển thị cây thư mục, giúp kiểm tra nhanh tệp nào nằm trong thư mục nào. Các tệp được tạo bằng `touch` đang rỗng, bao gồm `package.json` và `deploy.sh`.

![Bài 1: tạo cấu trúc project bằng Terminal và kiểm tra bằng tree](img/bai-01.png)

**Cần bổ sung minh chứng:** đề yêu cầu ảnh kiểm tra bằng `ls -la`; ảnh hiện tại dùng `ls` và `tree`. Có thể chạy thêm:

```bash
cd ~/dev-practice
ls -la
ls -la backend frontend scripts
```

Nếu chụp sau bài 2, thư mục `backend` sẽ có `guide.md` thay cho `README.md` và có thêm `error.log`.
