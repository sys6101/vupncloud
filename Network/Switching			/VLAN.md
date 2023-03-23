# Virtual Local Area Network
VLAN (Virtual Local Area Network) là một kỹ thuật được sử dụng để phân chia mạng LAN vật lý thành các mạng LAN ảo, mang lại tính linh hoạt, bảo mật và quản lý hiệu quả hơn. VLAN giúp giảm thiểu việc quảng bá thông tin, tăng bảo mật và đơn giản hóa việc quản lý mạng.


Phân chia mạng: VLAN cho phép phân chia mạng LAN vật lý thành nhiều mạng LAN ảo dựa trên các tiêu chí khác nhau, như vị trí địa lý, phòng ban hoặc chức năng.

## Trunking

Cấu hình trunk: Cấu hình đường truyền trunk giữa các switch để cho phép thông tin của nhiều VLAN truyền qua một đường truyền vật lý.
Cấu hình định tuyến giữa các VLAN (nếu cần thiết): Nếu bạn muốn cho phép giao tiếp giữa các VLAN, cần cấu hình định tuyến giữa các VLAN bằng cách sử dụng một thiết bị định tuyến (router) hoặc một switch lớp 3 (Layer 3 switch) hỗ trợ định tuyến giữa các VLAN.      

Cơ chế hoạt động trong VLAN Trunking Protocol(VTP):

- VTP thêm bớt và chỉnh sửa các VLAN      
- VTP gửi thông điệp quảng bá "VTP domain" 5 phút 1 lần hoặc khi có sự thay đổi xảy ra trong cấu hình VLAN        
- Thông điệp VTP bao gồm: rivision number, VLAN name, VLAN number, thông tin switch có cổng gắn với mỗi VLAN      
- Với mỗi lần sửa đổi, số revision sẽ tăng lên, cho phép client dễ dàng theo dõi các sự kiện xảy ra       
- Các VTP client sẽ được đồng bộ với những thay đổi trên VTP server       
- Bên cạnh đó còn có VTP trasparent, đóng vai trò chỉ forward thông điệp quảng và mà không đồng bộ với chúng

## Các loại VLAN
- VLAN 1: là kiểu mạng mặc định của tất cả các thiết bị hỗ trợ VLAN, tất cả các cổng mạng trên thiết bị mặc định đều nằm trong cùng một miền quảng bá và dưới quản lý của VLAN 1.

- Default VLAN: Là kiểu VLAN mặc định ban đầu với tất cả các interface trên một thiết bị, vì vậy cũng có thể hiểu là VLAN 1

- User VLAN: là VLAN trong đó chứa các tài khoản người dùng thành từng nhóm dựa theo các thuộc tính như phòng ban, chức năng

- Native VLAN: dùng để cấu hình Trunking với một số thiết bị cũ không tương thích, lúc này cần set native VLAN để chúng có thể giao tiếp với nhau mà không cần tag VLAN khi đi qua trunk. Mặc định Native VLAN cũng là VLAN 1, tuy nhiên cần phải đổi để đạt được tính bảo mật.


## Routing giữa các VLAN

Routing giữa các VLAN cho phép các thiết bị truy cập mạng ở các VLAN khác nhau có thể giao tiếp với nhau thông qua địa chỉ IP. Để thực hiện routing giữa các VLAN, có thể sử dụng một trong các giải pháp sau:

Sử dụng một router ở chế độ định tuyến trung gian giữa các VLAN: Router được cấu hình để có các đường đi tới các mạng VLAN khác nhau. Khi một thiết bị truy cập mạng trong một VLAN muốn giao tiếp với một thiết bị truy cập mạng trong một VLAN khác, các gói tin sẽ được gửi đến router, sau đó router sẽ định tuyến chúng đến VLAN đích. Điều này cho phép các thiết bị truy cập mạng trong các VLAN khác nhau giao tiếp với nhau.

Sử dụng một switch layer 3: Một switch layer 3 có thể thực hiện cả chức năng switch và định tuyến, giúp cho các VLAN có thể giao tiếp với nhau thông qua địa chỉ IP. Switch layer 3 có thể được cấu hình để tạo các mạng con ảo (SVI) cho từng VLAN, đồng thời có thể thực hiện các chức năng định tuyến trên các mạng con ảo này để đảm bảo các thiết bị trong các VLAN khác nhau có thể giao tiếp với nhau.

Sử dụng các thiết bị firewall thông minh: Các thiết bị firewall thông minh có khả năng định tuyến giữa các mạng VLAN khác nhau. Chúng có thể được cấu hình để chuyển tiếp các gói tin giữa các VLAN và kiểm soát quyền truy cập giữa chúng.

Tùy thuộc vào nhu cầu và tài nguyên của hệ thống mạng, các giải pháp trên có thể được sử dụng riêng lẻ hoặc kết hợp với nhau để đảm bảo hiệu suất tốt nhất và tính linh hoạt cao nhất trong quá trình routing giữa các VLAN.




