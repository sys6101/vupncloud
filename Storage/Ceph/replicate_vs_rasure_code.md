# Replicate & Erasure code

## Replicate                                                          

Trong hệ thống lưu trữ phân tán Ceph, khái niệm "replicate" đề cập đến quá trình sao chép dữ liệu từ một OSD (Object Storage Daemon) sang các OSD khác trong mạng lưu trữ. Việc replicate dữ liệu là một phần quan trọng trong việc đảm bảo tính sẵn sàng và bền vững của hệ thống Ceph, vì nó giúp tạo ra bản sao dự phòng của dữ liệu trên nhiều OSD khác nhau để đảm bảo rằng dữ liệu không bị mất khi xảy ra sự cố.

Theo mặc định, pool Ceph được tạo với loại “được sao chép”. Trong nhóm kiểu nhân rộng, mọi đối tượng đều được sao chép vào nhiều đĩa. Việc sao chép nhiều lần này là phương pháp bảo vệ dữ liệu được gọi là “sao chép”.

**Cách hoạt động của Replicate trong Ceph**

Replication Factor: Mỗi PG có một số lượng bản sao dự phòng được xác định bởi Replication Factor (RF). RF xác định số lượng OSDs mà mỗi PG sẽ được replicate lên. Ví dụ, với RF=2, mỗi PG sẽ có 2 bản sao dự phòng.

Replica Placement: Ceph tự động quản lý việc sao chép dữ liệu bằng cách đảm bảo rằng các bản sao dự phòng của PG được đặt trên các OSD khác nhau trong các node khác nhau. Điều này giúp đảm bảo tính sẵn sàng và phân phối tải hiệu quả.

**Cách cấu hình Replicate trong Ceph**

Cấu hình Replication Factor: Để cấu hình RF, bạn cần sửa đổi cấu hình trong tệp ceph.conf hoặc sử dụng câu lệnh CLI. Ví dụ, để đặt RF=2 cho pool có tên là mypool, bạn có thể sử dụng lệnh sau:

    ceph osd pool set mypool size 2

Quản lý Placement Group: Ceph sẽ tự động quản lý việc replicate thông qua các PG. Bạn có thể kiểm tra trạng thái PG và việc replicate bằng cách sử dụng lệnh ceph pg dump hoặc các lệnh tương tự.


## Erasure Code trong Ceph

Erasure Code (mã sửa lỗi) là một phương pháp mã hóa dữ liệu trong hệ thống lưu trữ phân tán Ceph để đảm bảo tính sẵn sàng và bền vững của dữ liệu. Thay vì sử dụng phương pháp replicate truyền thống để sao chép dữ liệu, erasure code chia dữ liệu thành các mảnh nhỏ và mã hóa chúng thành các đoạn mã dự phòng được phân tán trên các OSD (Object Storage Daemon) khác nhau. Erasure code giúp tiết kiệm không gian lưu trữ so với phương pháp replicate truyền thống, đồng thời vẫn đảm bảo tính sẵn sàng và khả năng khôi phục dữ liệu.

Tiết kiệm không gian: Erasure code giúp tiết kiệm không gian lưu trữ so với replicate truyền thống, vì nó sử dụng mã hóa để tạo ra các đoạn mã dự phòng thay vì sao chép đầy đủ dữ liệu.

Đảm bảo tính sẵn sàng: Erasure code vẫn đảm bảo tính sẵn sàng của dữ liệu bằng cách tạo ra các đoạn mã dự phòng trên các OSD khác nhau. Khi một số OSD gặp sự cố, dữ liệu vẫn có thể được khôi phục từ các đoạn mã khác.

Hiệu suất và phân phối tải: Erasure code giúp phân phối tải dữ liệu và tối ưu hiệu suất hệ thống, vì dữ liệu được chia thành các đoạn mã nhỏ và được phân tán trên nhiều OSD khác nhau.

**Cách hoạt động của Erasure Code trong Ceph**

Chunking: Trước khi mã hóa, dữ liệu được chia thành các phần nhỏ gọi là "chunk".

Erasure Encoding: Các chunk sau đó được mã hóa thành các đoạn mã dự phòng bằng cách sử dụng mã sửa lỗi như Reed-Solomon.

Phân phối đoạn mã: Các đoạn mã dự phòng được phân tán trên các OSDs khác nhau. Số lượng đoạn mã được xác định bởi kích thước của erasure code và số lượng đoạn mã dự phòng.

Khôi phục dữ liệu: Khi một OSD gặp sự cố hoặc không hoạt động, dữ liệu vẫn có thể được khôi phục bằng cách sử dụng các đoạn mã dự phòng trên các OSD khác.

**Cách cấu hình Erasure Code trong Ceph**

Erasure Code tương tự như RAID5 và yêu cầu ít nhất ba máy chủ

Tạo pool Erasure Code: Để tạo một pool sử dụng erasure code, bạn cần sử dụng lệnh sau:


    ceph osd pool create [tên_pool] [số_lượng_pgid] [số_lượng_pgp] erasure [plugin] [plugin_options]

