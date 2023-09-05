# LVM
LVM tags dùng để xác định các volume logic được sử dụng bởi ceph osd.       
Các tags hay dùng :
- ceph.type: Mô tả xem thiết bị là OSD hay Journal, với khả năng mở rộng sang các loại khác khi được hỗ trợ (ví dụ: lockbox). Example: `ceph.type=osd`
  
- ceph.cluster_fsid: cho biết cluster ID của Ceph cluster.
- ceph.osd_id: cho biết ID của OSD mà volume liên kết.
- ceph.data_device: cho biết đường dẫn của volume logic chứa data cho OSD.
- ceph.data_uuid: cho biết UUID của volume logic chứa data cho OSD.
- ceph.journal_device: cho biết đường dẫn của volume logic chứa journal cho OSD
- ceph.journal_uuid: cho biết UUID của volume logic chứa journal cho OSD.

Các LVM tags được Ceph sử dụng để khám phá OSD và để quản lý dữ liệu được lưu trữ trên chúng. Khi một ổ đĩa logic mới được tạo, các thẻ LVM có thể được đặt bằng cách sử dụng lệnh `ceph-volume lvm prepare`. Lệnh `ceph-volume lvm list` có thể được sử dụng để liệt kê các ổ logic đã được gắn thẻ để sử dụng bởi Ceph OSD.

Ví dụ:

    [root@ceph01 ~]# ceph-volume lvm list


    ====== osd.2 =======

    [block]       /dev/ceph-114267ff-81c8-4868-a03c-c74a522da9d7/osd-block-14796d72-c0fa-4eb6-843b-4f2a15cc77f0

        block device              /dev/ceph-114267ff-81c8-4868-a03c-c74a522da9d7/osd-block-14796d72-c0fa-4eb6-843b-4f2a15cc77f0
        block uuid                gDxkcF-DevV-SW7X-uJ6c-6DjW-A0Ia-858R4q
        cephx lockbox secret      
        cluster fsid              47032e56-30e8-11ee-bb43-fa163e98cb29
        cluster name              ceph
        crush device class        
        encrypted                 0
        osd fsid                  14796d72-c0fa-4eb6-843b-4f2a15cc77f0
        osd id                    2
        osdspec affinity          all-available-devices
        type                      block
        vdo                       0
        devices                   /dev/vdb

    ====== osd.5 =======

    [block]       /dev/ceph-c2553bbf-fbec-4527-82a0-883174c76c9a/osd-block-6773c44f-0886-4314-ba42-bab3132c46b3

        block device              /dev/ceph-c2553bbf-fbec-4527-82a0-883174c76c9a/osd-block-6773c44f-0886-4314-ba42-bab3132c46b3
        block uuid                siKD4G-pTBn-Szf1-Da2j-Bj8r-ZHnR-b4Zn3b
        cephx lockbox secret      
        cluster fsid              47032e56-30e8-11ee-bb43-fa163e98cb29
        cluster name              ceph
        crush device class        
        encrypted                 0
        osd fsid                  6773c44f-0886-4314-ba42-bab3132c46b3
        osd id                    5
        osdspec affinity          all-available-devices
        type                      block
        vdo                       0
        devices                   /dev/vdc

