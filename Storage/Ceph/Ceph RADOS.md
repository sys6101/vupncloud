# Ceph RADOS

![Alt text](/Picture/Storage/rados1.png)

RADOS (Reliable Autonomic Distributed Object Store) là yếu tố nền tảng tạo nên Ceph storage cluster. Ceph data được lưu trên object, RADOS chịu trách nhiệm tổ chức, lưu trữ các object, bất kể loại dữ liệu. RADOS layer chắc chắn dữ liệu sẽ luôn chính xác, bảo đảm.Tất cả các phương thức truy cập trên RBD, CephFS, RADOSGW, librados đều hoạt động trên RADOS layer

Về tính nhất quán, dữ liệu sẽ luôn có bản sao, được phát hiện lỗi, khôi phục trên mọi node trong cluster. Khi ứng dụng lưu trữ tới Ceph cluster, dữ liệu sẽ được lưu tại Ceph Object Storage Device (OSD) dưới dạng object. Đây là thành phần duy nhất mà Ceph cluster sử dụng để lưu trữ và truy vấn, đọc ghi dữ liệu. Thông thường, tổng số ổ đĩa vật lý trong Ceph cluster sẽ bằng số lượng tiến trình OSD sử dụng để lưu trữ dữ liệu.


![Alt text](/Picture/Storage/rados2.png))

RADOS (Reliable Autonomic Distributed Object Store) là một hệ thống lưu trữ phân tán được sử dụng trong hệ thống Ceph. Đây là một hệ thống lưu trữ đối tượng (object storage) và cung cấp khả năng lưu trữ dữ liệu trên các thiết bị lưu trữ (storage devices) khác nhau trong một cluster. Dưới đây là một số thành phần quan trọng trong RADOS:

1. OSD (Object Storage Daemon): OSD là thành phần chính của RADOS và quản lý việc lưu trữ dữ liệu trên các thiết bị lưu trữ (disk drives hoặc SSDs). Mỗi OSD quản lý một phần của dữ liệu của cluster và có thể xử lý các yêu cầu lưu trữ và truy cập dữ liệu.

2. MON (Monitor): MON là thành phần quản lý metadata của cluster, bao gồm thông tin về các OSD, các pool lưu trữ, các máy khách (clients) và các đối tượng (objects). MON sử dụng một cơ sở dữ liệu key-value để lưu trữ metadata.

3. MDS (Metadata Server Daemon): MDS là thành phần quản lý metadata cho Ceph File System (CephFS). MDS lưu trữ thông tin về các tập tin và thư mục trong hệ thống tệp CephFS.

4. CRUSH (Controlled Replication Under Scalable Hashing): CRUSH là một thuật toán phân tán dữ liệu được sử dụng để xác định vị trí lưu trữ cho các đối tượng trên các OSD trong cluster. CRUSH sử dụng một bản đồ phân tán (distribution map) để ánh xạ các đối tượng vào các OSD.

5. RADOS Gateway (RadosGW): RadosGW là một thành phần RADOS cung cấp khả năng lưu trữ đối tượng trên một cổng HTTP. RadosGW cho phép người dùng truy cập và quản lý dữ liệu trên RADOS thông qua giao diện web.

6. librados: librados là một thư viện phần mềm được sử dụng để truy cập đến hệ thống lưu trữ RADOS. librados cung cấp các API để xử lý các yêu cầu đến OSD và các yêu cầu liên quan đến metadata.

Tất cả các thành phần này hoạt động cùng nhau để cung cấp một hệ thống lưu trữ phân tán đáng tin cậy và có khả năng mở rộng trong hệ thống Ceph.