Trong đó:
- [tên_pool] là tên của pool mới.         
- [số_lượng_pgid] và [số_lượng_pgp] là số lượng PGs và PGP trên pool.         
- [plugin] là tên của plugin erasure code bạn muốn sử dụng (ví dụ: jerasure, isa, sheepdog).          
- [plugin_options] là các tùy chọn cấu hình cho plugin erasure code. 
Cấu hình plugin Erasure Code: Bạn cần cấu hình plugin erasure code bằng cách chỉ định các tham số như kích thước của chunk, số lượng đoạn mã dự phòng, v.v.

### ERASURE-CODE PROFILES

Erasure-code profiles là một khái niệm liên quan đến lĩnh vực lưu trữ và bảo vệ dữ liệu. Đây là một phần quan trọng của  một phương pháp sử dụng trong hệ thống lưu trữ để tăng cường độ tin cậy và hiệu suất trong việc bảo vệ dữ liệu khỏi mất mát.

Trong ngữ cảnh của hệ thống lưu trữ, Erasure-code là một kỹ thuật sử dụng các phép toán toán học để chia nhỏ dữ liệu thành các phần nhỏ hơn, được gọi là "đoạn mã," và sau đó kết hợp các đoạn mã này để tạo ra các đoạn mã dự phòng. Điều này giúp bảo vệ dữ liệu khỏi mất mát bằng cách cho phép khôi phục lại thông tin ban đầu từ các đoạn mã dự phòng, ngay cả khi một số đoạn mã gốc bị hỏng hoặc mất đi.

Mỗi Erasure-code có thể được cấu hình với các thông số khác nhau, gọi là "Erasure-code profiles" Các hồ sơ này xác định cách mà dữ liệu sẽ được chia thành các đoạn mã, cách chúng được kết hợp để tạo thành các đoạn mã dự phòng, và cách mà các đoạn mã này được sắp xếp và lưu trữ.

Một số ví dụ về các thông số trong Erasure-code profiles có thể bao gồm:

- Chunk size: Điều này xác định kích thước của mỗi đoạn mã. Kích thước này có thể ảnh hưởng đến hiệu suất đọc/ghi và khả năng phục hồi sau lỗi.

- Parity count: Đây là số lượng đoạn mã dự phòng được tạo ra từ các đoạn mã gốc. Nó xác định khả năng chống chịu lỗi của hệ thống.

- rasure-code algorithm: Có nhiều Erasure-code algorithm khác nhau, chẳng hạn như Reed-Solomon, Cauchy Reed-Solomon, XOR-based, và nhiều thuật toán khác.

- Storage layout: Cách mà các đoạn mã và đoạn mã dự phòng được phân bố và lưu trữ trên các thiết bị lưu trữ vật lý, như đĩa cứng hoặc ổ SSD.

- Compute and bandwidth: Hiệu suất tính toán và băng thông cần thiết để thực hiện các phép toán Erasure-code và khôi phục dữ liệu.

## So sánh
**Erasure Coding**

- Hiệu suất và Sử dụng Băng thông: Erasure coding tối ưu hóa việc sử dụng băng thông và dung lượng lưu trữ. Thay vì replicate toàn bộ dữ liệu, nó tạo các đoạn mã dự phòng dựa trên phép toán toán học, giúp giảm thiểu dung lượng lưu trữ cần thiết cho các bản sao dự phòng.

- Tiết Kiệm Dung Lượng: Erasure-code có thể tiết kiệm dung lượng lưu trữ hơn so với replicate, đặc biệt trong các hệ thống lưu trữ lớn.

- Chống Chịu Lỗi: Erasure coding cho phép khôi phục dữ liệu từ các đoạn mã dự phòng ngay cả khi một số đoạn mã gốc bị hỏng, tăng cường khả năng chịu lỗi so với replicate.

- Hiệu suất đọc/ghi: Có thể thấp hơn so với Replicate vì quá trình mã hóa xoá yêu cầu tính toán phức tạp hơn. Tuy nhiên, mức độ này có thể được cải thiện với các tối ưu hóa phần mềm.

**Replication**

- Độ Tin Cậy: replicate đơn giản và dễ hiểu. Nếu một bản sao bị hỏng, dữ liệu vẫn có thể khả phục từ bản sao khác.

- Hiệu suất đọc: Với replicate, hiệu suất đọc thường tốt vì dữ liệu đã được replicate và có nhiều bản sao trên nhiều thiết bị.

- Khả năng Chịu Lỗi: Mất một thiết bị có thể dẫn đến mất dữ liệu nếu không có đủ bản sao. Khả năng chịu lỗi thấp hơn so với Erasure-code.

- Sử Dụng Dung Lượng: replicate yêu cầu dung lượng lưu trữ lớn hơn, vì mỗi bản sao yêu cầu một lượng lưu trữ tương tự như dữ liệu gốc.

- Hiệu suất đọc/ghi: Replicate thường có hiệu suất ghi tốt hơn so với Erasure Code, vì bạn chỉ cần sao chép dữ liệu lên nhiều nút.

- Dung Lượng Đĩa Trống: Để replicate, bạn cần dung lượng đĩa trống đủ lớn để chứa tất cả các bản sao.










