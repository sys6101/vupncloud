# Ceph Monitor

Ceph monitor (ceph-mon) là một trong những thành phần quan trọng của hệ thống lưu trữ phân tán Ceph. Ceph-mon chịu trách nhiệm giám sát tình trạng của toàn hệ thống và duy trì các thông tin cơ bản về cluster, bao gồm thông tin về monitor, OSD, PG, CRUSH và MDS map.

Ceph-mon hoạt động như một daemon chạy trên mỗi node của cluster Ceph và duy trì các bản sao của cluster map. Các bản sao này chứa thông tin về tình trạng của các node lưu trữ, các OSD đang hoạt động, các pool dữ liệu, cấu hình CRUSH, thông tin về metadata server (MDS), các phiên bản của các phần tử khác trong cluster. Các bản sao này được cập nhật định kỳ và được phân phối trên các node khác nhau để đảm bảo tính nhất quán dữ liệu.

Các cluster map này cung cấp cho client thông tin về vị trí của tất cả các ceph-mon và ceph-osd còn lại trong cluster. Client có thể truy vấn cluster map để xác định vị trí của các OSD và giao tiếp trực tiếp với chúng để đọc hoặc ghi dữ liệu. Việc truy vấn cluster map và tính toán vị trí của các OSD được thực hiện bởi các thư viện Ceph trên client.

Ngoài việc giám sát tình trạng của các OSD và các node trong cluster, ceph-mon cũng cung cấp các dịch vụ xác thực và ghi log. Ceph-mon ghi tất cả các thay đổi vào một bản PAXOS duy nhất và PAXOS sẽ ghi các thay đổi vào kho key-value đảm bảo tính nhất quán tốt nhất. Các bản sao PAXOS cũng được phân phối trên các node khác nhau để đảm bảo tính nhất quán dữ liệu.

Ceph-mon sử dụng snapshot và trình vòng lặp để đồng bộ hóa trên toàn bộ kho lưu trữ. Trình vòng lặp này giúp đảm bảo rằng các bản sao của cluster map được cập nhật đồng bộ trên toàn bộ cluster và đảm bảo rằng các node trong cluster đều có thông tin mới nhất về tình trạng của cluster.

Tổng quan, Ceph-monitor cung cấp cho hệ thống lưu trữ phân tán Ceph tính năng giám sát tình trạng của toàn hệ thống, cung cấp các dịch vụ xác thực và ghi log, duy trì thông tin về các node lưu trữ và đảm bảo tính nhất quán dữ liệu trên toàn bộ cluster.

![Alt text](/Picture/Storage/mon1.png)

