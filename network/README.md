# Mạng máy tính cơ bản cho Software Engineer

Ghi chép nền tảng để hiểu một request từ trình duyệt hoặc ứng dụng đi tới server như thế nào. Nội dung được sắp xếp từ **địa chỉ → dịch vụ → giao thức → luồng dữ liệu**, kèm hai sơ đồ tham khảo.

## Sơ đồ tổng quan

<img src="../img/1791854717932_6099000326064343182_g2011408275649891011_55777a554e604ae5515d9f5b52a23e43.jpg" alt="Sơ đồ nền tảng mạng máy tính: IP, port, OSI, TCP/UDP và luồng truy cập web" width="650">

## 1. Địa chỉ IP

**IP (Internet Protocol) address** là địa chỉ được gán cho một giao diện mạng để các thiết bị định vị và trao đổi dữ liệu với nhau. Có thể hình dung IP như địa chỉ nhận hàng: dữ liệu cần biết sẽ đi đến đâu. Một thiết bị có thể có nhiều địa chỉ IP, chẳng hạn một địa chỉ cho Wi-Fi và một địa chỉ cho VPN.

### Public IP và private IP

| Loại | Dùng ở đâu | Ví dụ |
| --- | --- | --- |
| **Public IP** | Địa chỉ có thể được định tuyến trên Internet công cộng. | Địa chỉ public được gán cho router, VPS hoặc dịch vụ cloud. |
| **Private IP** | Địa chỉ dùng trong mạng nội bộ; không được định tuyến trực tiếp trên Internet công cộng. | `192.168.1.10`, `10.0.0.5`, `172.16.0.8`. |

Các dải IPv4 private là `10.0.0.0/8`, `172.16.0.0/12` và `192.168.0.0/16`. Mạng gia đình, công ty, Docker và cloud thường dùng địa chỉ nội bộ. Router hoặc cổng mạng có thể dùng **NAT** để nhiều máy private cùng truy cập Internet. Có public IP cũng không có nghĩa là ai cũng kết nối được: firewall, cấu hình dịch vụ và quy tắc mạng vẫn có thể chặn truy cập.

Trên Windows, chạy `ipconfig` để xem IP của các giao diện mạng; `ipconfig /all` cho thêm thông tin như DNS server và địa chỉ MAC. Địa chỉ thấy ở máy thường là private IP, khác với public IP của kết nối ra Internet.

### IPv4 và IPv6

| Phiên bản | Cách viết | Kích thước |
| --- | --- | --- |
| **IPv4** | Bốn nhóm số từ `0` đến `255`, ví dụ `192.168.1.10` hoặc `8.8.8.8`. | 32 bit, khoảng 4,29 tỷ giá trị địa chỉ. |
| **IPv6** | Các nhóm số hệ 16 ngăn bằng dấu `:`, có thể rút gọn, ví dụ `2001:db8::1`. | 128 bit, không gian địa chỉ lớn hơn rất nhiều. |

IPv6 được phát triển một phần để giải quyết giới hạn số lượng địa chỉ IPv4. Hai phiên bản cùng được sử dụng hiện nay.

## 2. Domain, DNS, port và URL

**Domain** là tên dễ nhớ như `example.com`. **DNS** giúp tìm thông tin cần thiết để kết nối tới tên đó; khi truy cập web, trình duyệt thường tra bản ghi A hoặc AAAA để lấy địa chỉ IPv4 hoặc IPv6. Một tên miền có thể trả về nhiều IP, và kết quả có thể đã được lưu trong bộ nhớ đệm.

**Port** là số dùng để chọn điểm nhận kết nối của một dịch vụ trên máy. Nếu IP giống địa chỉ tòa nhà, port giống số phòng. Một máy có thể chạy web server, SSH và database trên các port khác nhau. Port gắn với giao thức vận chuyển như TCP hoặc UDP; cùng một số port có thể xuất hiện ở cả hai giao thức.

| Port thường gặp | Dịch vụ thường dùng | Ghi nhớ |
| --- | --- | --- |
| `80` | HTTP | Web không mã hóa ở tầng HTTP. |
| `443` | HTTPS | HTTPS thường dùng port này; HTTP/3 dùng QUIC trên UDP. |
| `22` | SSH | Truy cập shell từ xa. |
| `53` | DNS | Có thể dùng UDP hoặc TCP. |
| `3306` | MySQL | Port mặc định phổ biến của MySQL. |
| `6379` | Redis | Port mặc định phổ biến của Redis. |
| `3000` | Dev server | Thường gặp khi phát triển web, ví dụ Next.js. |
| `8080` | Dev server hoặc API | Thường gặp ở ứng dụng backend. |
| `9090` | Công cụ giám sát | Ví dụ Prometheus; tùy cấu hình. |

Đây là **quy ước thường gặp**, không phải cam kết rằng dịch vụ luôn chạy ở port đó. Cần xem cấu hình thực tế của ứng dụng.

**URL** là địa chỉ đầy đủ của một tài nguyên. Ví dụ `https://hotrodoan.vn:443/login` gồm:

| Thành phần | Giá trị | Ý nghĩa |
| --- | --- | --- |
| Scheme | `https` | Chỉ giao thức truy cập. |
| Host/domain | `hotrodoan.vn` | Tên máy chủ cần truy cập. |
| Port | `443` | Port HTTPS; có thể bỏ khỏi URL vì đây là port mặc định. |
| Path | `/login` | Đường dẫn tài nguyên trên server. |

