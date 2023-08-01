# Ceph Metadata Server (MDS)
Ceph Metadata Server (MDS) là một thành phần quan trọng trong hệ thống lưu trữ phân tán Ceph, đảm nhiệm vai trò quản lý metadata của hệ thống tập tin CephFS (Ceph File System). MDS giúp theo dõi thông tin về cấu trúc thư mục, quyền truy cập, và các siêu dữ liệu khác liên quan đến tệp và thư mục trong hệ thống.

Khi các máy khách (clients) gửi yêu cầu truy cập dữ liệu trong CephFS, MDS sẽ cung cấp thông tin về định vị tệp và thư mục, giúp máy khách có thể truy xuất dữ liệu một cách hiệu quả. MDS cũng giữ vai trò quan trọng trong việc đảm bảo tính toàn vẹn và đồng bộ hóa metadata giữa các nút lưu trữ (OSD) trong hệ thống Ceph.

![Alt text](/Picture/Storage/ceph-auth.png)