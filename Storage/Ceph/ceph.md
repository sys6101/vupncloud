# Ceph
![Alt text](/Picture/Storage/cephh.png)
## Giới thiệu
Ceph là 1 project mã nguồn mở, cung cấp giải pháp data storage. Ceph cung cấp hệ thống lưu trữ phân tán mạnh mẽ, tính mở rộng, hiệu năng cao, khả năng chịu lỗi cao. Xuất phát từ mục tiêu, Ceph được thiết kết với khả năng mở rộng cao, hỗ trợ lưu trữ tới mức exabyte cùng với tính tương thích cao với các phần cứng có sẵn  

Ceph nối bật khi ngành công nghiệp và storage phát triển và mở rộng. Hiện nay, các nền tảng hạ tầng đám mây public, private, hybird cloud dần trở nên phổ biến và to lớn. Ceph trở thành giải pháp nổi bật cho các vấn đề đang gặp phải.        
Phần cứng là thành phần quyết định hạ tầng cloud và Ceph đáng ứng vấn đề gặp phải, cung cấp hệ thống lưu trữ mạnh mẽ, độ tin cậy cao.      

Nguyên tắc cơ bản của Ceph:         
- Khả năng mở rộng tất cả thành phần      
- Tính chịu lỗi cao       
- Giải pháp dựa trên phần mềm, hoàn toàn mở, tính thích nghi cao      
- Chạy tương thích với mọi phần cứng      
- Ceph xây dựng kiến trúc mạnh mẽ, tính mở rộng cao, hiệu năng cao, nền tảng mạnh mẽ cho doanh nghiệp, giảm bớt sự phụ thuộc vào phần cứng đắt tiền.      
Ceph universal storage system cung cấp khả năng lưu trữ trên block, file, object, cho phép custom theo ý muốn.      
Nền tảng Ceph xây dựng dựa trên object, tổ chức blocks. Tất cả các kiểu dữ liệu, block, file đều được lưu trên object thuộc Ceph cluster. Object storage là giải pháp cho hệ thống lưu trữ truyền thống, cho phép xây dựng kiến trúc hạ tầng độc lập với phần cứng. Ceph quản lý trên mức object, nhân bản obj toàn cluster, nâng cao tính bảo đảm. Tại Ceph, object sẽ không tồn tại đường dẫn vật lý, kiến obj linh hoạt khi lưu trữ, tạo nền tảng mở rộng tới hàng petabyte-exabyte.     


## Kiển trúc, thành phần Ceph

### Lưu Trữ Phân Phối Dữ Liệu Đáng Tin Cây (RADOS)

Yếu tố nền tảng tạo nên Ceph storage cluster. Ceph data được lưu trên object, RADOS chịu trách nhiệm tổ chức, lưu trữ các object, bất kể loại dữ liệu. RADOS layer chắc chắn dữ liệu sẽ luôn chính xác, bảo đảm.Tất cả các phương thức truy cập trên RBD, CephFS, RADOSGW, librados đều hoạt động trên RADOS layer

Về tính nhất quán, dữ liệu sẽ luôn có bản sao, được phát hiện lỗi, khôi phục trên mọi node trong cluster. Khi ứng dụng lưu trữ tới Ceph cluster, dữ liệu sẽ được lưu tại Ceph Object Storage Device (OSD) dưới dạng object. Đây là thành phần duy nhất mà Ceph cluster sử dụng để lưu trữ và truy vấn, đọc ghi dữ liệu. Thông thường, tổng số ổ đĩa vật lý trong Ceph cluster sẽ bằng số lượng tiến trình OSD sử dụng để lưu trữ dữ liệu

### Tiến Trình Giám Sát (Ceph Monitor – Ceph MON)

Là một thành phần tập trung trong hệ thống Ceph, chịu trách nhiệm giám sát trạng thái toàn bộ cụm lưu trữ (cluster), theo dõi trạng thái của các OSD (Object Storage Daemon), MON (Monitor), PG (Placement Group), và CRUSH map (bản đồ CRUSH). Các nút trong cụm lưu trữ sẽ liên tục giám sát và chia sẻ thông tin về các thay đổi xảy ra trong hệ thống.
Tiến trình Giám sát Ceph Monitor (Ceph MON) đóng vai trò quan trọng trong việc duy trì tính toàn vẹn và hiệu suất của hệ thống Ceph. Mỗi node trong cụm lưu trữ được cấu hình để chạy một tiến trình MON, và các tiến trình này liên tục giao tiếp với nhau để đồng bộ hóa thông tin về trạng thái của các thành phần trong cụm.