URL còn có thể có query string sau `?` và fragment sau `#`. **IP là địa chỉ mạng, domain là tên, DNS hỗ trợ tra cứu tên, port chọn dịch vụ, còn URL mô tả nơi truy cập cụ thể.**

<img src="../img/1791898896513_6099000326064343182_g2011408275649891011_fc74a8d1640011b56f852e78369de38e.jpg" alt="Sơ đồ các thành phần của URL: scheme, subdomain, domain, TLD, path và slug" width="560">

*Ảnh minh họa URL: [Ahrefs – The anatomy of a URL](https://ahrefs.com/blog/seo-friendly-urls/).*

## 3. Mô hình OSI và TCP/IP

**OSI** là mô hình tham chiếu 7 tầng để phân chia trách nhiệm khi dữ liệu được gửi từ ứng dụng qua mạng. Khi nhận dữ liệu, phía bên kia xử lý theo chiều ngược lại. Càng lên cao càng gần ứng dụng; càng xuống thấp càng gần đường truyền và phần cứng.

| Tầng | Tên | Vai trò và ví dụ |
| --- | --- | --- |
| 7 | Application | Giao thức ứng dụng như HTTP và DNS; nơi trình duyệt, Postman và API tương tác. |
| 6 | Presentation | Cách dữ liệu được biểu diễn, mã hóa hoặc nén trong mô hình tham chiếu. |
| 5 | Session | Thiết lập và quản lý phiên giao tiếp theo mô hình OSI. |
| 4 | Transport | Truyền dữ liệu giữa các đầu cuối: TCP, UDP và port. |
| 3 | Network | Địa chỉ IP và định tuyến gói tin qua các mạng; router hoạt động chủ yếu ở đây. |
| 2 | Data Link | Truyền trong cùng liên kết mạng: frame, địa chỉ MAC, switch. |
| 1 | Physical | Tín hiệu và môi trường truyền: cáp, sóng vô tuyến, thiết bị vật lý. |

OSI giúp **hình dung** chức năng từng tầng; giao thức thực tế không phải lúc nào cũng khớp cứng với một tầng. Ví dụ, JSON là định dạng dữ liệu của ứng dụng, còn TLS nằm giữa ứng dụng và tầng vận chuyển trong cách triển khai web thường gặp; không nên coi chúng luôn là “tầng 6”.

Trong thực tế, mô hình **TCP/IP** thường được trình bày bằng 4 tầng: Application (gần OSI 5–7), Transport (OSI 4), Internet (OSI 3) và Network Access (gần OSI 1–2).

## 4. TCP và UDP

| | TCP | UDP |
| --- | --- | --- |
| Cách hoạt động | Thiết lập kết nối; truyền luồng byte có thứ tự và cơ chế gửi lại khi cần. | Gửi từng datagram; bản thân UDP không bảo đảm nhận được hoặc đúng thứ tự. |
| Thường gặp | SSH, MySQL, nhiều kết nối HTTP/1.1 và HTTP/2. | DNS, game, gọi video và một số luồng thời gian thực. |
| Điều cần nhớ | Tính tin cậy có thêm chi phí điều khiển kết nối. | Ít cơ chế hơn ở tầng vận chuyển; ứng dụng có thể tự bổ sung tính tin cậy. |

UDP không có nghĩa là mọi ứng dụng dùng nó đều mất dữ liệu hoặc luôn nhanh hơn TCP. Ví dụ **HTTP/3 dùng QUIC trên UDP** nhưng QUIC tự cung cấp cơ chế kết nối và truyền tin cậy.

## 5. Một lần truy cập web diễn ra thế nào?

Giả sử mở `https://example.com/login`:

1. **Đọc URL:** trình duyệt xác định `https`, host `example.com`, path `/login` và port mặc định `443`.
2. **Tìm IP:** trình duyệt dùng kết quả DNS đã lưu hoặc hỏi DNS để có địa chỉ IP phù hợp.
3. **Kết nối:** dữ liệu được định tuyến tới IP và port. Với HTTP/1.1 hoặc HTTP/2, thường có **bắt tay TCP ba bước**; HTTP/3 dùng QUIC trên UDP.
4. **Bảo mật:** HTTPS thực hiện **bắt tay TLS** để xác thực server và thiết lập mã hóa. Bắt tay TLS khác với bắt tay TCP ba bước.
5. **Gửi request:** trình duyệt gửi HTTP request, chẳng hạn `GET /login`, kèm các header và cookie nếu có.
6. **Xử lý ở server:** reverse proxy hoặc web server chuyển request tới ứng dụng; ứng dụng có thể kiểm tra quyền, gọi database và tạo response.
7. **Trả response:** server gửi mã trạng thái, header và nội dung như HTML hoặc JSON. Trình duyệt có thể tiếp tục tải CSS, JavaScript và ảnh bằng các request khác.

Luồng này là bản đơn giản hóa: cache, CDN, proxy và cấu hình thực tế có thể làm các bước thay đổi.

## Thử trên máy Windows

```powershell
ipconfig
ipconfig /all
nslookup example.com
Test-NetConnection example.com -Port 443
curl.exe -I https://example.com/
```

`ipconfig` cho biết địa chỉ mạng của máy; `nslookup` kiểm tra phân giải DNS; `Test-NetConnection` thử kết nối TCP tới port 443; `curl.exe -I` chỉ lấy HTTP response header. Nếu một phép thử thất bại, cần xem lại DNS, firewall, mạng và dịch vụ trước khi kết luận server đã ngừng hoạt động.
