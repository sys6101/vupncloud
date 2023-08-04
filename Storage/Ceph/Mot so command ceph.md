# Command Ceph


<!-- **Ceph Monitor (ceph mon):**


- Khởi động Ceph Monitor: `systemctl start ceph-mon@{mon-id}`
- Tắt Ceph Monitor: `systemctl stop ceph-mon@{mon-id}`
- Kiểm tra trạng thái các Monitor: `ceph quorum_status`

- `ceph mon dump`: Lệnh này hiển thị thông tin chi tiết về các Ceph Monitor đang hoạt động trong cluster, bao gồm các thông tin về tên, địa chỉ IP và port của mỗi Monitor.

- `ceph mon stat`: Lệnh này hiển thị trạng thái của các Ceph Monitor, bao gồm các thông tin về số lượng nút Monitor đang hoạt động, số lượng OSD và PG (Placement Group) trên cluster.

- `ceph mon add <mon-name> <mon-ip>`: Lệnh này được sử dụng để thêm một Ceph Monitor mới vào cluster. Bạn cần chỉ định tên và địa chỉ IP của Monitor mới.

- `ceph mon remove <mon-name>`: Lệnh này được sử dụng để xóa một Ceph Monitor khỏi cluster. Bạn cần chỉ định tên của Monitor cần xóa.


- `ceph mon metadata`: Lệnh này hiển thị thông tin metadata về các Ceph Monitor, bao gồm thông tin về tên, địa chỉ IP, port và các thuộc tính khác.

- `ceph mon enable-msgr2`: Lệnh này được sử dụng để kích hoạt giao thức messenger v2 trong Ceph Monitor. Messenger v2 là một giao thức truyền thông hiệu suất cao hơn so với messenger v1.

- `ceph mon feature ls`: Lệnh này hiển thị danh sách các tính năng được kích hoạt trên Ceph Monitor. Các tính năng này bao gồm messenger v1, messenger v2, cephx và các tính năng khác.

Lưu ý rằng các lệnh này phải được thực thi trên node Ceph Monitor của cluster. Bạn có thể sử dụng lệnh `ceph mon` để hiển thị danh sách các lệnh khác liên quan đến Ceph Monitor.
  
**Ceph Manager (ceph mgr):**

- Khởi động Ceph Manager: `systemctl start ceph-mgr@{mgr-id}`
- Tắt Ceph Manager: `systemctl stop ceph-mgr@{mgr-id}`
- Kiểm tra trạng thái các Manager: `ceph mgr status`

- `ceph mgr module ls`: Hiển thị danh sách các module đang chạy trên Ceph Manager.

- `ceph mgr module enable <module-name>`: Bật module được chỉ định trên Ceph Manager.

- `ceph mgr module disable <module-name>`: Tắt module được chỉ định trên Ceph Manager.

- `ceph mgr services`: Hiển thị các dịch vụ đang được quản lý bởi Ceph Manager.

- `ceph mgr module options <module-name>`: Hiển thị các tùy chọn và cấu hình của module được chỉ định.

- `ceph mgr map <object-name>`: Hiển thị thông tin bản đồ (map) của đối tượng được chỉ định.

- `ceph mgr metadata`: Hiển thị thông tin metadata về Ceph Manager, bao gồm thông tin về tên, địa chỉ IP, port và các thuộc tính khác.

- `ceph mgr dump`: Hiển thị thông tin chi tiết về trạng thái của Ceph Manager, bao gồm thông tin về các module, dịch vụ và trạng thái hoạt động của chúng.

- `ceph mgr set <key> <value>`: Cài đặt giá trị cho một khóa cấu hình trên Ceph Manager.

- `ceph mgr get <key>`: Hiển thị giá trị của một khóa cấu hình trên Ceph Manager.


Lưu ý rằng các lệnh này phải được thực thi trên node Ceph Manager của cluster. Bạn có thể sử dụng lệnh `ceph mgr` để hiển thị danh sách các lệnh khác liên quan đến Ceph Manager.

**Ceph OSD (ceph osd):**