Các Ceph MON giám sát trạng thái của các OSD, làm việc với Ceph OSD để theo dõi sự sẵn có và trạng thái hoạt động của các thiết bị lưu trữ. Nếu một OSD gặp sự cố hoặc không hoạt động đúng cách, Ceph MON sẽ phát hiện và báo cáo về tình trạng này, giúp hệ thống có thể tự động điều chỉnh và khắc phục vấn đề.

Các MON cũng giám sát các tiến trình MON khác và đồng bộ hóa thông tin về các PG và CRUSH map. PG (Placement Group) quản lý việc phân phối dữ liệu và CRUSH map định nghĩa cách dữ liệu được phân tán và lưu trữ trên các OSD. Bằng cách duy trì đồng bộ hóa này, Ceph MON đảm bảo rằng dữ liệu được phân phối đều đặn và đáng tin cậy trong toàn bộ cụm lưu trữ.

Thông qua việc chia sẻ thông tin và liên tục cập nhật trạng thái, Ceph MON cho phép hệ thống tự động phản ứng với các thay đổi và sự cố, đồng thời cải thiện tính sẵn sàng và hiệu suất của cụm lưu trữ Ceph. Điều này đóng góp vào sự ổn định và hiệu quả của hệ thống lưu trữ phân tán Ceph trong môi trường lưu trữ dữ liệu lớn và có tính sẵn sàng cao.

### Ceph Block Device Hay RADOS Block Device (RBD)

Ceph Block Device (RBD) là một tính năng trong hệ thống Ceph, cho phép tạo và quản lý các thiết bị lưu trữ dạng khối trên Ceph Storage Cluster. RBD cung cấp khả năng lưu trữ dữ liệu theo dạng các "block device" ảo, tương tự như các ổ đĩa trên máy chủ thông thường. Điều này cho phép RBD được sử dụng như một lưu trữ dữ liệu hiệu suất cao và có tính sẵn sàng cao cho các ứng dụng, máy ảo (VMs), hoặc các ứng dụng lưu trữ khác.

### Ceph Object Gateway Hay RADOS Gateway (RGW)

Ceph Object Gateway (RGW) hay RADOS Gateway là một thành phần trong hệ thống lưu trữ phân tán Ceph, cho phép lưu trữ và truy xuất dữ liệu theo mô hình dịch vụ đối tượng (object storage). RGW cung cấp một RESTful API để quản lý các đối tượng dữ liệu, cho phép các ứng dụng và dịch vụ có thể truy xuất và lưu trữ dữ liệu trực tiếp từ hệ thống Ceph.TTương thích với Amazone S3 (Simple Storage Service) và OpenStack Object Storage API (Swift). RGW cũng hỗ trợ OpenStack Keystone authentication services.

### Ceph Metadata Server (MDS)
Ceph Metadata Server (MDS) là một thành phần quan trọng trong hệ thống lưu trữ phân tán Ceph, đảm nhiệm vai trò quản lý metadata của hệ thống tập tin CephFS (Ceph File System). MDS giúp theo dõi thông tin về cấu trúc thư mục, quyền truy cập, và các siêu dữ liệu khác liên quan đến tệp và thư mục trong hệ thống.

Khi các máy khách (clients) gửi yêu cầu truy cập dữ liệu trong CephFS, MDS sẽ cung cấp thông tin về định vị tệp và thư mục, giúp máy khách có thể truy xuất dữ liệu một cách hiệu quả. MDS cũng giữ vai trò quan trọng trong việc đảm bảo tính toàn vẹn và đồng bộ hóa metadata giữa các nút lưu trữ (OSD) trong hệ thống Ceph.

### Ceph File System (CephFS)
Ceph File System (CephFS) là một hệ thống tập tin phân tán và có tính sẵn sàng cao trong hệ thống lưu trữ phân tán Ceph. Nó cung cấp một giao diện tập tin truyền thống cho việc truy xuất và quản lý dữ liệu trên Ceph Storage Cluster. CephFS cho phép các máy khách (clients) truy cập vào các tệp và thư mục dưới dạng hệ thống tập tin tiêu chuẩn, giống như khi làm việc với các hệ thống tập tin trên máy chủ thông thường.

### Ceph Object Storage Daemon– OSD
Trong Ceph có OSD ( viết tắt của "Object Storage Daemon".) là một phần tử quan trọng của hệ thống lưu trữ Ceph và đảm nhiệm việc lưu trữ các đối tượng (object), dữ liệu khối (block) và tệp (file) trên các đĩa cứng. Mỗi OSD sẽ chịu trách nhiệm cho một phần của dữ liệu và đảm bảo sao lưu dữ liệu đó trên nhiều máy chủ khác nhau để đảm bảo tính sẵn sàng và độ tin cậy của hệ thống.

