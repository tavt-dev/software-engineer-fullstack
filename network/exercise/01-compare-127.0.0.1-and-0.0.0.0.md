# Bài tập 01: Phân biệt `127.0.0.1` và `0.0.0.0`

> GitHub issue: [#7 — Compare 127.0.0.1 and 0.0.0.0](https://github.com/tavt-dev/software-engineer-fullstack/issues/7)

## Câu hỏi

- Hai địa chỉ có ý nghĩa gì?
- Server lắng nghe trên từng địa chỉ thì thiết bị nào truy cập được?
- Vì sao thường thấy cả hai địa chỉ khi chạy server?
- Firewall ảnh hưởng thế nào đến việc truy cập từ máy khác?

## Câu trả lời

### `127.0.0.1`: địa chỉ loopback IPv4

`127.0.0.1` trỏ về chính máy đang sử dụng. Nếu server chỉ lắng nghe tại `127.0.0.1:8080`, ứng dụng trên cùng máy có thể kết nối đến server, còn thiết bị khác không thể kết nối qua IP LAN của máy chạy server.

`localhost` là tên thường dùng cho loopback. Tùy cấu hình, tên này có thể trỏ đến `127.0.0.1` (IPv4) hoặc `::1` (IPv6). Trên mỗi thiết bị, `127.0.0.1` đều trỏ về **chính thiết bị đó**.

### `0.0.0.0`: lắng nghe trên mọi địa chỉ IPv4

Khi cấu hình server lắng nghe tại `0.0.0.0:8080`, server nhận kết nối qua tất cả địa chỉ IPv4 khả dụng của máy, bao gồm địa chỉ loopback và IP LAN. `0.0.0.0` dùng để **cấu hình nơi server lắng nghe**; client thường truy cập bằng một địa chỉ cụ thể như `127.0.0.1` hoặc IP LAN, không dùng `0.0.0.0` làm địa chỉ đích.

### Ví dụ

Giả sử máy chạy server có IP LAN là `192.168.1.10` và ứng dụng chạy ở port `8080`:

| Server lắng nghe tại | Trên chính máy chạy server | Trên máy khác cùng mạng |
| --- | --- | --- |
| `127.0.0.1:8080` | Truy cập `http://127.0.0.1:8080` | Không truy cập được qua `http://192.168.1.10:8080` |
| `0.0.0.0:8080` | Truy cập `http://127.0.0.1:8080` | Có thể truy cập `http://192.168.1.10:8080` nếu mạng và firewall cho phép |

### Vì sao thường thấy cả hai?

- Dùng `127.0.0.1` khi chỉ cần truy cập ứng dụng trên máy đang chạy server.
- Dùng IP LAN hoặc `0.0.0.0` khi cần cho thiết bị khác trong mạng truy cập. Với `0.0.0.0`, server cũng có thể lắng nghe trên các giao diện mạng khác của máy, nên cần chú ý phạm vi truy cập.

### Firewall ảnh hưởng thế nào?

Dù server lắng nghe tại `0.0.0.0`, thiết bị khác vẫn có thể không kết nối được nếu firewall chặn port `8080` hoặc mạng không cho phép hai thiết bị liên lạc. Ngược lại, mở firewall không khiến server chỉ lắng nghe tại `127.0.0.1` nhận kết nối qua IP LAN.