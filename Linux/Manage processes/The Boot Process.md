# The Boot Process
Quá trình khởi động hệ thống (Boot process) là quá trình mà hệ thống sử dụng để khởi động và chuẩn bị cho việc sử dụng. Quá trình khởi đông này có nhiều bước khác nhau và phụ thụô vào hệ thống sử dụng , tuy nhiên phần lớn các hệ thống linux đều có các bước tương tự 
1. BIOS

Đầu tiên máy tính sẽ được bật lên và kiểm tra phần cứng, BIOS (Basic Input/Output System) sẽ được thực thi để kiểm tra và tải các thiết bị phần cứng cơ bản như bộ nhớ RAM , ổ đĩa cứng, bàn phím , chuột và màn hình. BIOS cũng sẽ kiểm tra các thiết bị khác nhau để xác định chúng có sẵn và hoạt động tốt hay không.

2. MBR

Sau đó hệ thống sẽ đọc MBR (Master Boot Record) trên ổ đĩa cứng, nơi chứa thông tin cơ bản về phân vùng của ổ đĩa và nơi để tìm kiếm boot loader.

3. Boot loader

Boot loader được tìm thấy trong phân vùng đầu tiên của ổ đĩa cứng và được tải lên bộ nhớ RAM để thực thi. Trong Linux, GRUN ( Grand Unified Bootloader) là boot loader phổ biến nhất. GRUB sẽ cung cấp cho người dùng các tùy chọn khởi động hệ thống bao gồm cả việc khởi động từ một hệ điều hanh khác nếu có.

4. Kernel

Sau khi boot loader hoạt động, kernel sẽ được tải lên và khởi chạy. Kernel là phần cốt lõi của hệ thống Linux, nó quản lý tài nguyên phần cứng, quản lý bộ nhớ và cung cấp các dịch vụ cho các ứng dụng.

5. Init

Sau khi kernel được tải lên, init sẽ được khởi chạy . Init là quá trình đầu tiên được khởi động bởi kernel và có trách nhiệm khởi động tất cả các dịch vụ cần thiết trên hệ thống bao gồm các dịch vụ mạng dịch vụ hệ thống và các ứng dụng.

6. Runlevel

Runlevel là một chế độ hoạt động cụ thể của hệ thống. Mỗi runlevel sẽ có các dịch vụ và tiến trình cụ thể được khởi động trong quá trình khởi động. Có một số runlevel chuân được sử dụng:

- Runlevel 0: Tắt hệ thống
- Runlevel 1: Chế độ đơn người dùng , chỉ khởi động một số dịch vụ cần thiết để khôi phục hệ thống
- Runlevel 2: Chế độ đa người dùng, không có kết nối mạng
- Runlevel 3: Chế độ đa ngươi dùng , có kết nối mạng
- Runlevel 4: Không được sử dụng .
- Runlevel 5: Chế độ đa người dùng với giao diện đồ họa GUI
- Runlevel 6: Khởi động lại hệ thống

7. Login

Sau khi quá trình khởi động hoàn tât người dùng được yêu cầu đăng nhập vào hệ thống. Người dùng sẽ được cung cấp một tên người dùng và mật khẩu để truy cập vào hệ thống. Sau khi đăng nhập thnahf công thì người dùng có thể sử dụng các ứng dụng và dịch vụ trên hệ thống.