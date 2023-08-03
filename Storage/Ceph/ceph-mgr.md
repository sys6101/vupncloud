# Ceph Manager
Ceph Manager là một thành phần quan trọng trong hệ thống lưu trữ phân tán Ceph, cung cấp các tính năng quản lý và giám sát toàn diện cho cluster Ceph. Các tính năng quan trọng của Ceph Manager bao gồm:

-  Quản lý cluster: Ceph Manager cung cấp các tính năng quản lý toàn diện cho cluster Ceph, bao gồm các nhiệm vụ như thêm hoặc xóa OSD, tạo và xóa pool, quản lý OSD và MDS, và nhiều hơn nữa.

-  Giám sát cluster: Ceph Manager cung cấp các tính năng giám sát toàn diện cho cluster Ceph, bao gồm thông tin về tình trạng OSD, MDS, RADOS Gateway, và các dịch vụ khác trong cluster. Nó cũng cung cấp các công cụ giám sát hiệu suất và sức khỏe của cluster, giúp người dùng có thể theo dõi hoạt động của cluster và phát hiện các vấn đề có thể xảy ra.

-  Cung cấp REST API: Ceph Manager cung cấp REST API để cho phép người dùng tương tác với cluster Ceph thông qua giao diện web hoặc các ứng dụng khác. REST API cung cấp các tính năng như tạo và xóa pool, quản lý OSD và MDS, và cung cấp thông tin về tình trạng của cluster.

-  Cập nhật tự động: Ceph Manager có khả năng tự động cập nhật các thành phần trong cluster Ceph, giúp người quản trị tiết kiệm thời gian và nỗ lực trong việc cập nhật cluster.

Ceph Manager có thể triển khai trên nhiều node khác nhau trong cluster, giúp đảm bảo tính sẵn sàng cao cho quản lý và giám sát hệ thống. Nó cũng có khả năng mở rộng linh hoạt để xử lý hàng nghìn OSD và các nút trong cluster lớn.

Theo mặc định, ceph-mgr không yêu cầu cấu hình bổ sung nhưng cần đảm bảo service mgr phải được hoạt động. Nếu không có service mgr nào đang chạy, sẽ có cảnh báo về HEALTH cho cụm Ceph.

Ceph-mgr cung cấp các module bổ sung như Dashboard, ResfullAPI, Zabbix, Prometheus chủ yếu tập trung vào việc thu thập số liệu trên toàn cluster, cũng như hỗ trợ thao tác với cụm Ceph trên Dashboard (từ bản Nautilus 14.2.x)

Ceph-mgr thường được triển khai trên một node MON dựa vào các công cụ deploy như cephadm, ceph-ansible, ceph-deploy,...

Sử dụng các module
Sử dụng lệnh ceph mgr module ls để xem module nào khả dụng và module nào hiện đang được bật. Bật hoặc tắt chúng bằng các câu lệnh ceph mgr enable <module> và ceph mgr disable <module>.

Nếu một module được bật thì active ceph-mgr daemon sẽ tải và thực thi nó. Trong trường hợp module cung cấp dịch vụ, chẳng hạn như máy chủ HTTP, module có thể cung cấp địa chỉ khi được bật lên. Để xem địa chỉ của các module ceph mgr services

Ví dụ

    [root@ceph01 ceph]# ceph mgr module ls
    {
        "always_on_modules": [
            "balancer",
            "crash",
            "devicehealth",
            "orchestrator",
            "pg_autoscaler",
            "progress",
            "rbd_support",
            "status",
            "telemetry",
            "volumes"
        ],
        "enabled_modules": [
            "cephadm",
            "dashboard",
            "iostat",
            "nfs",
            "prometheus",
            "restful"
        ],
        "disabled_modules": [..................]

    [root@ceph01 ceph]# ceph mgr services
    {
        "dashboard": "https://192.168.54.63:8443/",
        "prometheus": "http://192.168.54.63:9283/"
    }


