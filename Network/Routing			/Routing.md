# Giao thức định tuyến

Giao thức định tuyến là một phần quan trọng của mạng máy tính, nó giúp định tuyến dữ liệu từ nguồn đến đích trong mạng. Định tuyến tĩnh là một trong những loại định tuyến được sử dụng trong mạng máy tính, trong đó định tuyến được thực hiện bằng cách cấu hình bằng tay các bảng định tuyến trên các router.

Khi một gói dữ liệu được gửi từ một nguồn đến một đích trong mạng, nó phải đi qua nhiều router trước khi đến được đích. Mỗi router phải quyết định đường đi tốt nhất để chuyển tiếp gói dữ liệu đến router tiếp theo trong đường truyền. Để đảm bảo đường truyền tối ưu, các router cần phải biết về cấu trúc mạng và thông tin định tuyến.

Trong định tuyến tĩnh, bảng định tuyến được cấu hình bằng tay trên router. Bảng định tuyến này chứa thông tin về các đường đi có thể được sử dụng để đến các đích trong mạng. Các đường đi này được xác định bằng cách sử dụng thông tin như địa chỉ IP đích, địa chỉ IP của các router trung gian và metric (thước đo độ dài đường đi).

Một ưu điểm của định tuyến tĩnh là tính đơn giản và độ tin cậy cao. Tuy nhiên, khi mạng có nhiều router và địa chỉ mạng, việc cấu hình bảng định tuyến trở nên phức tạp và dễ gây sai sót.

Ngoài định tuyến tĩnh, còn có định tuyến động được sử dụng trong mạng máy tính. Định tuyến động cho phép các router tự động xác định đường đi tốt nhất để chuyển tiếp gói dữ liệu đến đích. Định tuyến động được thực hiện bằng cách sử dụng các giao thức định tuyến động như OSPF, RIP, BGP, EIGRP, và IS-IS.


# Routing table, cấu hình định tuyến trên Linux	

Routing table (bảng định tuyến) là một bảng dữ liệu được lưu trữ trong một thiết bị mạng (như router hoặc switch) để quản lý các đường đi mạng và chỉ định các giao thức định tuyến để sử dụng cho việc gửi và nhận các gói tin trên mạng.

Bảng định tuyến bao gồm các thông tin về các địa chỉ mạng, các thiết bị mạng kết nối với mạng, các giao thức định tuyến và các đường đi mạng. Khi một thiết bị mạng nhận được một gói tin, nó sẽ sử dụng bảng định tuyến để tìm kiếm đường đi phù hợp nhất để gửi gói tin đó tới đích.

Bảng định tuyến được sử dụng để tối ưu hóa quá trình định tuyến, giúp giảm độ trễ mạng, tăng tốc độ truyền dữ liệu và giảm khả năng xảy ra lỗi trong quá trình gửi và nhận gói tin trên mạng.

Một số giao thức định tuyến phổ biến được sử dụng để cập nhật bảng định tuyến, bao gồm: RIP (Routing Information Protocol), OSPF (Open Shortest Path First), BGP (Border Gateway Protocol) và EIGRP (Enhanced Interior Gateway Routing Protocol). Các giao thức này cung cấp các cơ chế để truyền tải thông tin định tuyến và cập nhật bảng định tuyến trên các thiết bị mạng.


Cấu hình định tuyến: 

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/route1.png)

Routing table

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/route2.png)


