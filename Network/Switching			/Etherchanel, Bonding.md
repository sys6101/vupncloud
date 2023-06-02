# EtherChannel

EtherChannel là một kỹ thuật nhóm hai hay nhiều đường kết nối truyền tải dữ liệu vật lý (Link Aggregation) thành một đường ảo duy nhất (Logic) có Port ảo thậm chí cả MAC ảo nhằm mục đích tăng tốc độ truyền dữ liệu và tăng khả năng dự phòng (Redundancy) cho hệ thống.

Các Switch phải đều phải hỗ trợ kỹ thuật EtherChannel và phải được cấu hình EtherChannel đồng nhất giữa các Port kết nối với nhau.

Các Port kết nối EtherChannel giữa 2 Switch phải tương đồng với nhau:       
- Cấu hình (Configuration)        
- Tốc độ (Speed)      
- Băng thông (Bandwidth)      
- Duplex (Full Duplex)        
- Native VLAN và các VLANs        
- Switchport Mode (Trunking, Access)  


## Phân loại EtherChannel:
Có 2 loại giao thức EtherChannel:

###  LACP (Link Aggregation Control Protocol):
Là giao thức cấu hình EtherChannel chuẩn quốc tế IEEE 802.3ad và có thể dùng được cho hầu hết các thiết bị thuộc các hãng khác nhau, LACP hỗ trợ ghép tối đa 16 Link vật lý thành một Link luận lý (8 Port Active – 8 Port Passive).        
LACP có 3 chế độ:
On: Chế độ cấu hình EtherChannel tĩnh, chế độ này thường không được dùng vì các Switch cấu hình EtherChannel có thể hoạt động được và cũng có thể không hoạt động được vì các Switch được cầu hình bằng tay phục thuộc vào con người nên hoàn toàn không có bước thương lượng trao đổi chính sách giừa bên dẫn đến khả năng Loop cao và bị STP Block.       
Active: Chế độ tự động – Tự động thương lượng với đối tác
Passive: Chế độ bị động – Chờ được thương lượng 
### PAgP (Port Aggregation Protocol):
Là giao thức cấu hình EtherChannel độc quyền của các thiết bị hãng Cisco và chỉ hỗ trợ ghép tối đa 8 Link vật lý thành một Link luận lý.        

PAgP cũng có 3 chế độ tương tự LACP:

- On
- Active
- Passive
# Interface network bonding
Linux cho phép quản trị viên cấu hình bonding interface từ 2 đến nhiều interface vật lý (NIC) kết hợp lại thành 1 interface ảo có tên là ‘bonding interface‘, bằng cách sử dụng module kernel ‘bonding’ trên Linux. 

Giúp cho 2 hoặc nhiều network interface hoạt động như 1 card mạng interface, cũng như các lợi ích mà tính năng ‘bonding‘ mang đến như: tăng bandwidth, tăng tính dự phòng card mạng network, cân bằng tải mức cơ bản ở tầng network server, v.v.

![image](https://1.bp.blogspot.com/-pC3ffDXCgaU/VYppfrn-c0I/AAAAAAAABjo/f8Et3butsXk/s1600/bonding_linux.jpg)

Bonding interface còn có cơ chế dự phòng khi 1 card mạng NIC vật lý chết hoặc bị tháo dây mạng, card mạng bonding sẽ tự động chuyển đổi hoạt động network sang card mạng còn lại.



# LABS
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/mohinh1.png) 

Cấu hình địa chỉ ip 192.168.3.2 cho linux1   


![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/b1.png)        
Cấu hình địa chỉ ip 192.168.3.3 cho linux2   


![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/b2.png)                
Cấu hình Vlan đưa 2 interface vào chung Vlan 10             
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/vlan10.png)                
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/vlan101.png)    


    


Gộp 2 interface vật lý là eth1 và eth2 lại thành 1 interface ảo có tên là bond0. Cấu hình tại folder netplan /etc/netplan/


![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/b3.png)    

Có thể thấy card bond0 sẽ có flag 'MASTER' và 2 card eth1 và eth2 có có flag 'SLAVE'

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/b4.png) 


VLAN

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/bondvlan.png) 