Các OSD trong Ceph hoạt động như các tiến trình độc lập và có thể được triển khai trên các máy chủ vật lý hoặc máy ảo. Các OSD cũng sử dụng các giải thuật cân bằng tải để đảm bảo rằng dữ liệu được phân phối đồng đều trên toàn bộ hệ thống và tránh tình trạng quá tải hay quá tải trên một số máy chủ.

Ceph OSD bao gồm thành phần Ceph OSD filesystem, Linux filesystem nằm phía trên và cuối cùng là Ceph OSD service.Linux filesystem góp phần quan trọng trong tiến trình Ceph OSD như hỗ trợ extended attributes (XATTRs). XATTRs cung cấp các thông tin nội bộ về object state, snap shot, metadata, ACL tới OSD daemon, cho phép quản trị data.

Hoạt động Ceph OSD bên trên ổ đĩa vật lý có phân vùng trong Linux partition. Linux partition có thể là Btrfs (B-tree file system), XFS, or ext4. Việc lựa chọn filesystem góp phần lớn trong việc tính toán hiệu năng trên Ceph Cluster, mỗi file system đều có đặc điểm riêng.

Btrfs: OSD chạy định dạng Btrfs, sẽ cung cấp hiệu năng tốt nhất khi so sánh với XFS, EXT4. Điểm mạnh khi sử dụng Btrfs là nó hỗ trợ các công nghệ mới nhất (copy-on-write, writable snapshots, vm provisioning, cloning v.v). Tuy nhiên Btrfs vẫn chưa ổn định trên một số nền tảng;
XFS: Mang lại sự tin cậy, ổn định vững chắc cho FS. Được khuyên dùng cho hệ thống Ceph Cluster, và hiện tại đang được sử dụng nhiều nhất trên Ceph Storage. Tuy nhiên nó vẫn còn một số đặc điểm kém Btfs và một số vần đề về hiệu suất khi mở rộng Metadata.
XFS thuộc loại Journaling Filesystem, vì vậy, mỗi khi client thao tác với Ceph cluster, đầu tiên nó sẽ ghi vào Journaling Space sau đó mới là XFS file system. Điều này làm tăng chi phi về dữ liệu cũng như khiến XFS chạy chậm hơn Btrfs;
Ext4: Fourth Extended Filesystem, thuộc loại Journaling Filesystem, hỗ trợ tốt Ceph OSD; Tuy nhiên nó không thân thiện bằng XFS. Từ góc độ hiệu năng, Ext4 chưa bằng Btrfs.

### Các command 
Dưới đây là một số câu lệnh liên quan đến các thành phần chính trong hệ thống lưu trữ phân tán Ceph:

**Ceph Monitor (ceph mon):**

- Khởi động Ceph Monitor: systemctl start ceph-mon@{mon-id}
- Tắt Ceph Monitor: systemctl stop ceph-mon@{mon-id}
- Kiểm tra trạng thái các Monitor: ceph quorum_status
  
**Ceph Manager (ceph mgr):**

- Khởi động Ceph Manager: systemctl start ceph-mgr@{mgr-id}
- Tắt Ceph Manager: systemctl stop ceph-mgr@{mgr-id}
- Kiểm tra trạng thái các Manager: ceph mgr status

**Ceph OSD (ceph osd):**

- Khởi động Ceph OSD: systemctl start ceph-osd@{osd-id}
- Tắt Ceph OSD: systemctl stop ceph-osd@{osd-id}
- Kiểm tra trạng thái các OSD: ceph osd status

**Ceph Block Device (RBD):**

- Tạo một pool để lưu trữ RBD: ceph osd pool create {pool-name} {pg-num}
- Tạo một RBD: rbd create {image-name} --size {image-size} --pool {pool-name}
- Gắn một RBD vào hệ thống: rbd map {image-name} --pool {pool-name}
- Xóa một RBD: rbd remove {image-name} --pool {pool-name}

**Ceph RADOS Gateway (RadosGW):**

- Khởi động RadosGW: radosgw -c {path-to-config} --rgw-socket-path /var/run/ceph/ceph-rgw.{rgw-id}.asok
- Tạo người dùng mới trên RadosGW: radosgw-admin user create --uid={user-id} --display-name={display-name}

Lưu ý rằng cần thay thế {mon-id}, {mgr-id}, {osd-id}, {pool-name}, {pg-num}, {image-name}, {image-size}, {rgw-id}, {path-to-config}, {user-id}, và {display-name} bằng các giá trị tương ứng trong hệ thống Ceph của bạn. Các câu lệnh này được thực hiện từ máy chủ quản lý (admin node) hoặc từ các nút trong cụm Ceph.