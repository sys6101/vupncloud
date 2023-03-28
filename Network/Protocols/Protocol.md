# Các giao thức trong mạng Internet

|Giao thức |  Port  |     Info   |
|------    |------- |-------------|
|TCP/IP (Transmission Control Protocol/Internet Protocol)|Không có port cụ thể       |Đây là giao thức chính được sử dụng trong Internet để truyền dữ liệu giữa các thiết bị mạng. TCP đảm bảo việc gửi và nhận dữ liệu giữa hai máy tính, trong khi IP quản lý việc định tuyến dữ liệu giữa các mạng khác nhau.              |
|HTTP (Hypertext Transfer Protocol)    |80    |Giao thức này được sử dụng để truyền tải các trang web giữa máy tính của người dùng và các máy chủ web.     |
|FTP (File Transfer Protocol)|20 (data), 21(control)|Giao thức này được sử dụng để truyền tải các tệp tin giữa các máy tính trong mạng.|
|SMTP (Simple Mail Transfer Protocol)|25|Giao thức này được sử dụng để gửi và nhận email giữa các máy chủ email.|
|DNS (Domain Name System)|53|Giao thức này được sử dụng để ánh xạ địa chỉ IP của các máy tính thành tên miền dễ nhớ.|
|DHCP (Dynamic Host Configuration Protocol)|67(server), 68 (device request IP)|Giao thức này được sử dụng để cấu hình địa chỉ IP và các thông số mạng khác cho các thiết bị mạng tự động.|
|ICMP (Internet Control Message Protocol)|Không có port cụ thể vì ko phải là giao thức truyền tải|Giao thức này được sử dụng để kiểm tra kết nối mạng, phát hiện lỗi và gửi các thông báo trạng thái giữa các máy tính trong mạng.|
|SSL/TLS (Secure Sockets Layer/Transport Layer Security)|443|Giao thức này được sử dụng để bảo vệ dữ liệu được truyền tải trên mạng Internet bằng cách mã hóa thông tin để ngăn chặn các hacker có thể truy cập và đánh cắp thông tin.|
|SNMP (Simple Network Management Protocol)|Port 161 (dành cho gửi yêu cầu từ quản trị viên mạng) và port 162 (dành cho nhận thông báo từ các thiết bị mạng).|Giao thức này được sử dụng để quản lý các thiết bị mạng từ xa. Nó cho phép các quản trị viên mạng có thể giám sát và điều khiển các thiết bị mạng như máy chủ, switch, router từ một máy tính từ xa.|
|SSH (Secure Shell)|22|Giao thức này được sử dụng để đăng nhập vào các thiết bị mạng từ xa một cách an toàn. Nó cung cấp cơ chế xác thực để bảo vệ tài khoản đăng nhập.|
|ARP (Address Resolution Protocol)|Không có port cụ thể, vì ARP không phải là giao thức truyền tải dữ liệu.|Giao thức này được sử dụng để ánh xạ địa chỉ IP sang địa chỉ MAC của các thiết bị mạng trong cùng một mạng LAN. ARP giúp cho các thiết bị mạng có thể giao tiếp với nhau bằng địa chỉ MAC thay vì địa chỉ IP.|