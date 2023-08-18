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

### DEGRADED

Việc Ghi Dữ Liệu: Khi một thiết bị client ghi một đối tượng vào máy chủ lưu trữ chính (primary OSD), primary OSD sẽ phải sao chép dữ liệu sang các thiết bị lưu trữ sao chép (replica OSDs). Sau khi primary OSD đã lưu trữ đối tượng, PG (placement group) sẽ ở trạng thái "DEGRADED" cho đến khi primary OSD nhận được thông báo xác nhận từ các replica OSDs rằng Ceph đã tạo các đối tượng sao chép thành công.

Nguyên Nhân Trạng Thái "DEGRADED": Một OSD có thể đang hoạt động mặc dù nó chưa nắm giữ tất cả các đối tượng. Nếu một OSD bị ngưng hoạt động, Ceph sẽ đánh dấu các PG gán cho OSD này là "DEGRADED". Khi OSD này trở lại hoạt động, các OSD sẽ cần kết nối lại với nhau (peer) để đồng bộ dữ liệu. Dù sao, ngay cả khi PG bị "DEGRADED", một thiết bị client vẫn có thể ghi dữ liệu mới vào PG này nếu nó đang hoạt động.

Trường Hợp OSD Ngừng Hoạt Động Và Xử Lý: Nếu một OSD bị ngưng hoạt động và tình trạng "DEGRADED" không được khắc phục, Ceph có thể đánh dấu OSD bị ngưng hoạt động là không còn trong cụm và sẽ chuyển dữ liệu từ OSD bị ngưng hoạt động sang OSD khác. Thời gian giữa việc đánh dấu OSD bị ngưng hoạt động và việc đánh dấu không còn trong cụm được điều khiển bởi tham số "mon osd down out interval", mặc định là 600 giây.

### RECOVERING
Ceph được thiết kế để đảm bảo tính chống lỗi ở một quy mô lớn, trong đó sự cố về phần cứng và phần mềm là thường xuyên. Khi một OSD ngưng hoạt động, nội dung của nó có thể chậm lại so với trạng thái hiện tại của các bản sao khác trong các nhóm đặt vị trí (placement groups). Khi OSD được khởi động lại, nội dung của các nhóm đặt vị trí cần được cập nhật để phản ánh trạng thái hiện tại. Trong khoảng thời gian đó, OSD có thể ở trạng thái khôi phục (recovering).

Quá trình khôi phục không luôn dễ dàng, vì sự cố phần cứng có thể gây ra sự cố tràn lan của nhiều OSD. Ví dụ, một switch mạng cho một rack hoặc tủ có thể hỏng, dẫn đến việc các OSD trên nhiều máy chủ có thể chậm lại so với trạng thái hiện tại của cụm. Mỗi OSD phải khôi phục sau khi sự cố được giải quyết.

Ceph cung cấp một số cài đặt để cân bằng việc sử dụng tài nguyên giữa các yêu cầu dịch vụ mới và việc khôi phục các đối tượng dữ liệu và phục hồi các nhóm đặt vị trí về trạng thái hiện tại. Cài đặt "osd recovery delay start" cho phép một OSD khởi động lại, tái kết nối và thậm chí xử lý một số yêu cầu phát lại trước khi bắt đầu quá trình khôi phục. Cài đặt "osd recovery thread timeout" đặt một thời gian chờ cho luồng, vì nhiều OSD có thể gặp sự cố, khởi động lại và tái kết nối ở tốc độ khác nhau. Cài đặt "osd recovery max active" giới hạn số lượng yêu cầu khôi phục mà một OSD sẽ xử lý đồng thời để tránh việc OSD không thể phục vụ. Cài đặt "osd recovery max chunk" giới hạn kích thước của các phần dữ liệu đã khôi phục để tránh quá tải mạng.
osd down


### BACKFILLING

"Backfilling" trong Ceph mô tả một trạng thái mà một OSD đang thực hiện việc tái cân bằng dữ liệu của nó với các OSD khác trong cụm. Khi một OSD mới được thêm vào cụm hoặc một OSD bị lỗi và được sửa chữa hoặc thay thế, dữ liệu cần được tái cân bằng để đảm bảo việc phân bổ dữ liệu tuân thủ theo cấu hình yêu cầu về sự độc lập và bản sao.

Khi một quá trình tái cân bằng diễn ra, một số OSDs có thể ở trạng thái "backfilling" khi chúng nhận dữ liệu mới hoặc gửi dữ liệu cho các OSD khác.

Đặc điểm của quá trình "backfilling":

Khôi phục dữ liệu: Đây là việc tái cân bằng dữ liệu từ OSD hoạt động sang OSD khác.

