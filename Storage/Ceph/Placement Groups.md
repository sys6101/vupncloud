# PLACEMENT GROUP

Placement Groups ẩn với Ceph clients nhưng chúng có vai trò quan trọng trong Ceph Storage Clusters.

PG là một khái niệm quan trọng trong hệ thống Ceph, liên quan đến cách dữ liệu được phân phối và quản lý trên các OSD (Object Storage Daemon) trong cụm Ceph. Mỗi PG đại diện cho một phần của dữ liệu và các PG được sử dụng để cân bằng tải và quản lý việc phân phối dữ liệu trên các OSD.

Trong hệ thống Ceph, các bước cơ bản để làm việc với PG (PG) bao gồm:

Tạo Pool|Trước tiên, bạn cần tạo một "pool" (nhóm lưu trữ) trong Ceph. Mỗi pool sẽ chứa một số lượng PG.

Đặt Số Lượng PG Cho Pool|Bạn cần xác định số lượng PG mà pool đó sẽ có. Số lượng PG ảnh hưởng đến việc cân bằng tải và hiệu suất của hệ thống.

Cân Bằng PG|Hệ thống Ceph sẽ cố gắng cân bằng PG trên các OSD để đảm bảo rằng dữ liệu được phân phối đồng đều và tận dụng tốt các tài nguyên.


Quản Lý Sự Cố PG|Khi có sự cố xảy ra, như OSD bị lỗi hoặc mất kết nối, hệ thống sẽ tự động chuyển trạng thái của PG và thực hiện các hoạt động khôi phục.

Tối Ưu Hóa Cấu Hình PG|Có thể điều chỉnh cấu hình số lượng PG, loại cân bằng tải, và các tham số khác để tối ưu hóa hiệu suất của hệ thống.

Các PG (PG) là một phần quan trọng của việc quản lý và triển khai hệ thống lưu trữ Ceph. Chúng giúp cân bằng tải, đảm bảo tính toàn vẹn dữ liệu và đảm bảo hiệu suất ổn định.

<!-- ## HOW ARE PLACEMENT GROUPS USED ?
## AUTOSCALING PLACEMENT GROUPS

Bạn có thể cho phép cluster đưa ra đề xuất hoặc tự động điều chỉnh PG bằng cách `enabling pg-autoscaling`

Mỗi pool trong hệ thống có pg_autoscale_mode có thể đặt là `off`, `on`, `warn`.
Ví dụ:

    ceph osd pool set <pool-name> pg_autoscale_mode <mode>
    ceph osd pool set pool1 pg_autoscale_mode on

Bạn có thể đặt mặc định pg_autoscale_mode khi tạo các pool sau:

    ceph config set global osd_pool_default_pg_autoscale_mode <mode>

Bạn có thể xem từng pool, mức độ sử dụng tương đối của nhóm và mọi thay đổi được đề xuất đối với số lượng PG bằng lệnh này:

    [root@ceph01 ~]# ceph osd pool autoscale-status
    POOL                     SIZE  TARGET SIZE  RATE  RAW CAPACITY   RATIO  TARGET RATIO  EFFECTIVE RATIO  BIAS  PG_NUM  NEW PG_NUM  AUTOSCALE  BULK                                                                  
    device_health_metrics      0                 3.0        30708M  0.0000                                  1.0       1              on         False                                                                 
    pool1                  512.1M                3.0        30708M  0.0500                                  1.0      32              on         False                                                                 
    pool2                  512.1M                3.0        30708M  0.0500                                  1.0      32              on         False 

- SIZE|số lượng dữ liệu được lưu trữ trong pool.
- RATE|hệ số nhân cho pool xác định dung lượng lưu trữ thô dược tiêu thụ
- RAW CAPACITY|tổng dung lượng lưu trữ thô -->

## PLACEMENT GROUP STATES

PLACEMENT GROUP STATES trong Ceph là các trạng thái mà một PLACEMENT GROUP có thể trải qua trong quá trình hoạt động của hệ thống lưu trữ Ceph. Mỗi trạng thái đại diện cho tình trạng hiện tại của PG và có ý nghĩa cụ thể đối với quá trình xử lý dữ liệu và quản lý trong Ceph. Các trạng thái này cung cấp thông tin về việc PG đã hoàn thành việc sao chép, kiểm tra, sửa chữa, đồng bộ hóa và các hoạt động khác liên quan đến quản lý dữ liệu và đảm bảo tính toàn vẹn của hệ thống.