- Khởi động Ceph OSD: `systemctl start ceph-osd@{osd-id}`
- Tắt Ceph OSD: systemctl `stop ceph-osd@{osd-id}`
- Kiểm tra trạng thái các OSD: `ceph osd status`

Đây là các lệnh Ceph OSD (ceph osd) phổ biến trong hệ thống Ceph:

- `ceph osd tree`: Hiển thị cây OSD và thông tin chi tiết về các OSD trong cluster.

- `ceph osd status`: Hiển thị trạng thái hoạt động của các OSD trong cluster.

- `ceph osd pool create <pool-name> <pg-num> [pgp-num]`: Tạo một pool mới với tên và số lượng PG được chỉ định.

- `ceph osd pool delete <pool-name> <pool-name> --yes-i-really-really-mean-it`: Xóa một pool khỏi cluster. Lưu ý rằng việc xóa pool sẽ xóa toàn bộ dữ liệu trong pool đó.

- `ceph osd pool set <pool-name> <key> <value>`: Cài đặt giá trị cho một thuộc tính của một pool.

- `ceph osd pool get <pool-name> <key>`: Hiển thị giá trị của một thuộc tính của một pool.

- `ceph osd pool application enable <pool-name> <app-name>`: Bật tính năng ứng dụng cho một pool cụ thể.

- `ceph osd pool application disable <pool-name> <app-name>`: Tắt tính năng ứng dụng cho một pool cụ thể.

- `ceph osd pool set-quota <pool-name> max_bytes <size>`: Thiết lập giới hạn dung lượng tối đa cho một pool.

- `ceph osd pool get-quota <pool-name>`: Hiển thị giới hạn dung lượng của một pool.

- `ceph osd pool set-size <pool-name> <size>`: Thiết lập số lượng bản sao (replicas) của dữ liệu trong một pool.

- `ceph osd pool set-min-size <pool-name> <size>`: Thiết lập số lượng bản sao tối thiểu (minimum replicas) của dữ liệu trong một pool.

- `ceph osd pool set-pg-num <pool-name> <pg-num>`: Thiết lập số lượng PG của một pool.

- `ceph osd pool set-pgp-num <pool-name> <pgp-num>`: Thiết lập số lượng PGP của một pool.

- `ceph osd pool lspools`: Hiển thị danh sách các pool hiện có trong cluster.

Lưu ý rằng các lệnh này phải được thực thi trên một OSD trong cluster. Bạn có thể sử dụng lệnh `ceph osd --help` để hiển thị danh sách các lệnh khác liên quan đến OSD.

**Ceph Block Device (RBD):**

- Tạo một pool để lưu trữ RBD: `ceph osd pool create {pool-name} {pg-num}`
- Tạo một RBD: `rbd create {image-name} --size {image-size} --pool {pool-name}`
- Gắn một RBD vào hệ thống: `rbd map {image-name} --pool {pool-name}`
- Xóa một RBD: `rbd remove {image-name} --pool {pool-name}`

- `rbd create <pool-name>/<image-name> --size <image-size>`: Tạo một ảnh mới với tên được chỉ định và kích thước được cấu hình.

- `rbd rm <pool-name>/<image-name>`: Xóa một ảnh khỏi pool.

- `rbd ls <pool-name>`: Hiển thị danh sách các ảnh trong pool được chỉ định.

- `rbd info <pool-name>/<image-name>`: Hiển thị thông tin chi tiết về một ảnh.

- `rbd resize <pool-name>/<image-name> --size <new-size>`: Thay đổi kích thước của một ảnh.

- `rbd snap create <pool-name>/<image-name>@<snapshot-name>`: Tạo một snapshot mới của một ảnh.

- `rbd snap rm <pool-name>/<image-name>@<snapshot-name>`: Xóa một snapshot của một ảnh.

- `rbd snap ls <pool-name>/<image-name>`: Hiển thị danh sách các snapshot của một ảnh.

- `rbd snap rollback <pool-name>/<image-name>@<snapshot-name>`: Khôi phục một ảnh về trạng thái của một snapshot được chỉ định.

