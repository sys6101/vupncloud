# Flushing strategies
fsync(): Hàm fsync() là lệnh gọi của hệ điều hành nhằm đảm bảo rằng tất cả dữ liệu đã sửa đổi và metadata được liên kết với tệp được đồng bộ hóa và ghi vào đĩa. Nó đảm bảo độ bền, nghĩa là khi lệnh gọi fsync() trả về, dữ liệu sẽ được lưu trữ an toàn trên đĩa. Tuy nhiên, fsync() có thể là một hoạt động tốn kém về mặt hiệu suất.

fdatasync(): Tương tự như fsync(), hàm fdatasync() cũng đồng bộ hóa dữ liệu và metadata vào đĩa, nhưng nó chỉ đảm bảo đồng bộ hóa dữ liệu tệp chứ không nhất thiết phải là metadata. Điều này có thể làm cho fdatasync() nhanh hơn fsync() vì nó tránh đồng bộ hóa các bản cập nhật metadata không cần thiết.

O_DIRECT: flag này có thể được sử dụng khi mở một tập tin để bỏ qua bộ đệm của hệ điều hành và ghi dữ liệu trực tiếp vào bộ nhớ lưu trữ vĩnh viễn. Điều này có thể rất nhanh, nhưng cũng có thể rủi ro, vì bộ đệm của hệ điều hành được thiết kế để cải thiện hiệu suất và độ tin cậy.

O_SYNC: O_SYNC là flag có thể được sử dụng với các thao tác mở tệp để đảm bảo rằng mỗi thao tác ghi được đồng bộ hóa với đĩa trước khi lệnh gọi ghi hoàn tất. Nó cung cấp chức năng tương tự như fsync(), nhưng việc đồng bộ hóa xảy ra với từng thao tác ghi riêng lẻ, điều này có thể ảnh hưởng đến hiệu suất.

O_DSYNC: O_DSYNC là flag có thể được sử dụng với các thao tác mở tệp để đảm bảo rằng dữ liệu tệp được đồng bộ hóa với đĩa nhưng không nhất thiết phải đồng bộ hóa metadata. Điều này có thể làm cho O_DSYNC nhanh hơn O_SYNC hoặc fsync() vì nó tránh đồng bộ hóa các bản cập nhật metadata không cần thiết.

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