| Trạng Thái|Info| Ghi chú |         
|-----------|--------------|-------|
| Creating        | Khi một pool mới đang được tạo trong Ceph cluster|
| Activating | Khi một pool hoặc đối tượng đang được kích hoạt để có thể sử dụng.|
|Active|Khi một pool hoặc đối tượng đã được kích hoạt và sẵn sàng để sử dụng.|
|Clean|Khi các dữ liệu sao lưu đã được sao chép hoặc di chuyển và cluster đang trong trạng thái hoạt động bình thường.
|Down|Khi một phần tử trong Ceph cluster (như một máy chủ OSD) không hoạt động.|
|Scrubbing|Khi Ceph đang thực hiện quá trình kiểm tra dữ liệu trên các OSD để phát hiện lỗi hoặc sự không nhất quán.|
|Deep|Một giai đoạn của quá trình scrubbing, trong đó Ceph kiểm tra toàn bộ dữ liệu.|
Degraded|Khi mất mát dữ liệu đã xảy ra và Ceph cluster đang hoạt động dưới dạng bất thường.|Trạng thái này đề cập đến việc hệ thống hoạt động không bình thường do mất mát dữ liệu. Điều này có thể là một vấn đề quan trọng, đặc biệt nếu mất mát dữ liệu ảnh hưởng đến tính sẵn sàng và toàn vẹn của dữ liệu
Inconsistent|Khi dữ liệu không nhất quán giữa các OSD trong cluster.|Dữ liệu không nhất quán có thể đưa đến việc không thể truy cập dữ liệu hoặc dẫn đến sự không rõ ràng về trạng thái của hệ thống.
Peering|Khi các OSD đang cố gắng cập nhật dữ liệu từ các OSD khác.|Quá trình peering cần thiết để đồng bộ hóa dữ liệu giữa các phần tử của hệ thống. Khi quá trình này gặp vấn đề, hệ thống có thể gặp khó khăn trong việc cập nhật dữ liệu.
Repair|Khi Ceph đang cố gắng sửa chữa dữ liệu bị hỏng.|Việc sửa chữa dữ liệu bị hỏng đòi hỏi sự can thiệp để khôi phục dữ liệu, do đó có thể gây ra gián đoạn trong hoạt động của hệ thống.
Recovering|Khi dữ liệu đang được khôi phục từ các sao lưu.
Forced Recovery|Một giai đoạn khôi phục khi dữ liệu bị hỏng và cần can thiệp thủ công.
Recovery Wait|Khi đợi cho quá trình khôi phục hoàn thành.
Recovery Toofull|Khi không đủ dung lượng để hoàn thành quá trình khôi phục.|Nếu không đủ dung lượng để hoàn thành quá trình khôi phục, điều này có thể gây ra tình trạng còn lặp lại và dẫn đến sự không ổn định.
Recovery Unfound|Khi dữ liệu cần khôi phục không tìm thấy.|Nếu dữ liệu cần khôi phục không tìm thấy, có thể dẫn đến mất mát dữ liệu và sự không nhất quán.
Backfilling|Khi Ceph đang sao chép dữ liệu từ một OSD khác để bổ sung vào OSD hiện tại.
Forced Backfill|Một giai đoạn của quá trình backfilling khi cần can thiệp thủ công.
Backfill Wait|Khi đợi cho quá trình backfilling hoàn thành.
Backfill Toofull|Khi không đủ dung lượng để hoàn thành quá trình backfilling.
Backfill Unfound|Khi dữ liệu cần backfill không tìm thấy.
Incomplete|Khi một quá trình nào đó không hoàn thành.
Stale|Khi dữ liệu đã lạc hậu so với trạng thái mới nhất.
Remapped|Khi dữ liệu đã được ánh xạ lại từ các OSD khác.
Undersized|Khi dung lượng của OSD không đủ để lưu trữ dữ liệu.
Peered|Khi các OSD đã kết nối và cập nhật dữ liệu cho nhau.
Snaptrim|Khi Ceph đang loại bỏ các phiên bản snapshot cũ.
Snaptrim Wait|Khi đợi cho quá trình snaptrim hoàn thành.
Snaptrim Error|Khi quá trình snaptrim gặp lỗi.|Lỗi trong quá trình loại bỏ phiên bản snapshot cũ có thể ảnh hưởng đến quản lý dung lượng và hiệu suất của hệ thống.
