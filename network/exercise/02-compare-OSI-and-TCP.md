# Bài tập 02: So sánh mô hình OSI và TCP/IP

## Câu hỏi

- OSI có những tầng nào? TCP/IP có những tầng nào?
- Các tầng của hai mô hình tương ứng với nhau ra sao?
- Hai mô hình được dùng để làm gì?

## Câu trả lời

**OSI** là mô hình tham chiếu gồm 7 tầng, giúp chia nhỏ chức năng của hệ thống mạng để học tập, thiết kế và phân tích sự cố. **TCP/IP** thường được trình bày bằng 4 tầng, mô tả bộ giao thức đang được dùng để truyền dữ liệu trên Internet. Việc ghép tầng giữa hai mô hình chỉ mang tính tương đối: giao thức thực tế không phải lúc nào cũng nằm gọn trong một tầng OSI.

### Bảy tầng của OSI

| Tầng | Tên | Chức năng chính | Ví dụ |
| --- | --- | --- | --- |
| 7 | **Application — Ứng dụng** | Cung cấp giao thức mạng cho ứng dụng. | HTTP, DNS, SMTP |
| 6 | **Presentation — Trình bày** | Biểu diễn, chuyển đổi định dạng, mã hóa và nén dữ liệu theo mô hình tham chiếu. | Mã hóa, định dạng dữ liệu |
| 5 | **Session — Phiên** | Thiết lập, duy trì và kết thúc phiên giao tiếp. | Quản lý phiên trao đổi giữa hai bên |
| 4 | **Transport — Giao vận** | Truyền dữ liệu giữa các tiến trình; dùng port, hỗ trợ chia dữ liệu và các cơ chế truyền tùy giao thức. | TCP, UDP |
| 3 | **Network — Mạng** | Địa chỉ IP và định tuyến gói tin qua các mạng. | IPv4, IPv6, ICMP |
| 2 | **Data Link — Liên kết dữ liệu** | Đóng gói thành frame và truyền trên một liên kết; dùng địa chỉ MAC, có thể phát hiện lỗi frame. | Ethernet, phần liên kết dữ liệu của Wi-Fi |
| 1 | **Physical — Vật lý** | Truyền bit dưới dạng tín hiệu điện, quang hoặc vô tuyến. | Cáp mạng, sóng vô tuyến, đầu nối |

Đơn vị dữ liệu thường được gọi là **data** ở các tầng trên, **segment** với TCP hoặc **datagram** với UDP ở tầng giao vận, **packet** ở tầng mạng, **frame** ở tầng liên kết dữ liệu và **bit** ở tầng vật lý.

### Bốn tầng của TCP/IP

| Tầng | Tên | Chức năng chính | Ví dụ |
| --- | --- | --- | --- |
| 4 | **Application — Ứng dụng** | Cung cấp giao thức cho ứng dụng và bao gồm các chức năng tương ứng với các tầng OSI phía trên. | HTTP, DNS, SMTP, FTP |
| 3 | **Transport — Giao vận** | Truyền dữ liệu giữa các tiến trình, sử dụng port. | TCP, UDP |
| 2 | **Internet — Liên mạng** | Địa chỉ IP và chuyển gói tin qua các mạng. | IP, ICMP |
| 1 | **Network Access — Truy cập mạng** | Truyền frame và tín hiệu trên mạng có dây hoặc không dây. | Ethernet, Wi-Fi |

### Ánh xạ hai mô hình

| OSI | TCP/IP |
| --- | --- |
| 7. Application + 6. Presentation + 5. Session | Application |
| 4. Transport | Transport |
| 3. Network | Internet |
| 2. Data Link + 1. Physical | Network Access |

