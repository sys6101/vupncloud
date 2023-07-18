# CLoud storage
Lưu trữ đám mây hay Cloud storage là một thuật ngữ dùng để chỉ các hành động lưu giữ, sắp xếp, quản lý, chia sẻ, và sao lưu dữ liệu của cá thể sở hữu nó trên một hệ thống lưu trữ bên ngoài ổ cứng được duy trì bởi các nhà cung cấp (hay bên thứ ba). Dịch vụ này cho phép khách hàng hay người dùng có thể truy cập được tất cả các tệp tin của họ từ xa tại bất kỳ vị trí địa lý nào.

Tức là, thay vì lưu trữ thông tin vào ổ cứng máy tính của bạn hoặc các thiết bị lưu trữ cục bộ khác như usb chẳng hạn, bạn lưu nó vào 1 hệ cơ sở dữ liệu từ xa. Máy tính của bạn sẽ được kết nối với hệ cơ sở dữ liệu đó thông qua internet, và nhờ kết nối internet, bạn sẽ truy xuất được dữ liệu mình cần thông qua các ứng dụng desktop hay ứng dụng web online.

Lấy một ví dụ đơn giản: Nếu bạn đang tải cái gì đó lên Google Drive của mình thì đó là bạn đang tải 1 tệp dữ liệu lên kho lưu trữ đám mây – cloud storage rồi đó. Và trong trường hợp này thì Google chính là bên cung cấp dịch vụ lưu trữ đám mây.
# 3 loại Cloud Storage chính
## Object storage
Object storage là kiến ​​trúc lưu trữ dữ liệu để lưu trữ dữ liệu phi cấu trúc, phân chia dữ liệu thành các đơn vị—đối tượng—và lưu trữ chúng trong môi trường dữ liệu phẳng có cấu trúc. Mỗi đối tượng bao gồm data, metadata và một mã định danh duy nhất mà các ứng dụng có thể sử dụng để truy cập và truy xuất dễ dàng.

Object có thể được lưu trữ trên máy tính  hoặc trên cloud. Không có giới hạn về số lượng object có thể được lưu trữ, miễn là bạn có khả năng. Ngoài ra, object được đóng gói với metadata có thể tùy chỉnh để mô tả nội dung. Khả năng gắn thẻ file bằng metadata này cho phép bạn lập chỉ mục file dễ dàng.

- Ưu điểm:  
   - Khả năng mở rộng lớn: Dễ dàng mở rộng kiến ​​trúc phẳng của object storage mà không gặp phải những hạn chế giống như file or block storage. Kích thước object storage về cơ bản là vô hạn, vì vậy dữ liệu có thể mở rộng đến hàng exabyte bằng cách thêm thiết bị mới.   
  - Giảm độ phức tạp:Object storage không có thư mục, loại bỏ phần lớn sự phức tạp đi kèm với các hệ thống phân cấp.Điều này  giúp truy xuất tệp dễ dàng hơn vì bạn không cần biết vị trí chính xác.        
  - Hữu ích với metadata được đính kèm vào file. Metadata này có thể được tạo tự động hoặc do người dùng xác định, cho phép thực hiện nhiều kiểu phân tích. Một lợi ích khác là khả năng lưu trữ dữ liệu một cách linh hoạt mà không cần quan tâm đến hệ thống phân cấp. Điều này cho phép khả năng sử dụng tài nguyên lưu trữ ở mức tối đa cũng như việc mở rộng dễ dàng hơn bằng cách chỉ cần thêm node vào trong cụm storage.
- Nhược điểm: Chậm hơn só với hệ thống file or block storage.
## Block storage
Block storage là một loại cloud storage chia file và data thành các khối có kích thước bằng nhau. Phương thức lưu trữ này cho phép truy xuất dữ liệu nhanh chóng vì nó không dựa vào hệ thống tệp.  
Khi muốn truy xuất một tệp, một request được thực hiện với thiết bị block storage . Sau khi request được dich thành block request, file được tập hợp lại sẽ được trả về máy, thiết bị block storage như vậy sẽ tương  tự một ổ cứng tiêu chuẩn.     
- Ưu điểm: 
  - Các hoạt động có độ trễ thấp. Block storage cũng hỗ trợ các loại filesystem bao gồm NTFS, XFS hoặc ext4. Các block (khối) cũng thường được sao chép trên các thiết bị, đảm bảo rằng dữ liệu có thể khôi phục được nếu một thiết bị bị hỏng

  - Hiệu suất cao: Block storage có tốc độ truy cập nhanh và thời gian đáp ứng ngắn, cho phép lưu trữ và truy cập dữ liệu một cách hiệu quả.
  - Ứng dụng đa dạng: Block storage có thể được sử dụng cho nhiều mục đích khác nhau, từ lưu trữ dữ liệu cá nhân đến lưu trữ dữ liệu doanh nghiệp.

- Nhược điểm:

  - Giá thành cao: So với các hình thức lưu trữ khác, block storage có thể đòi hỏi mức phí cao hơn.

  - Khó thay đổi dung lượng: Trong một số trường hợp, việc thay đổi dung lượng của block storage có thể phức tạp và đòi hỏi thời gian và công sức.

  - Yêu cầu kỹ năng kỹ thuật: Để cấu hình và quản lý block storage, người dùng cần có kiến thức kỹ thuật và kỹ năng phù hợp.
