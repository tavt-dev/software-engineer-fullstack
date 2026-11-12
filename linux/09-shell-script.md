# 9. Tự động hóa với shell script

[← Mục lục](README.md) · [← IP và kết nối mạng](08-mang-va-ket-noi.md)

**Shell script** là tệp chứa các lệnh để shell thực hiện lần lượt, thường có đuôi `.sh`. Thay vì gõ lại nhiều lệnh, bạn gom chúng vào một tệp rồi chạy khi cần.

## Kiểm tra server bằng `trangiang.sh`

Tạo và mở tệp:

```bash
nano trangiang.sh
```

Nhập nội dung sau, lưu bằng `Ctrl + O`, `Enter`, rồi thoát bằng `Ctrl + X`:

```bash
#!/bin/bash

echo "=== Hostname ==="
hostname

echo "=== User ==="
whoami

echo "=== RAM ==="
free -h

echo "=== Disk ==="
df -h

echo "=== Ports ==="
ss -tuln

echo "=== IP ==="
hostname -I

echo "=== Kernel ==="
uname -a
```

**`#!/bin/bash`** là shebang, đặt ở dòng đầu. Khi chạy tệp trực tiếp, dòng này yêu cầu hệ thống dùng Bash để thực hiện script. `echo` in tiêu đề để bạn dễ đọc từng phần kết quả.

### Chạy script

Cách đơn giản là gọi Bash để đọc và chạy tệp:

```bash
bash trangiang.sh
```

Hoặc cấp quyền thực thi rồi chạy trực tiếp:

```bash
chmod u+x trangiang.sh
./trangiang.sh
```

`./` chỉ tệp nằm trong thư mục hiện tại. Cách chạy bằng `bash trangiang.sh` không yêu cầu tệp có quyền thực thi.

Trong công việc, script này giúp xem nhanh thông tin máy trước khi triển khai hoặc kiểm tra sự cố. Các lệnh chạy trên **máy bạn đang thực thi script**; muốn kiểm tra server thì đăng nhập SSH vào server và chạy tại đó.

## Tạo cấu trúc project tự động

Tạo tệp `tao-project.sh` bằng `nano`, rồi nhập:

```bash
#!/bin/bash
set -e

mkdir -p fullstack-demo/backend/src
mkdir -p fullstack-demo/backend/logs
mkdir -p fullstack-demo/backend/config
mkdir -p fullstack-demo/frontend/public
mkdir -p fullstack-demo/frontend/src
mkdir -p fullstack-demo/backup
mkdir -p fullstack-demo/script

touch fullstack-demo/backend/logs/app.log
touch fullstack-demo/backend/config/application.properties
touch fullstack-demo/frontend/src/index.html
touch fullstack-demo/script/README.md

echo "Da tao cau truc trong thu muc fullstack-demo"
```

Trong script này, `set -e` dừng việc tạo project nếu một lệnh `mkdir` hoặc `touch` thất bại. `mkdir -p` tạo thư mục nhiều cấp; `touch` tạo tệp rỗng nếu chưa có và giữ nội dung của tệp đã tồn tại.

Chạy script và kiểm tra kết quả:

```bash
bash tao-project.sh
tree fullstack-demo
```

Project được tạo trong thư mục `fullstack-demo` thuộc vị trí bạn đang đứng:

```text
fullstack-demo/
├── backend/
│   ├── src/
│   ├── logs/
│   │   └── app.log
│   └── config/
│       └── application.properties
├── frontend/
│   ├── public/
│   └── src/
│       └── index.html
├── backup/
└── script/
    └── README.md
```

> Dev cần hiểu code chạy ở đâu, kiểm tra lỗi ở đâu và biết tự động hóa những việc lặp lại.
