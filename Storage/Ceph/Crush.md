# Crush map

Crush (Controlled Replication Under Scalable Hashing) là một thuật toán được sử dụng trong hệ thống phân tán Ceph để ánh xạ dữ liệu vào các nút lưu trữ. Crush Map đóng vai trò quan trọng trong việc xác định cách dữ liệu được phân phối và sao chép trên các nút trong cụm Ceph.

Crush Map mô tả kiến trúc và cấu hình của hệ thống lưu trữ Ceph. Nó được biểu diễn dưới dạng cây, trong đó các nút lưu trữ và các quy tắc xử lý dữ liệu được xác định. Cây Crush Map có thể có nhiều cấp độ, bắt đầu từ node root và tiếp tục với các nút con.

Các nút trong Crush Map đại diện cho các thiết bị lưu trữ, bao gồm cả OSD (Object Storage Device) và host. Mỗi nút trong cây được định danh bằng một số duy nhất, được gọi là CRUSH ID. Các OSDs thường được đặt tại các leaf nodes của tree, trong khi các host có thể là leaf nodes hoặc intermediate nodes.

Quy tắc Crush Map xác định cách dữ liệu được phân phối và sao chép trong hệ thống. Các quy tắc này được xác định dưới dạng các rule set (tập luật) và các rule . Mỗi rule set chỉ định cách dữ liệu sẽ được xử lý, bao gồm số lượng bản sao, độ ưu tiên và các yếu tố khác.

Các rule xác định cách Crush Map ánh xạ dữ liệu vào các OSDs. Các rule này có thể sử dụng các thuộc tính như weight, type và class để quyết định cách dữ liệu được phân phối. Crush Map sử dụng thuật toán Crush để tính toán và thực hiện việc ánh xạ dữ liệu dựa trên các rule và trạng thái của các nút trong cây.

Một Crush Map chi tiết sẽ bao gồm các thành phần sau:
1. Các nút lưu trữ OSD và host được định danh bằng CRUSH ID.
2. Cấu trúc cây Crush Map với các node root, nút con và leaf nodes.
3. Các quy tắc Crush Map để xác định cách dữ liệu được phân phối và sao chép.
4. Các thuộc tính như weight, type và class được áp dụng cho các rule và nút trong cây.

Crush Map cho phép Ceph phân phối dữ liệu một cách linh hoạt và hiệu quả trên các nút lưu trữ trong hệ thống phân tán. Nó cung cấp khả năng mở rộng và sự đàn hồi trong việc quản lý dữ liệu trên các nút trong cụm Ceph.

**Crush Location**      
Vị trí của OSD trong hệ thống phân cấp của CRUSH map được gọi là vị trí CRUSH của nó. Thông số kỹ thuật của vị trí CRUSH có row, rack, chassis, and host đồng thời cũng là một phần của CRUSH root 'default' (trường hợp của hầu hết các cụm), vị trí CRUSH của nó có thể được chỉ định như sau: `root=default row=a rack=a2 chassis=a2a host=a2a1`

“Bucket”, trong ngữ cảnh của CRUSH, là một thuật ngữ cho bất kỳ nút bên trong nào trong hệ thống phân cấp: hosts, racks, rows...Default types:

- ``osd`` (or ``device``)
- ``host``
- ``chassis``
- ``rack``
- ``row``
- ``pdu``
- ``pod``
- ``room``
- ``datacenter``
- ``zone``
- ``region``
- ``root``

Mỗi node (device or bucket) trong cấu trúc phân cấp có weight cho biết tỷ lệ tương đối của tổng dữ liệu sẽ được lưu trữ bởi thiết bị đó hoặc cây con cấu trúc phân cấp. Weight được đặt ở các lá, cho biết kích thước của thiết bị. Weight thường được đo bằng tebibyte (TiB).
Để có cái nhìn đơn giản về hệ thống phân cấp CRUSH của cụm, bao gồm cả trọng số, hãy chạy lệnh sau:
```
$ ceph osd tree

ID  CLASS  WEIGHT   TYPE NAME        STATUS  REWEIGHT  PRI-AFF
-1         0.05878  root default                              
-7         0.01959      host ceph01                           
 2    hdd  0.00980          osd.2        up   1.00000  1.00000
 5    hdd  0.00980          osd.5        up   1.00000  1.00000
-3         0.01959      host ceph02                           
 0    hdd  0.00980          osd.0        up   1.00000  1.00000
 3    hdd  0.00980          osd.3        up   1.00000  1.00000
-5         0.01959      host ceph03                           
 1    hdd  0.00980          osd.1        up   1.00000  1.00000
 4    hdd  0.00980          osd.4        up   1.00000  1.00000

```


Xem các weight được dùng ở các node và được gắn nhãn hoặc pool từ lệnh: 

```
ceph osd crush tree
```
Thêm xóa một OSD:
```
ceph osd crush set {name} {weight} root={root} [{bucket-type}={bucket-name} ...]
ceph osd crush set osd.0 1.0 root=default datacenter=dc1 room=room1 row=foo rack=bar host=foo-bar-1
```
Trong đó:
- `name` tên OSD, ví dụ osd.0
- `weight`  CRUSH weight cho OSD, thường được tính bằng terabytes (TB).Ví dụ 1.0
- `root` root node của tree (thường là default). Ví dụ root=default
- `bucket-type` vị trí của osd trong CRUSH. Ví dụ: datacenter=dc1 room=room1 row=foo rack=bar host=foo-bar-1

Điều chỉnh weight một OSD CRUSH trong CRUSH map :
```
ceph osd crush reweight {name} {weight}
```
Xóa một OSD từ CRUSH map:
```
ceph osd crush remove {name}
```

Thêm một bucket:
```
ceph osd crush add-bucket {bucket-name} {bucket-type}
```

PGs:  

    pg num = (OSD count) / (pool size)

Trong đó:
- OSD count là tổng số OSD trong cluster.
- Pool size là số OSD được gán cho pool đó