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
