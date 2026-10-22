# Vì sao Software Engineer cần học Linux?

Khi mới học lập trình, chúng ta thường tập trung vào ngôn ngữ và framework: Java, Python, C#, Spring Boot cho backend; React, Next.js cho frontend; SQL cho cơ sở dữ liệu. Viết xong và chạy được trên máy cá nhân là một bước quan trọng, nhưng ứng dụng còn phải hoạt động ổn định khi đưa cho người khác sử dụng.

Trong công việc, bạn có thể cần:

- Đưa ứng dụng lên server và cho người dùng truy cập qua Internet.
- Đọc log để tìm nguyên nhân lỗi khi ứng dụng đang chạy.
- Kiểm tra tiến trình, bộ nhớ, CPU, ổ đĩa và quyền truy cập tệp.
- Kiểm tra kết nối mạng, địa chỉ IP và cổng (port) của dịch vụ.
- Cấu hình dịch vụ như Nginx, kết nối bằng SSH, chạy Docker.
- Trao đổi với người phụ trách DevOps hoặc vận hành hệ thống.

Nhiều server và môi trường cloud chạy Linux. Vì vậy, hiểu Linux giúp bạn biết **mã của mình chạy ở đâu, phụ thuộc vào những gì và xử lý sự cố như thế nào**. Mục tiêu không phải ai cũng trở thành quản trị viên hệ thống; một Software Engineer nên có đủ kiến thức để triển khai, quan sát và phối hợp xử lý các vấn đề của ứng dụng.

> **Nhớ:** Muốn đi xa trong nghề phần mềm, các em không thể chỉ biết viết code. Các em phải hiểu code của mình đang chạy trên môi trường nào.

**Chuyển tư duy:** Từ “người chỉ biết lập trình” sang “người hiểu sản phẩm đang vận hành như thế nào”. Khi có lỗi, hãy nghĩ đến cả mã nguồn, tiến trình, cấu hình, hệ điều hành, mạng và những dịch vụ mà ứng dụng phụ thuộc.

## 1. Hệ điều hành là gì?

**Hệ điều hành (Operating System, OS)** là lớp phần mềm trung gian giữa phần cứng và ứng dụng. Máy tính có phần cứng, nhưng ứng dụng cần hệ điều hành đứng ra quản lý CPU, bộ nhớ, ổ đĩa, thiết bị và mạng để có thể sử dụng các tài nguyên đó.

Ví dụ, khi chương trình Java đọc một tệp log, nó không điều khiển ổ đĩa trực tiếp. Chương trình yêu cầu hệ điều hành mở và đọc tệp. Hệ điều hành kiểm tra quyền truy cập rồi làm việc với hệ thống tệp và thiết bị lưu trữ.

```text
Ứng dụng (Java, Python, Node.js, ...)
             ↓
Hệ điều hành
             ↓
Phần cứng (CPU, RAM, ổ đĩa, card mạng, ...)
```

Một số khái niệm bạn sẽ gặp thường xuyên:

| Khái niệm | Hiểu đơn giản |
| --- | --- |
| Tiến trình (process) | Một chương trình đang chạy, chẳng hạn tiến trình của ứng dụng backend. |
| Hệ thống tệp (file system) | Cách hệ điều hành tổ chức và truy cập tệp, thư mục. |
| Quyền truy cập (permissions) | Quy định ai được đọc, sửa hoặc chạy một tệp. |
| Dịch vụ (service) | Chương trình chạy để phục vụ một chức năng, ví dụ web server hoặc database. |
| Log | Bản ghi sự kiện giúp theo dõi hoạt động và tìm lỗi. |

## 2. Linux, kernel và bản phân phối

**Kernel** là nhân của hệ điều hành. Nó quản lý các tài nguyên cốt lõi như tiến trình, bộ nhớ, thiết bị và giao tiếp mạng. Nói chính xác, **Linux là một kernel**. Trong giao tiếp hằng ngày, mọi người cũng thường dùng từ “Linux” để chỉ cả hệ điều hành được xây dựng quanh kernel đó.

**Bản phân phối Linux (distribution, hay distro)** là một hệ điều hành hoàn chỉnh kết hợp Linux kernel với các công cụ, thư viện, trình quản lý gói và cấu hình cần thiết. Ubuntu, Debian và Fedora là các ví dụ về bản phân phối.

Hình dung: kernel là phần lõi giúp phần mềm làm việc với phần cứng; bản phân phối đóng gói phần lõi cùng những thành phần cần thiết để bạn cài đặt và sử dụng một hệ điều hành.

### Mã nguồn mở nghĩa là gì?

Phần mềm **mã nguồn mở** có mã nguồn được công khai để cộng đồng có thể xem, học, chỉnh sửa và phát triển theo điều kiện của giấy phép đi kèm. Linux là một biểu tượng lớn của thế giới mã nguồn mở. “Mã nguồn mở” không có nghĩa là không có quy tắc sử dụng: mỗi dự án vẫn có giấy phép riêng.

## 3. Máy ảo và VMware

**Máy ảo (Virtual Machine, VM)** là một máy tính được tạo bằng phần mềm trên máy thật. Máy ảo được cấp CPU, RAM, ổ đĩa và card mạng ảo; nó có thể cài hệ điều hành riêng. Máy thật được gọi là **host**, còn hệ điều hành trong máy ảo là **guest**.

**VMware** là tên hãng và nhóm sản phẩm ảo hóa; ví dụ VMware Workstation cho phép tạo và chạy máy ảo trên máy cá nhân. Bạn có thể dùng máy ảo để cài Ubuntu, thực hành lệnh Linux, thử cấu hình server và khôi phục môi trường khi cần.

Máy ảo khác container: máy ảo chạy hệ điều hành guest riêng, còn container chia sẻ kernel của máy host. Vì vậy, học máy ảo giúp hiểu một server như một máy tính độc lập; học Docker sau đó sẽ dễ hình dung hơn.
