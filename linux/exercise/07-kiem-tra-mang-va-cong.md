# Bài tập 07: Kiểm tra mạng và cổng

> GitHub issue: [#18 — Kiểm tra mạng và cổng](https://github.com/tavt-dev/software-engineer-fullstack/issues/18)

## Câu hỏi

- Chạy `ping google.com`, `curl https://example.com` và `ss -tuln`.
- Máy có kết nối Internet không? Dựa vào kết quả nào?
- curl trả về nội dung gì? Máy đang có những cổng nào?

## Câu trả lời

```bash
ping google.com
# Nhấn Ctrl + C để dừng và xem thống kê.

curl https://example.com
ss -tuln
```

### Máy có kết nối Internet không?

Có kết nối tại thời điểm kiểm tra:

- `ping google.com` nhận được phản hồi từ IP `74.125.130.102`.
- Thống kê ping: gửi `35` gói, nhận `32` gói, mất khoảng **8,57%**; thời gian khứ hồi trung bình khoảng `36,864 ms`.
- `curl https://example.com` nhận được nội dung trang web từ Internet.

Kết quả cho thấy máy truy cập Internet được, nhưng lần ping này có mất gói nên chưa thể kết luận kết nối ổn định.

### curl trả về nội dung gì?

`curl` in mã HTML của trang **Example Domain** ra Terminal, gồm các thẻ như `<!doctype html>`, `<html>` và `<title>Example Domain</title>`. Lệnh này giúp kiểm tra nội dung web trả về ngay trong Terminal.

### Máy đang có những cổng nào?

`ss -tuln` liệt kê socket TCP đang lắng nghe và socket UDP đang chờ nhận dữ liệu. Các tùy chọn: `-t` là TCP, `-u` là UDP, `-l` lọc socket đang chờ nhận kết nối/dữ liệu, `-n` hiển thị địa chỉ và số cổng dạng số.

| Giao thức | Cổng | Địa chỉ cục bộ trong ảnh |
| --- | --- | --- |
| TCP | `53` | `127.0.0.53`, `127.0.0.54` |
| TCP | `631` | `127.0.0.1`, `[::1]` |
| UDP | `53` | `127.0.0.53`, `127.0.0.54` |
| UDP | `323` | `127.0.0.1`, `[::1]` |
| UDP | `5353` | `0.0.0.0`, `[::]` |

TCP hiển thị trạng thái `LISTEN`; UDP hiển thị `UNCONN`. Các địa chỉ `127.x.x.x` và `::1` chỉ dùng trong máy. `0.0.0.0` và `[::]` là địa chỉ gắn trên mọi giao diện tương ứng IPv4 và IPv6; khả năng truy cập từ máy khác còn phụ thuộc mạng và firewall.

![Bài 7: kết quả ping, HTML trả về từ curl và các socket do ss liệt kê](img/bai-07.png)
