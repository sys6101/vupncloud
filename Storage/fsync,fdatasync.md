# Flushing strategies
fsync(): Hàm fsync() là lệnh gọi của hệ điều hành nhằm đảm bảo rằng tất cả dữ liệu đã sửa đổi và metadata được liên kết với tệp được đồng bộ hóa và ghi vào đĩa. Nó đảm bảo độ bền, nghĩa là khi lệnh gọi fsync() trả về, dữ liệu sẽ được lưu trữ an toàn trên đĩa. Tuy nhiên, fsync() có thể là một hoạt động tốn kém về mặt hiệu suất.      
  
Ưu điểm:
- Đảm bảo tính toàn vẹn dữ liệu sau khi ghi vào ổ đĩa.

Nhược điểm:
- Tốc độ thực hiện chậm.
- Tốn nhiều tài nguyên hệ thống.


fdatasync(): Tương tự như fsync(), hàm fdatasync() cũng đồng bộ hóa dữ liệu và metadata vào đĩa, nhưng nó chỉ đảm bảo đồng bộ hóa dữ liệu tệp chứ không nhất thiết phải là metadata. Điều này có thể làm cho fdatasync() nhanh hơn fsync() vì nó tránh đồng bộ hóa các bản cập nhật metadata không cần thiết.   

Ưu điểm:
- Nhanh hơn fsync() do không đồng bộ hóa metadata.
- Đảm bảo tính toàn vẹn dữ liệu khi ghi vào ổ đĩa.

Nhược điểm:
- Không đồng bộ hóa các metadata liên quan đến tệp.

O_DIRECT: flag này có thể được sử dụng khi mở một tập tin để bỏ qua bộ đệm của hệ điều hành và ghi dữ liệu trực tiếp vào bộ nhớ lưu trữ vĩnh viễn. Điều này có thể rất nhanh, nhưng cũng có thể rủi ro, vì bộ đệm của hệ điều hành được thiết kế để cải thiện hiệu suất và độ tin cậy.    

Ưu điểm:
- Giảm thiểu tài nguyên hệ thống.
- Giảm độ trễ khi ghi dữ liệu.

Nhược điểm:
- Phải đọc và ghi trực tiếp vào ổ đĩa, làm tăng tải cho hệ thống.

O_SYNC: O_SYNC là flag có thể được sử dụng với các thao tác mở tệp để đảm bảo rằng mỗi thao tác ghi được đồng bộ hóa với đĩa trước khi lệnh gọi ghi hoàn tất. Nó cung cấp chức năng tương tự như fsync(), nhưng việc đồng bộ hóa xảy ra với từng thao tác ghi riêng lẻ, điều này có thể ảnh hưởng đến hiệu suất.   

Ưu điểm:
- Đảm bảo tính toàn vẹn dữ liệu khi ghi vào tệp.

Nhược điểm:
- Giảm hiệu suất.
- Tốn nhiều tài nguyên hệ thống.


O_DSYNC: O_DSYNC là flag có thể được sử dụng với các thao tác mở tệp để đảm bảo rằng dữ liệu tệp được đồng bộ hóa với đĩa nhưng không nhất thiết phải đồng bộ hóa metadata. Điều này có thể làm cho O_DSYNC nhanh hơn O_SYNC hoặc fsync() vì nó tránh đồng bộ hóa các bản cập nhật metadata không cần thiết.  

Ưu điểm:
- Giảm độ trễ và tăng hiệu suất.

Nhược điểm:
- Không đảm bảo rằng các metadata đã được cập nhật.
- Có thể gây ra mất mát dữ liệu nếu có sự cố xảy ra trong quá trình ghi dữ liệu.

Ví dụ, sau đây là một đoạn mã Python đơn giản mở một file, ghi dữ liệu vào file và đồng bộ hóa dữ liệu với hàm fsync:

```
import os

filename = "file.txt"
flags = os.O_WRONLY | os.O_SYNC
fd = os.open(filename, flags)

os.write(fd, b"Hello World\n")

os.fsync(fd)

os.close(fd)
```

Trong đoạn mã này, chúng ta sử dụng module os để mở file với flag O_SYNC và ghi dữ liệu vào file bằng hàm os.write. Sau đó, chúng ta gọi hàm os.fsync để đồng bộ hóa dữ liệu từ cache đến đĩa.

Dưới đây  sử dụng hàm os.fdatasync để đồng bộ hóa dữ liệu mà không đồng bộ hóa metadata:

```
import os

filename = "file.txt"
flags = os.O_WRONLY | os.O_DSYNC
fd = os.open(filename, flags)

os.write(fd, b"Hello World\n")

os.fdatasync(fd)

os.close(fd)
```

Trong đoạn mã này, chúng ta sử dụng flag O_DSYNC để mở file và gọi hàm os.fdatasync để đồng bộ hóa dữ liệu mà không đồng bộ hóa metadata.
