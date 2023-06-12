# Keepalived
Keepalived là một phần mềm mã nguồn mở được sử dụng để triển khai cơ chế High Availability (HA) cho các dịch vụ mạng, đặc biệt là dịch vụ máy chủ. Nó cung cấp khả năng chuyển giao nguồn và IP ảo giữa các máy chủ hoạt động trong chế độ Active-Active hoặc Active-Standby. Cơ chế này giúp đảm bảo rằng dịch vụ vẫn có sẵn khi một máy chủ hoặc một phiên bản của dịch vụ bị lỗi.

Keepalived hoạt động dựa trên giao thức VRRP (Virtual Router Redundancy Protocol) để quản lý và chuyển giao địa chỉ IP ảo. Giao thức VRRP cho phép các máy chủ trong cụm định nghĩa một IP ảo chung và chọn ra một máy chủ chính để đảm nhận vai trò Active. Các máy chủ khác trong nhóm sẽ hoạt động trong vai trò Standby, sẵn sàng tiếp nhận nếu máy chủ chính gặp sự cố.






## Thiết lập VRRP (Virtual Router Redundancy Protocol)

Chuẩn bị hai máy chủ Linux (A và B).
Cài đặt và cấu hình Keepalived trên cả hai máy chủ.
Thiết lập VRRP trên cụm máy chủ A và B.
Đảm bảo rằng máy chủ A được cấu hình là nút chính (master) và máy chủ B là nút dự phòng (backup).
Kiểm tra tính HA (High Availability) bằng cách tắt dịch vụ mạng trên máy chủ đang hoạt động và đảm bảo rằng nút dự phòng tự động nhận IP và chuyển giao dịch mạng.  
#### Thay đổi IPv4 của interface eth2 cho 2 máy
Dùng: `sudo vim /etc/netplan/01-netcfg.yaml`

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/keep1.png)     
Dùng `sudo netplan apply `để lưu lại đã thay đổi        
Dùng `ip a` để xem cài đặt đã thay đổi      

#### Enable IPv4_forward:

Để nhận các gói tin từ NIC này sang NIC khác của máy

Uncomment dòng `net.ipv4.ip_forward=1` trong file /etc/sysctl.conf.

Mở file /etc/sysctl.conf dùng `sudo vim /etc/sysctl.conf`. Nhấn `i` để vào chế độ insert và sửa. Sửa xong thì nhấn phím `Esc` và gõ `:wq`

Áp dụng thay đổi tức thì mà không cần reboot dùng: `sudo sysctl -p /etc/sysctl.conf`
 ### Cài đặt và cấu hình keepalived trên từng server
 
  #### **Cài đặt keepalived trên cả 2 gateway**
```
    sudo apt-get install keepalived
  ```
  
  #### tạo hoặc chỉnh sửa file config của keepalive ta dùng:
   ```
   sudo vim /etc/keepalived/keepalived.conf
   ```
  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/keep2.png)         

  
Giá trị ưu tiên (priority) cao hơn được sử dụng để xác định máy chủ nào sẽ làm máy chủ chính (master) trong một cụm hoạt động. Máy chủ với giá trị ưu tiên cao nhất sẽ được chọn làm máy chủ chính (master), trong khi các máy chủ khác sẽ trở thành máy chủ dự phòng (backup).

Khi các máy chủ khởi động hoặc khi có sự thay đổi trong cụm, các máy chủ sẽ gửi tin nhắn VRRP (Virtual Router Redundancy Protocol) chứa giá trị ưu tiên của mình. Máy chủ có giá trị ưu tiên cao nhất sẽ chiếm quyền điều khiển và trở thành máy chủ chính (master).

Vì vậy, trong Keepalived, không quan trọng sử dụng trạng thái nào (ví dụ: trạng thái MASTER hoặc BACKUP), mà chỉ cần xác định máy chủ có giá trị ưu tiên cao nhất là máy chủ chính (master).
  
#### Khởi động lại KeepAlived trên cả 2 server

```
sudo service keepalived start
```
Dùng ip a để kiểm tra:

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/keep3.png)     

Khi máy có ip 192.168.1.10 dừng hoạt động thì máy  dự phòng tự động nhận IP và chuyển giao dịch mạng.  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/keep4.png)     



  



