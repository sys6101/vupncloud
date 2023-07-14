# System call
## Định nghĩa
System call là cách 1 chương trình yêu cầu 1 service từ kernel của HĐH mà nó đang chạy. Nó có thể bao gồm các service liên quan đến phần cứng như truy cập HDD, tạo và thực thi các process mới và giao tiếp với kernel service như service lập lịch process. System call cung cấp 1 giao tiếp giữa process và HĐH.

Trong hầu hết các hệ thống, system call chỉ có thể được thực hiện từ các process của user space.

![](https://user-images.githubusercontent.com/83684068/124892365-026b2780-e004-11eb-93f7-b430fe8fdabd.png)

Để user thực thi yêu cầu xuống kernel, kernel cung cấp cho user space các API (còn gọi là các dịch vụ) là system call. System call là cửa ngõ vào kernel, cho phép tiến trình trên tầng user yêu cầu kernel thực thi tác vụ cho mình. Những dịch vụ này có thể là tạo một tiến trình mới (fork), thực thi I/O (read, write), hoặc tạo ra một pipe cho giao tiếp liên tiến trình (IPC).

Giả sử, với một chương trình đơn giản có tác vụ đọc dữ liệu trong một file A, và sao chép nó qua một file B. Để chương trình này hoạt động bình thường, nó cần phải đọc được file A (system call 1: đọc file). Nếu có lỗi xảy ra, chương trình phải xuất một dòng báo lỗi ra màn hình cho người dùng (system call 2: ghi) và thoát chương trình (system call 3: thoát). Nếu không có lỗi sẽ tiến hành tạo file thứ B nếu không có sẵn (system call 4: tạo file) và thực hiện sao chép (system call 5: ghi file).

Các system call phổ biến có thể kể đến open, read, write, close, wait, exec, fork, exit và kill. Ngoài ra, Linux còn có hơn 300 call khác có thể xem tại đây. Trong bài này chỉ tập trung đến các call và tag dùng trong việc flush dữ liệu từ cache xuống disk.

## Flushing strategies
![Alt text](/Picture/Storage/image-5.png)
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
## User space và Kernel space
![Alt text](/Picture/Storage/kernel.png)

Trong kiến trúc máy tính, User space và Kernel space là hai vùng không gian bộ nhớ riêng biệt mà hệ điều hành sử dụng để quản lý các tác vụ và các quyền truy cập vào các tài nguyên hệ thống.

User space là vùng bộ nhớ mà các ứng dụng và tiến trình người dùng được thực thi trong đó. Trong không gian này, các ứng dụng và tiến trình chỉ có thể truy cập vào các tài nguyên và dịch vụ được cấp phép bởi hệ điều hành. Họ không có quyền truy cập trực tiếp vào phần cứng và các tài nguyên hệ thống như bộ nhớ, thiết bị ngoại vi và các file kernel.

Kernel space là vùng bộ nhớ mà hệ điều hành và các driver thiết bị được thực thi trong đó. Trong không gian này, các tiến trình và driver có quyền truy cập vào tất cả các tài nguyên hệ thống, bao gồm các tài nguyên bị ẩn khỏi User space. Kernel space cũng cung cấp các dịch vụ cho User space như quản lý bộ nhớ, quản lý tập tin, và quản lý các thiết bị ngoại vi.

Kernel space và User space được phân biệt bởi các giới hạn phần cứng được cài đặt bởi bộ vi xử lý. Khi một tiến trình được tạo ra, hệ điều hành xác định xem các yêu cầu của nó có thuộc về User space hay Kernel space. Khi một tiến trình hoặc ứng dụng yêu cầu một tài nguyên hệ thống, hệ điều hành sẽ kiểm tra xem nó có quyền truy cập tài nguyên đó hay không và chuyển các yêu cầu tương ứng đến Kernel space để thực hiện.

Tổng quan về User space và Kernel space:

User space là vùng bộ nhớ mà các ứng dụng và tiến trình người dùng được thực thi trong đó.
Kernel space là vùng bộ nhớ mà hệ điều hành và các driver thiết bị được thực thi trong đó.
Kernel space cung cấp quyền truy cập vào tất cả các tài nguyên hệ thống, bao gồm các tài nguyên bị ẩn khỏi User space.
User space và Kernel space được phân biệt bởi các giới hạn phần cứng được cài đặt bởi bộ vi xử lý.