- `rbd snap protect <pool-name>/<image-name>@<snapshot-name>`: Bảo vệ một snapshot của một ảnh khỏi việc xóa.

- `rbd snap unprotect <pool-name>/<image-name>@<snapshot-name>`: Hủy bảo vệ một snapshot của một ảnh.

- `rbd export <pool-name>/<image-name> <file-name>`: Xuất một ảnh sang một tệp tin.

- `rbd import <file-name> <pool-name>/<image-name>`: Nhập một ảnh từ một tệp tin.

- `rbd map <pool-name>/<image-name>`: Ánh xạ một ảnh sang một thiết bị block device.

- `rbd unmap <device-name>`: Hủy ánh xạ một ảnh từ một thiết bị block device.

Lưu ý rằng các lệnh này phải được thực thi trên một node client trong cluster Ceph. Bạn có thể sử dụng lệnh `rbd` để hiển thị danh sách các lệnh khác liên quan đến RBD.

**Ceph RADOS Gateway (RadosGW):**

- Khởi động RadosGW: `radosgw -c {path-to-config} --rgw-socket-path /var/run/ceph/ceph-rgw.{rgw-id}.asok`
- Tạo người dùng mới trên RadosGW: `radosgw-admin user create --uid={user-id} --display-name={display-name}`

- `radosgw-admin user create --uid=<user-id> --display-name=<display-name>`: Tạo một người dùng mới trên RADOS Gateway.

- `radosgw-admin user rm --uid=<user-id>`: Xóa một người dùng khỏi RADOS Gateway.

- `radosgw-admin user modify --uid=<user-id> --display-name=<display-name>`: Sửa đổi thông tin người dùng trên RADOS Gateway.

- `radosgw-admin user info --uid=<user-id>`: Hiển thị thông tin chi tiết về một người dùng trên RADOS Gateway.

- `radosgw-admin user stats --uid=<user-id>`: Hiển thị thống kê về việc sử dụng lưu trữ của một người dùng trên RADOS Gateway.

- `radosgw-admin bucket list --uid=<user-id>`: Liệt kê danh sách các bucket của một người dùng trên RADOS Gateway.

- `radosgw-admin bucket limit check --bucket=<bucket-name> --uid=<user-id>`: Kiểm tra giới hạn dung lượng của một bucket.

- `radosgw-admin bucket limit set --bucket=<bucket-name> --uid=<user-id> --max-objects=<max-objects> --max-size=<max-size>`: Thiết lập giới hạn dung lượng cho một bucket.

- `radosgw-admin object rm --bucket=<bucket-name> --object=<object-name>`: Xóa một đối tượng khỏi một bucket trên RADOS Gateway.

- `radosgw-admin usage show --uid=<user-id>`: Hiển thị thông tin về sử dụng lưu trữ của một người dùng trên RADOS Gateway.

- `radosgw-admin metadata list user` - Hiển thị danh sách các metadata của người dùng.

- `radosgw-admin metadata get user <user-id> <metadata-key>` - Hiển thị giá trị của một metadata của người dùng.

- `radosgw-admin metadata put user <user-id> <metadata-key> <metadata-value>` - Đặt giá trị cho một metadata của người dùng.

- `radosgw-admin metadata rm user <user-id> <metadata-key>` - Xóa một metadata của người dùng.

Lưu ý rằng các lệnh này phải được thực thi trên một node admin trong cluster Ceph. Bạn có thể sử dụng lệnh `radosgw-admin --help` để hiển thị danh sách các lệnh khác liên quan đến RADOS Gateway.
Lưu ý rằng cần thay thế {mon-id}, {mgr-id}, {osd-id}, {pool-name}, {pg-num}, {image-name}, {image-size}, {rgw-id}, {path-to-config}, {user-id}, và {display-name} bằng các giá trị tương ứng trong hệ thống Ceph của bạn. Các câu lệnh này được thực hiện từ máy chủ quản lý (admin node) hoặc từ các nút trong cụm Ceph. -->