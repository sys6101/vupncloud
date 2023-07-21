# Ceph
Ceph là 1 project mã nguồn mở, cung cấp giải pháp data storage. Ceph cung cấp hệ thống lưu trữ phân tán mạnh mẽ, tính mở rộng, hiệu năng cao, khả năng chịu lỗi cao. Xuất phát từ mục tiêu, Ceph được thiết kết với khả năng mở rộng cao, hỗ trợ lưu trữ tới mức exabyte cùng với tính tương thích cao với các phần cứng có sẵn     
Ceph nối bật khi ngành công nghiệp và storage phát triển và mở rộng. Hiện nay, các nền tảng hạ tầng đám mây public, private, hybird cloud dần trở nên phổ biến và to lớn. Ceph trở thành giải pháp nổi bật cho các vấn đề đang gặp phải.        
Phần cứng là thành phần quyết định hạ tầng cloud và Ceph đáng ứng vấn đề gặp phải, cung cấp hệ thống lưu trữ mạnh mẽ, độ tin cậy cao.       
Nguyên tắc cơ bản của Ceph:         
Khả năng mở rộng tất cả thành phần      
Tính chịu lỗi cao       
Giải pháp dựa trên phần mềm, hoàn toàn mở, tính thích nghi cao      
Chạy tương thích với mọi phần cứng      
Ceph xây dựng kiến trúc mạnh mẽ, tính mở rộng cao, hiệu năng cao, nền tảng mạnh mẽ cho doanh nghiệp, giảm bớt sự phụ thuộc vào phần cứng đắt tiền.      
Ceph universal storage system cung cấp khả năng lưu trữ trên block, file, object, cho phép custom theo ý muốn.      
Nền tảng Ceph xây dựng dựa trên object, tổ chức blocks. Tất cả các kiểu dữ liệu, block, file đều được lưu trên object thuộc Ceph cluster. Object storage là giải pháp cho hệ thống lưu trữ truyền thống, cho phép xây dựng kiến trúc hạ tầng độc lập với phần cứng. Ceph quản lý trên mức object, nhân bản obj toàn cluster, nâng cao tính bảo đảm. Tại Ceph, object sẽ không tồn tại đường dẫn vật lý, kiến obj linh hoạt khi lưu trữ, tạo nền tảng mở rộng tới hàng petabyte-exabyte.     
echo "Setup IP  eth1"
nmcli con modify eth1 ipv4.addresses 192.168.98.85/24
nmcli con modify eth1 ipv4.gateway 192.168.98.1
nmcli con modify eth1 ipv4.dns 8.8.8.8
nmcli con modify eth1 ipv4.method manual
nmcli con modify eth1 connection.autoconnect yes
 

[root@vu-centos8-1 ~]# yum install -y epel-release
[root@vu-centos8-1 ~]# yum update -y

 cat <<EOF> /etc/yum.repos.d/ceph.repo
[ceph]
name=Ceph packages for $basearch
baseurl=https://download.ceph.com/rpm-pacific/el8/x86_64/
enabled=1
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc

[ceph-noarch]
name=Ceph noarch packages
baseurl=https://download.ceph.com/rpm-pacific/el8/noarch
enabled=1
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc

[ceph-source]
name=Ceph source packages
baseurl=https://download.ceph.com/rpm-pacific/el8/SRPMS
enabled=0
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc
EOF
[root@vu-centos8-1 ~]# yum update -y
[root@vu-centos8-1 ~]# yum install -y ceph