# IPtables 
## 1. Khái niệm  
      
Iptables là một ứng dụng tường lửa dựa trên lọc gói rất mạnh, miễn phí và có sẵn trên Linux.

Iptables/Netfilter gồm 2 phần là Netfilter ở trong nhân Linux và Iptables nằm ngoài nhân. Iptables chịu trách nhiệm giao tiếp với người dùng và sau đó đẩy các luật của người dùng vào cho Netfiler xử lí. Netfilter tiến hành lọc các gói dữ liệu ở mức IP. Netfilter làm việc trực tiếp trong nhân, nhanh và không làm giảm tốc độ của hệ thống.
### Table
Iptable sử dụng table để định nghĩa các rules cụ thể cho các gói tin. Các phiên bản Linux hiện nay có 4 loại table khác nhau:

- Đầu tiên phải kể đến filter table: Table này quen thuộc và hay được sử dụng nhất. table này nhằm quyết định liệu gói tin có được chuyển đến địa chỉ đích hay không.

- Tiếp theo là mangle table: Table này liên quan đến việc sửa head của gói tin, ví dụ chỉnh sửa giá trị các trường TTL, MTU, Type of Service.

- Table Nat: Table này cho phép route các gói tin đến các host khác nhau trong mạng NAT table cách thay đổi IP nguồn và IP đích của gói tin. Table này cho phép kết nối đến các dịch vụ không được truy cập trực tiếp được do đang trong mạng NAT.

- Table raw: 1 gói tin có thể thuộc một kết nối mới hoặc cũng có thể là của 1 một kết nối đã tồn tại. Table raw cho phép bạn làm việc với gói tin trước khi kernel kiểm tra trạng thái gói tin.

### Chains.
Mỗi table được tạo với một số chains nhất định. Chains cho phép lọc gói tin tại các điểm khác nhau. Iptable có thể thiết lập với các chains sau:

- Chain PREROUTING: Các rule thuộc chain này sẽ được áp dụng ngay khi gói tin vừa vào đến Network interface. Chain này chỉ có ở table NAT, raw và mangle.

- Chain INPUT: Các rule thuộc chain này áp dụng cho các gói tin ngay trước khi các gói tin được vào hệ thống. Chain này có trong 2 table mangle và filter.

- Chain OUTPUT: Các rule thuộc chain này áp dụng cho các gói tin ngay khi gói tin đi ra từ hệ thống. Chain này có trong 3 table là raw, mangle và filter.

- Chain FORWARD: Các rule thuộc chain này áp dụng cho các gói tin chuyển tiếp qua hệ thống. Chain này chỉ có trong 2 table mangle và table.

- Chain POSTROUTING: áp dụng cho các gói tin đi network interface. Chain này có trong 2 table mangle và NAT.

### Target
Target hiểu đơn giản là các hành động áp dụng cho các gói tin. Đối với những gói tin đúng theo rule mà chúng ta đặt ra thì các hành động (TARGET) có thể thực hiện được đó là:

-  ACCEPT: chấp nhận gói tin, cho phép gói tin đi vào hệ thống.

-  DROP: loại bỏ gói tin, không có gói tin trả lời, giống như là hệ thống không tồn tại.

-  REJECT: loại bỏ gói tin nhưng có trả lời table gói tin khác, ví dụ trả lời table 1 gói tin “connection reset” đối với gói TCP hoặc bản tin “destination host unreachable” đối với gói UDP và ICMP.

- LOG: chấp nhận gói tin nhưng có ghi lại log.

Gói tin sẽ đi qua tất cả các rule chứ không dừng lại khi đã đúng với 1 rule đặt ra. Đối với những gói tin không khớp với rule nào cả mặc định sẽ được chấp nhận.
## Các tùy chọn

Các tùy chọn trong Iptables được phân chia rõ ràng theo từng dạng khác nhau. Tùy chọn để chỉ định thông số Iptables là một trong những tùy chọn cực kỳ quan trọng. Chi tiết thực hiện các tùy chọn chỉ định thông số như sau:

- Chỉ định tên Table: -t
- Chỉ định loại giao thức: – p
- Chỉ định card mạng vào: -i
- Chỉ định card mạng ra: -o
- Chỉ định IP nguồn: -s <địa_chỉ _ip_nguồn>
- Chỉ định IP đích: -d <địa_chỉ_ip_dích>
- Chỉ định cổng nguồn: -sport
- Chỉ định cổng đích: -dport


Rule iptables:
- chỉ allow 1 ip vào 1 port trên server
- drop hết các access vào port nếu ko phải ip này        
  ![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/iptables1.png)
    

Kết nối từ IP được cấp phép:        
    ![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/iptables2.jpg)     
Kết nối từ IP không được cấp phép:      
    ![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/iptables3.jpg)     

  ### Cấp phát DHCP
#### Phía server
Đầu tiên cài đặt DCHP server: 
```
sudo apt install isc-dhcp-server
sudo apt -y install netplan.io

```

Thay đổi IPv4 của interface ens37:
 sudo vim /etc/netplan/00-installer-config.yaml. 

```
network:  
  ethernets:  
    ens33:  
      dhcp4: true 
    ens37:  
      addresses: [192.168.10.2/24]  
      gateway4: 192.168.10.1  
      nameservers:  
        addresses:  [8.8.8.8] 
        search: 
        - vupn.me 
  version: 2  
```
Mở file cấu hình DHCP :
```
sudo vim /etc/dhcp/dhcpd.conf
```

Sưa
```
subnet 192.168.10.0  netmask 255.255.255.0 {
  range 192.168.10.1 192.168.10.254;
  option domain-name-servers 192.168.10.1;
  option domain-name "lan";
  option subnet-mask 255.255.255.0;
  option routers 192.168.10.1;
  option broadcast-address 192.168.10.255;
  default-lease-time 600;
  max-lease-time 7200;
}


```
Gán IP theo một địa chỉ MAC nhất định:
```
host vucl2 {
    hardware ethernet 00:0c:29:18:a3:d8;
    fixed-address 192.168.10.5;
}

```
#### Phía client

Đặt các interface của client theo IP động DHCP, và kết quả cuối cùng:   
Client fix_address  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/iptables2.png)
Client không fix_address  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/iptables3.png)
Dùng sudo systemctl restart isc-dhcp-server để khởi động dịch vụ isc-dhcp-server  

Dùng sudo systemctl enable isc-dhcp-server để bật dịch vụ isc-dhcp-server 


Dùng sudo iptables --table nat --append POSTROUTING -s 192.168.0.0/24 --out-interface ens33 --jump MASQUERADE để thêm rule NAT
lệnh này thiết lập NAT trên máy tính Linux để cho phép các thiết bị trong mạng 192.168.0.0/24 truy cập Internet thông qua giao diện mạng ens33
Lưu cài đặt đã thay đổi với iptables: sudo iptables-save


