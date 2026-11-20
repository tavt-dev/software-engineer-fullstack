# 5. Dockerfile: tự đóng gói ứng dụng thành image

[← Mục lục](README.md) · [← Env và network](04-env-va-network.md)

## Từ dùng image có sẵn đến tự tạo image

Ở các bài trước, mình dùng image có sẵn như Nginx và MySQL. Nhưng backend, frontend hoặc service nội bộ do team viết thì cần được đóng gói thành image riêng.

Ví dụ web app do bạn Giang viết đang nằm trong một repo. Làm sao để chạy ứng dụng đó bằng Docker, để người khác build lại và để local, staging, production dùng cùng cách đóng gói?

**Dockerfile là file văn bản mô tả công thức tạo image cho ứng dụng.** File này cho Docker biết bắt đầu từ image nền nào, làm việc ở đâu, copy những tệp nào, cài thêm gì và khởi động ứng dụng bằng lệnh nào.

## Source code, Dockerfile, image và container

| Thành phần | Vai trò | Liên tưởng |
| --- | --- | --- |
| Source code (SC) | Mã nguồn ứng dụng, ví dụ `app.js`. | Nguyên liệu. |
| Dockerfile | Các chỉ dẫn để tạo image. | Công thức nấu ăn. |
| Image | Bản đóng gói chứa ứng dụng, runtime và các tệp cần thiết. | Món ăn đã chế biến. |
| Container | Một instance của image, có thể được khởi động hoặc dừng. | Một phần ăn được mang ra sử dụng. |

**Dockerfile không phải image hay container.** Có `app.js` mới chỉ là có source code; cần viết Dockerfile và build để tạo image, sau đó chạy container từ image đó.

```text
Source code + Dockerfile
          ↓ docker build
        Image
          ↓ docker run
      Container chạy ứng dụng
```

## Các chỉ dẫn Dockerfile đầu tiên

| Chỉ dẫn | Ví dụ | Ý nghĩa |
| --- | --- | --- |
| `FROM` | `FROM node:24` | Chọn image nền có Node.js để chạy ứng dụng. Viết đúng là `FROM`, không phải `FORM`. |
| `WORKDIR` | `WORKDIR /app` | Đặt thư mục làm việc bên trong image là `/app`, tạo thư mục nếu chưa có. |
| `COPY` | `COPY . .` | Copy tệp từ build context vào thư mục làm việc trong image. |
| `RUN` | `RUN npm install` | Chạy lệnh trong quá trình **build image**, ví dụ cài dependency. |
| `EXPOSE` | `EXPOSE 3000` | Ghi nhận ứng dụng dự kiến lắng nghe cổng `3000`. |
| `CMD` | `CMD ["node", "app.js"]` | Đặt lệnh mặc định khi **khởi động container** từ image. |

