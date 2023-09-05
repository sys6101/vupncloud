# Ceph override


`noin`: Thường được sử dụng cùng với noout để giải quyết vấn đề với các OSD "flapping" (nghĩa là nhanh chóng thay đổi trạng thái).

`noout`: Nếu một OSD không báo cáo với monitor trong thời gian quy định, OSD này sẽ bị đánh dấu là out. Flag này ngăn chặn việc đánh dấu tự động.

`noup`: Thường được sử dụng cùng với nodown để giải quyết vấn đề OSD flapping.

`nodown`: Các vấn đề mạng có thể làm gián đoạn quá trình heartbeat của Ceph. flag này ngăn chặn việc đánh dấu một OSD là down một cách không chính xác.

`full`: Khi một cluster đạt ngưỡng dung lượng tối đa, bạn có thể đặt trạng thái của nó là full và mở rộng dung lượng.

`pause`: Ngăn chặn các hoạt động của client trên cluster.

`nobackfill`: Ngăn chặn việc backfill khi một OSD hoặc node tạm thời offline.

`norecover:` Khi thay thế một ổ đĩa OSD và bạn không muốn PGs phục hồi vào một OSD khác, flag này sẽ ngăn việc đó.

`noscrub` và `nodeep-scrub`: Ngăn chặn việc scrubbing trên OSDs.

`notieragent`: Dừng quá trình tìm các đối tượng lạnh để đẩy vào lớp lưu trữ dự phòng.
 
**Procedure**:

    ceph osd set FLAG

**Example**:

Set nodown và cho dừng 1 osd:

    ceph osd set nodown
    systemctl stop ceph-osd@0.service

Kết quả:

    ceph -s

    cluster:
        id:     f28d528e-fc01-4a14-9e7e-cda7ad185944
        health: HEALTH_WARN
                nodown flag(s) set

    services:
        mon: 3 daemons, quorum ceph01,ceph02,ceph03 (age 3h)
        mgr: ceph01(active, since 3d)
        osd: 6 osds: 6 up (since 5m), 6 in (since 3d)
            flags nodown

    data:
        pools:   3 pools, 65 pgs
        objects: 273 objects, 1.0 GiB
        usage:   4.0 GiB used, 56 GiB / 60 GiB avail
        pgs:     65 active+clean

1 osd dừng nhưng vẫn ở trạng thái up, giờ unset nodown thì sẽ có 1 osd down:

    cluster:
        id:     f28d528e-fc01-4a14-9e7e-cda7ad185944
        health: HEALTH_WARN
                nodown flag(s) set

    services:
        mon: 3 daemons, quorum ceph01,ceph02,ceph03 (age 3h)
        mgr: ceph01(active, since 3d)
        osd: 6 osds: 6 up (since 5m), 6 in (since 3d)
            flags nodown

    data:
        pools:   3 pools, 65 pgs
        objects: 273 objects, 1.0 GiB
        usage:   4.0 GiB used, 56 GiB / 60 GiB avail
        pgs:     65 active+clean

Đôi khi,  có thể cần thực hiện bảo trì trên một phần của cụm của mình hoặc khắc phục một vấn đề ảnh hưởng đến một lĩnh vực gặp sự cố (ví dụ: một rack). Nếu không muốn CRUSH tự động cân bằng lại cụm khi bạn tạm dừng OSD cho việc bảo trì, hãy đặt cụm thành trạng thái "noout" trước:

    ceph osd set noout

Trên các phiên bản Luminous hoặc mới hơn, việc đặt flag này chỉ trên các OSD bị ảnh hưởng sẽ an toàn hơn. Bạn có thể làm điều này từng cái một:

    ceph osd add-noout osd.0
    ceph osd rm-noout osd.0

Hoặc là toàn bộ một bucket CRUSH cùng một lúc. Giả sử bạn sẽ tạm dừng "prod-ceph-data1701" để thêm RAM:

    ceph osd set-group noout prod-ceph-data1701



