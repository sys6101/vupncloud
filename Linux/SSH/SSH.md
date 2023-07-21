# SSH
SSH là viết tắt của Secure Shell, và nó là một giao thức cho phép quản trị viên mạng kết nối với các máy chủ từ xa qua kết nối Internet không an toàn. Hơn nữa, SSH cung cấp các bộ tiện ích cho sự phát triển chính của giao thức SSH.

SSH thiết lập một cơ chế xác thực mật khẩu mạnh bằng cách thiết lập một liên kết giao tiếp dữ liệu được mã hóa giữa hai máy qua Internet. Giao thức SSH hiện được các nhà quản trị mạng sử dụng rộng rãi trong quá trình quản lý và điều chỉnh ứng dụng từ xa. Nó cho phép bạn đăng nhập thủ công vào mạng máy tính và thực hiện các tác vụ cơ bản như truyền tệp.

Các chức năng cơ bản có thể kể đến như sau:     
- Hỗ trợ truy cập từ xa vào các hệ thống và thiết bị ứng dụng giao thức SSH.      
- Cho phép truyền tệp an toàn.        
- Sử dụng hệ thống điều khiển từ xa để thực hiện các lệnh an ninh và an toàn.
- Quản lý các thành phần cơ sở hạ tầng mạng an toàn và hiệu quả.      

## Mã hóa Symmetric Encryption
Mã hóa đối xứng là một phương pháp mã hóa ứng dụng khóa bí mật để giải mã thông điệp cho cả máy chủ và máy khách. Do đó, bất kỳ ai sở hữu khóa đều có thể giải mã thông điệp trong quá trình truyền.

Khóa đối xứng được sử dụng để mã hóa hoàn toàn tất cả các giao dịch giao thức SSH. Đặc biệt, khi bạn tìm hiểu về mã hóa Symmetric Encryption của SSH là gì, bạn nhớ máy chủ và máy khách chịu trách nhiệm tạo khóa bí mật không được tiết lộ cho bên thứ ba. Thuật toán rất an toàn vì khóa không được truyền giữa máy khách và máy chủ. Cả hai máy tính đều có thể chia sẻ thông tin chung, dựa vào đó xác định được khóa bí mật. 

Khóa bí mật không thể bị phát hiện bởi bất kỳ máy tính nào khác, cho dù nó có thể nắm bắt thông tin hay không. Tuy nhiên, cần lưu ý rằng Secret Token chỉ có thời hạn dùng trong một phiên SSH, nơi nó được hình thành thông qua xác thực máy khách. Khi một khóa mới được tạo, tất cả các Packets giữa hai máy phải được mã hóa bằng khóa riêng. Người dùng phải cung cấp mật khẩu như một phần của quá trình này.

## Mã hóa Asymmetric Encryption 
Trái ngược với mã hóa đối xứng, mã hóa không đối xứng sử dụng hai khóa riêng biệt để mã hóa và giải mã. Cặp khóa Public Key – Private Key được tạo thành từ Public Key và Private Key. Public Key hiển thị cho tất cả các thành phần liên quan. Tuy nhiên, nó liên quan trực tiếp đến khóa cá nhân Private Key. Sự phụ thuộc này khiến cho Public Key gần như không thể tự mã hóa các tin nhắn trong khi cũng có thể giải mã bất kỳ thứ gì được mã hóa bởi Private Key.

Trong khi đó, Private Key luôn được giữ kín và không bao giờ được chia sẻ với bên thứ ba. Tin nhắn có thể được giải mã bằng Private Key. Kết quả là khi một bên giải quyết thành công tin nhắn được gửi đến Public Key, thì bên kia sẽ sở hữu Private Key.

Tuy nhiên, nếu chưa rõ SSH là gì, cần lưu ý rằng mã hóa không đối xứng không thể mã hóa tất cả SSH. Nó chỉ có thể áp dụng khi trao đổi thuật toán khóa. Trước khi một phiên có thể bắt đầu, cả hai bên phải đồng ý tạo một cặp Public Key – Private Key trong thời gian ngắn hạn. Chia sẻ Private Key cùng một lúc để tạo Secret Key chung.

Máy chủ sử dụng Public Key của máy khách mỗi khi liên kết đối xứng chính thức được thiết lập an toàn. Sau đó, đối với quá trình xác thực, hãy khởi tạo, thay đổi và truyền đến máy khách. Nếu Khách hàng giải mã thành công tin nhắn, tức là họ đang sở hữu Private Key. Đồng thời, phiên SSH được khởi chạy.


