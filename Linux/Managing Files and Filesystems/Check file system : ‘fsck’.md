## Để kiểm tra và sửa lỗi hệ thống tệp Linux, bạn có thể sử dụng một số công cụ dòng lệnh như fsck, e2fsck hoặc xfs_repair. Dưới đây là các bước cơ bản để kiểm tra và sửa lỗi hệ thống tệp Linux bằng fsck và e2fsck.

### Kiểm tra hệ thống tệp:


``sudo fsck /dev/sda1``     
Trong đó /dev/sda1 là phân vùng bạn muốn kiểm tra. Nếu hệ thống tệp có lỗi, fsck sẽ hiển thị thông báo về lỗi và yêu cầu bạn sửa chúng.

### Sửa lỗi hệ thống tệp:


``sudo fsck -y /dev/sda1``      
Trong trường hợp lỗi khá nghiêm trọng và bạn muốn sửa chúng, bạn có thể sử dụng tùy chọn -y để tự động sửa các lỗi không cần yêu cầu sự can thiệp của bạn.

### Kiểm tra hệ thống tệp sử dụng e2fsck:


```sudo e2fsck -f /dev/sda1```
Tùy chọn -f được sử dụng để yêu cầu e2fsck kiểm tra hệ thống tệp ngay cả khi nó được xác định là "sạch".

### Sửa lỗi hệ thống tệp sử dụng e2fsck:


``sudo e2fsck -y /dev/sda1``    
Tùy chọn -y được sử dụng để tự động sửa các lỗi tìm thấy trong quá trình kiểm tra.