Nếu có điều gì đó khiến OSD 'flap' (liên tục bị đánh dấu tắt rồi lại bật lên), bạn có thể buộc các máy chủ dừng sự động động này bằng cách tạm thời đóng băng trạng thái của chúng:


    ceph osd set noup      
    ceph osd set nodown    
Những flag này sẽ được ghi lại trong bản đồ osdmap:


    ceph osd dump | grep flags
    flags no-up,no-down
Bạn có thể xóa các flag này bằng cách:


    ceph osd unset noup
    ceph osd unset nodown

Hai flag khác được hỗ trợ, đó là noin và noout, chúng ngăn các OSD khởi động (được cấp phát dữ liệu) bị đánh dấu vào (allocated data) hoặc bảo vệ các OSD khỏi việc bị đánh dấu ra .

Node bị sập:    

    ceph osd set noout        
    ceph osd set nodown

Chủ động tắt một node (ví dụ: thay CPU):   

    ceph osd set noout        
    ceph osd set noup

OSD bắt đầu hiển thị dấu hiệu lỗi: 
     
    ceph osd set noout        

Nâng cấp phần mềm cho node hoặc OSD:   

    ceph osd set noout        
    ceph osd set nodown       
    ceph osd set noup     

Bảo dưỡng phần cứng:   
     
    ceph osd set noout        

Cần tái cân bằng dữ liệu nhưng không muốn nó xảy ra ngay lập tức:   

    ceph osd set norebalance

Đang triển khai OSD mới và không muốn chúng tham gia ngay:

    ceph osd set noup     

Khi bạn muốn ngăn chặn việc tái cân bằng dữ liệu do việc thay đổi crush map: 

    ceph osd set norebalance

Ngăn việc tạo các PG mới: 

    ceph osd set norecover

Ngăn việc phục hồi dữ liệu trong các PG:  

    ceph osd set norecover

Một số OSD được đánh dấu là down do vấn đề mạng tạm thời:  

    ceph osd set nodown       
Lý do: Để tránh việc Ceph tự động đánh dấu OSD là down khi gặp vấn đề mạng tạm thời.

Cần thực hiện thao tác backup mà không muốn ảnh hưởng đến hiệu suất: 

    ceph osd set norecover 

Lý do: Để tạm ngừng quá trình phục hồi, giúp tối ưu hiệu suất cho việc backup.

Triển khai một số OSD mới nhưng muốn tránh việc chúng nhận dữ liệu ngay lập tức:  

    ceph osd set noin     

Lý do: Để ngăn chặn việc OSD mới được triển khai nhận dữ liệu ngay sau khi chúng được thêm vào.     

Bảo trì một số OSD cụ thể mà không muốn chúng tái khởi động tự động: 

    ceph osd set noup       

Lý do: Để ngăn chặn việc OSD khởi động lại tự động sau khi được bảo trì.

Cần ngăn chặn việc tái cân bằng dữ liệu khi thay đổi trọng số của OSD trong crush map:  

    ceph osd set noreweight       

Lý do: Để tạm dừng việc thay đổi trọng số của OSD dẫn đến tái cân bằng dữ liệu.

Ngăn chặn việc xoá snapshots:  

    ceph osd set nosnaptrim  

Lý do: Để tạm dừng việc tự động xoá snapshots trong quá trình bảo trì hoặc nâng cấp.

Ngăn chặn việc viết dữ liệu vào cluster khi có nghi ngờ về sự cố: 

    ceph osd set noup     
    ceph osd set nodown   

Lý do: Để đảm bảo rằng không có thêm dữ liệu mới được ghi vào cluster khi đang kiểm tra hoặc khắc phục sự cố.      

Bảo trì toàn bộ cluster hoặc thực hiện nâng cấp lớn: 

    ceph osd set noout        
    ceph osd set noup     
    ceph osd set nodown   

Lý do: Để đảm bảo rằng dữ liệu không bị tái cân bằng hoặc bị ảnh hưởng khi thực hiện các thao tác bảo trì lớn trên toàn bộ cluster.