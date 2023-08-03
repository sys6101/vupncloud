# Ceph authentication


Ceph Authentication là quá trình xác thực người dùng khi sử dụng các dịch vụ của Ceph. Các client trong Ceph có thể là end-user, admin hoặc các service/daemon liên quan đến Ceph, ví dụ OSD, monitor hoặc Object Gateways. Để xác định chính xác các loại client cũng như tránh khỏi can thiệp từ các bên thứ 3, Ceph cung cấp hệ thống xác thực tên cephx.

Trong hệ thống xác thực Ceph, mỗi client có một username và một khóa bí mật riêng để xác thực. User yêu cầu ceph client liên hệ với một monitor. Mỗi monitor đều có khả năng xác thực người dùng và phân phối key, do vậy không có "single point of failure" hoặc bottleneck khi sử dụng cephx. Monitor trả về một bộ dữ liệu xác thực gọi là ticket chứa session key để sử dụng các service của Ceph. Session key được mã hóa với các tham số bí mật của user, do vậy chỉ có user đã được chỉ định mới có thể yêu cầu dịch vụ từ Ceph monitor. Client sử dụng session key này để yêu cầu dịch vụ từ monitor, và monitor cung cấp cho user một ticket, ticket này sẽ xác thực người dùng để sử dụng osd. Ticket này giúp client có thể tương tác với bất kỳ osd hoặc mds. Ticket này có giới hạn thời gian.





Trong tổng quan, hệ thống xác thực Ceph đảm bảo tính bảo mật và tin cậy của hệ thống lưu trữ phân tán Ceph bằng cách sử dụng các chứng chỉ và mật khẩu riêng tư, và hỗ trợ nhiều phương thức xác thực khác nhau để đáp ứng nhu cầu sử dụng và cấu hình của hệ thống.

## Quá trình xác thực 

![Alt text](/Picture/Storage/auth2.png)

- Để sử dụng cephx, trước hết quản trị viên phải cấu hình các users. Trong quá trình này, user client.admin sẽ sử dụng lệnh `ceph auth get-or-create-key` để sinh ra username và khóa bí mật. Hệ thống sẽ sinh ra một username và khóa bí mật, lưu một bản sao lại cho mons và chuyển khóa bí mật lại cho user client.admin. Quá trình này mô tả client và mon chia sẻ khóa bí mật.

- Để xác xác thực monitor client chuyển user name cho monitor và monitor sẽ tạo session key và mã hoá nó cùng với sercet key có liên quan đến user name. Sau đó monitor sẽ truyên ticket được mã hoá trở về client. Client sẽ giải mã session key với sercet key và request ticket, monitor sẽ tạo và mã hoá ticket cùng với sercet key của user rồi truyền trở lại cho client. Client sẽ giải mã ticket và dùng nó để sign request đến OSDs và metadata server trong cluster.
- 
![Alt text](/Picture/Storage/auth3.png)

Giao thức cephx xác thực các liên lạc đang diễn ra giữa client và ceph server. Mỗi thông điệp được gửi giữa client và server được ký bởi ticket mà các monitor, OSD, MDS có thể xác thực bằng key chia sẻ:

![Alt text](/Picture/Storage/auth4.png)

 