Hiệu quả: Ceph cố gắng thực hiện backfilling một cách hiệu quả nhất có thể, nhưng quá trình này có thể làm giảm hiệu suất tổng thể của cụm nếu nó diễn ra quá nhanh. Vì vậy, có các thiết lập cụ thể để kiểm soát tốc độ của quá trình này.

Trạng thái "backfilling" là một phần bình thường của hoạt động của Ceph và giúp đảm bảo dữ liệu được lưu trữ một cách an toàn và hiệu quả trên toàn bộ cụm

### REMAPPED
"Remapped" liên quan đến việc tái ánh xạ các đối tượng hoặc PGs (Placement Groups) từ một OSD (Object Storage Daemon) sang một OSD khác. Điều này thường diễn ra trong các tình huống như:

OSD Failure: Khi một OSD bị lỗi hoặc không khả dụng, Ceph sẽ cố gắng tái ánh xạ dữ liệu của OSD đó đến các OSD khác trong cụm để duy trì tính sẵn sàng và độ tin cậy của dữ liệu.

Tái cân bằng dữ liệu: Khi thêm hoặc loại bỏ OSDs khỏi cụm, Ceph sẽ tái cân bằng dữ liệu. Trong quá trình này, PGs sẽ được "remapped" đến các OSD mới.

Cấu hình CRUSH Map thay đổi: Ceph sử dụng thuật toán CRUSH để xác định nơi đặt dữ liệu. Mỗi khi CRUSH map được cập nhật (ví dụ: thay đổi trọng số của OSD hoặc thay đổi cấu trúc cây), một số PGs có thể cần phải được remapped.


Lưu ý rằng việc remapped là một phần tự nhiên và thường xuyên của hoạt động của Ceph, nhưng nó cũng có thể tăng tải trên cụm và giảm hiệu suất. Vì vậy, việc giám sát và quản lý tốc độ tái ánh xạ có thể giúp tối ưu hóa hiệu suất và độ tin cậy của cụm.
### STALE
"Stale" thường liên quan đến Placement Groups (PGs). Một PG ở trạng thái "stale" thường chỉ ra rằng Ceph Monitor (cơ sở dữ liệu quản lý và điều phối trung tâm của Ceph) không nhận được cập nhật từ PG đó trong một khoảng thời gian dài.

Đây là một số điểm quan trọng về trạng thái "stale":

Không có thông tin mới: Khi một PG ở trạng thái "stale", điều đó có nghĩa là Monitors không nhận được thông tin hoạt động mới từ PG đó.

Không nhất thiết là lỗi: Một PG có thể trở nên "stale" mà không gây ra bất kỳ sự cố hoặc lỗi nào trong hệ thống. Tuy nhiên, việc này có thể là một dấu hiệu cho thấy có sự cố mạng hoặc vấn đề với OSD chứa PG.

Đòi hỏi giám sát: Nếu một PG ở trạng thái "stale" trong một khoảng thời gian dài, nên kiểm tra cụm Ceph để đảm bảo rằng mọi thứ đang hoạt động bình thường. Việc này có thể liên quan đến việc kiểm tra tình trạng mạng, trạng thái OSD, và các thông tin khác.

### INCONSISTENCY

Khi nói đến "inconsistency", chúng ta thường đề cập đến sự không nhất quán trong dữ liệu giữa các bản sao của một Placement Group (PG) hoặc một đối tượng cụ thể. Ceph cố gắng duy trì dữ liệu nhất quán giữa các bản sao, nhưng trong một số trường hợp do lỗi phần cứng, lỗi phần mềm hoặc vấn đề mạng, dữ liệu có thể trở nên không nhất quán.

Dưới đây là một số điểm quan trọng về "inconsistency" trong Ceph:

Cảnh báo: Khi Ceph phát hiện ra sự không nhất quán trong dữ liệu, nó sẽ thường báo cáo thông qua các lệnh trạng thái như ceph health detail hoặc ceph pg dump_stuck.

Nguyên nhân: Có nhiều nguyên nhân có thể dẫn đến sự không nhất quán. Điều này có thể bao gồm việc một OSD bị hỏng và sau đó trở lại trạng thái hoạt động mà không cần tái đồng bộ, lỗi ổ đĩa hoặc lỗi tại mức hệ điều hành.

Khắc phục: Ceph cung cấp một công cụ gọi là ceph-objectstore-tool giúp khắc phục các vấn đề không nhất quán. Với công cụ này, bạn có thể sửa chữa đối tượng không nhất quán hoặc tái đồng bộ PGs.


![Alt text](/Picture/Storage/state1.png)
![Alt text](/Picture/Storage/state2.png)
![Alt text](/Picture/Storage/state3.png)
![Alt text](/Picture/Storage/state4.png)