## File storage
File storage là một phương pháp lưu trữ dữ liệu theo hệ thống phân cấp. File storage là phương pháp lưu trữ tiêu chuẩn mà hầu hết người dùng quen thuộc. Dữ liệu được lưu trữ ở định dạng giống như dữ liệu được truy xuất. File storage được truy cập thông qua giao thức Server Message Block (SMB) trên Windows hoặc giao thức Network File System (NFS) trên Linux.

SMB và NFS là các giao thức cho phép lưu trữ file trên server theo cách giống như dữ liệu được lưu trữ trên client. Cho phép gắn kết tất cả hoặc một phần hệ thống tệp của mình và chia sẻ quyền truy cập trên nhiều thiết bị khách. Các giao thức này cũng thường được sử dụng với các thiết bị Network-Attached Storage (NAS).

Thiết bị NAS thường được sử dụng để mở rộng quy mô lưu trữ file và cũng có thể được sử dụng dưới dạng bản sao lưu NAS để cung cấp dự phòng.

Trong lưu trữ file, dữ liệu được lưu trữ trong các tệp, các tệp được sắp xếp trong các thư mục và các thư mục được sắp xếp theo hệ thống phân cấp của các thư mục và thư mục con. Để định vị một tệp, tất cả những gì bạn hoặc hệ thống máy tính của bạn cần là đường dẫn—từ thư mục đến thư mục con đến thư mục đến tệp

- Ưu điểm: Quen thuộc với người dùng với hệ thống file phân cấp. File storage phù hợp với mục đích sử dụng văn phòng hoặc thư mục, lưu trữ lượng dữ liệu có cấu trúc nhỏ hoặc lưu trữ file có yêu cầu tính đảm bảo cao.

- Nhược điểm: tính linh hoạt thấp và ít tùy biến hơn so với block storage.
# So sánh
## File Storage vs Block Storage

|Feature       | File Storage  |   Block Storage     |
|------------- | ------------- |---------------------|
Truy cập dữ liệu|	Truy cập ở mức độ tệp|	Truy cập ở mức độ khối|
Chia sẻ dữ liệu	|Thích hợp hơn cho việc chia sẻ dữ liệu|	Không thích hợp cho việc chia sẻ dữ liệu|
Hệ thống tập tin|	Yêu cầu hệ thống tập tin để quản lý tệp|	Không yêu cầu hệ thống tập tin|
Dung lượng|	Có thể xử lý lượng lớn dữ liệu không có cấu trúc|	Thích hợp hơn cho dữ liệu có cấu trúc|
Hiệu suất|	Thường chậm hơn so với block storage|	Thường nhanh hơn so với file storage|
Các trường hợp sử dụng|	Thích hợp cho tài liệu, tệp phương tiện và dữ liệu không có cấu trúc|	Thích hợp cho cơ sở dữ liệu, máy ảo và dữ liệu có cấu trúc|

## File Storage vs Object Storage
|Feature       | File Storage  |   Block Storage     |
|------------- | ------------- |---------------------|
Cấu trúc dữ liệu|	Dữ liệu được tổ chức thành các tệp|	Dữ liệu được tổ chức thành các đối tượng|
Cơ chế truy cập|	Truy cập dữ liệu ở mức độ tệp|	Truy cập dữ liệu ở mức độ đối tượng|
Được tạo ra để lưu trữ|	Tệp phương tiện, tài liệu và dữ liệu không có cấu trúc|	Dữ liệu có cấu trúc và không có cấu trúc|
Khả năng mở rộng|	Khó khăn khi mở rộng để lưu trữ các tệp lớn|	Dễ dàng mở rộng để lưu trữ các đối tượng lớn|
Hiệu suất|	Nhanh hơn so với object storage khi truy cập các tệp nhỏ|	Chậm hơn so với file storage khi truy cập các tệp nhỏ|
Tính khả dụng cao|	Không tốt cho các ứng dụng có yêu cầu khả dụng cao|	Tốt cho các ứng dụng có yêu cầu khả dụng cao|
## Block Storage vs Object Storage
|Feature       | Block Storage  |   Object Storage   |
|------------- | ------------- |---------------------|
Cơ chế truy cập|	Truy cập dữ liệu ở mức độ khối|	Truy cập dữ liệu ở mức độ đối tượng|
Được tạo ra để lưu trữ|	Dữ liệu có cấu trúc, như cơ sở dữ liệu và máy ảo|	Dữ liệu có cấu trúc và không có cấu trúc|
Cơ chế lưu trữ|	Lưu trữ dữ liệu trên các khối được định danh|	Lưu trữ dữ liệu trên các đối tượng được định danh|
Khả năng mở rộng|	Dễ dàng mở rộng để lưu trữ các khối lớn|	Dễ dàng mở rộng để lưu trữ các đối tượng lớn|
Hiệu suất|	Thường nhanh hơn so với object storage|	Chậm hơn so với block storage
Tính khả dụng cao|	Tốt cho các ứng dụng có yêu cầu khả dụng cao|	Tốt cho các ứng dụng có yêu cầu khả dụng cao|