App Java cần image nền phù hợp với Java; Node.js dùng Node; web tĩnh có thể dùng Nginx. Ví dụ buổi học dùng `node:20`; bài này dùng `node:24` vì Node 20 đã hết hỗ trợ theo [lịch phát hành Node.js](https://nodejs.org/en/about/previous-releases).

### Hiểu hai dấu chấm trong `COPY . .`

Với lệnh build `docker build -t 17112026 .` chạy tại thư mục project:

- Dấu `.` cuối lệnh build chọn thư mục hiện tại làm **build context**, tức tập tệp Docker được dùng để build.
- Dấu `.` thứ nhất trong `COPY . .` chỉ nguồn ở gốc build context.
- Dấu `.` thứ hai chỉ thư mục đích, ở đây là `/app` do đã đặt `WORKDIR /app`.

Các tệp được loại bằng `.dockerignore` sẽ không được copy theo cách này. Nguồn của `COPY` được tính từ build context, không tự động từ vị trí Dockerfile khi dùng một context khác. Xem [Docker build context](https://docs.docker.com/build/concepts/context/).

### Phân biệt `RUN`, `CMD`, `EXPOSE` và `-p`

`RUN npm install` cài thư viện vào image trong lúc build. `CMD ["node", "app.js"]` khởi động web app khi chạy container; build image không có nghĩa web app đã chạy. Lệnh truyền sau tên image trong `docker run` có thể thay thế `CMD` mặc định.

`EXPOSE 3000` không tự mở cổng trên host, cũng không khiến ứng dụng tự lắng nghe cổng đó. Code phải lắng nghe cổng `3000`, còn `docker run -p 3000:3000` mới publish cổng host để truy cập. Xem [Dockerfile reference](https://docs.docker.com/reference/dockerfile/).

## Thực hành: đóng gói web app Node.js

Ví dụ dùng Bash trên Linux, Docker Engine đang chạy, cổng host `3000` còn trống và tên container chưa được dùng. Cần mạng để tải image nền. Không cần cài Node.js trên host vì ứng dụng chạy bằng Node trong container.

Tạo thư mục lab riêng:

```bash
mkdir -p ~/dockerfile-lab
cd ~/dockerfile-lab
```

Tạo ba tệp sau bằng trình soạn thảo bạn đang dùng:

```text
dockerfile-lab/
├── app.js
├── Dockerfile
└── .dockerignore
```

### 1. Viết source code `app.js`

```javascript
const http = require('node:http');

const port = Number(process.env.PORT || 3000);

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
  res.end('Xin chào từ web app của mình chạy bằng Docker!\n');
});

server.listen(port, '0.0.0.0', () => {
  console.log(`Web app đang lắng nghe cổng ${port}`);
});
```

App dùng module `http` có sẵn của Node.js nên không cần `package.json` hay `npm install`. Lắng nghe tại `0.0.0.0` giúp nhận kết nối qua giao diện mạng của container; chỉ lắng nghe `127.0.0.1` trong container sẽ không nhận kết nối được chuyển tiếp từ host.

### 2. Viết `Dockerfile`

Tên file là **`Dockerfile`**, không có phần mở rộng:

```dockerfile
FROM node:24
WORKDIR /app
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

Không thêm `RUN npm install` vào ví dụ này vì ứng dụng không có dependency bên ngoài. Với project dùng npm, cần có `package.json`; khi có `package-lock.json` đồng bộ, có thể dùng `RUN npm ci` để cài dependency theo lockfile.

### 3. Viết `.dockerignore`

```text
node_modules
.git
.env
.env.*
*.log
```

Đặt file này ở gốc build context để loại thư viện đã cài trên host, lịch sử Git, tệp env và log khỏi context. Kiểm tra các tệp khác trước khi dùng `COPY . .` để tránh đưa dữ liệu không cần thiết vào image.

### 4. Build image

Đứng trong `~/dockerfile-lab`, chạy:

```bash
docker build -t 17112026 .
docker images 17112026
```

| Thành phần | Ý nghĩa |
| --- | --- |
| `docker build` | Tạo image theo Dockerfile, mặc định tìm file tên `Dockerfile` ở gốc context. |
| `-t 17112026` | Đặt tên image là `17112026`; không ghi tag thì mặc định là `latest`. |
| `.` | Chọn thư mục hiện tại làm build context. |

`17112026` là tên image của ví dụ, có thể thay bằng tên dễ nhớ như `giang-web:v1`. Build thành công mới tạo được image để chạy ở bước sau; lệnh này không tự đẩy image lên Docker Hub.

### 5. Chạy container và kiểm tra

```bash
docker run -d -p 3000:3000 --name tienkiengiangcontainer 17112026:latest
docker ps
docker logs tienkiengiangcontainer
curl http://localhost:3000
```

`-d` chạy nền; `-p 3000:3000` ánh xạ cổng `3000` trên host tới cổng `3000` của app; `--name` đặt tên container. Theo mặc định, cách publish này dùng mọi giao diện mạng trên host; nếu chỉ truy cập từ host, dùng `-p 127.0.0.1:3000:3000`.

**Kết quả mong đợi:** `curl` trả về `Xin chào từ web app của mình chạy bằng Docker!`. Đây là hướng dẫn thực hành, chưa phải kết quả đã chạy trên máy của bạn.

Nếu dùng Docker trên VPS hoặc máy ảo, `localhost` trong lệnh trên là máy chạy Docker. Từ máy khác, dùng IP/domain và cổng host phù hợp với cấu hình mạng, firewall.

Nếu chưa truy cập được, xem `docker ps -a` và `docker logs tienkiengiangcontainer`. Kiểm tra app có lắng nghe `0.0.0.0:3000`, ánh xạ cổng có đúng và tên container có bị trùng không. Khi cổng host `3000` bận, có thể dùng `-p 3001:3000` rồi truy cập cổng `3001`.

## Khi sửa code và chuyển môi trường

Code được `COPY` vào image tại thời điểm build. Sửa `app.js` trên host không tự đổi code trong image hoặc container đã tạo; cần build lại và tạo container mới để dùng bản mới. Việc build lại cùng tên image cũng không cập nhật container cũ.

Dockerfile giúp cả team chia sẻ cách đóng gói. Tuy nhiên, cùng Dockerfile chưa đảm bảo hai lần build cho ra image giống hệt nếu image nền hoặc dependency thay đổi. Khi cần kiểm soát chặt hơn, cố định image nền bằng digest và quản lý dependency bằng lockfile.

Khi triển khai, ưu tiên **build một image đã kiểm thử rồi dùng chính image đó** cho local, staging và production trên nền tảng tương thích. Truyền cấu hình môi trường khi chạy, như đã học ở [bài env và network](04-env-va-network.md). Ví dụ app ở trên đọc biến `PORT`; nếu đổi cổng app thì cần đổi ánh xạ cổng tương ứng.

**Ghi nhớ:** Source code là nguyên liệu, Dockerfile là công thức, image là bản đóng gói, container là instance được tạo từ image để chạy ứng dụng.
