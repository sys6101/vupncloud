# Triển khai Ceph Pacific bằng cephadm
## Mô hình 
![Alt text](/Picture/Storage/cephpacific.png)

### Cài đặt cephadm


    root@ceph01:~# curl --silent --remote-name --location https://github.com/ceph/ceph/raw/pacific/src/cephadm/cephadm
    root@ceph01:~# ls
    cephadm
    root@ceph01:~# chmod +x cephadm
    Add repo
    root@ceph01:~# ./cephadm add-repo --release pacific
    root@ceph01:~# ./cephadm install
    root@ceph01:~# which cephadm
    /usr/sbin/cephadm
### Tạo thư mục dữ liệu cho Ceph để ghi các file cấu hình
    # mkdir -p /etc/ceph

### Tạo một cluster mới
Bước đầu tiên để tạo một cụm Ceph cluster mới là chạy lệnh cephadm bootstrap trên máy chủ đầu tiên của cụm.

Command này sẽ tạo monitor daemon đầu tiên cho Ceph cluster.

    root@ceph1:~# cephadm bootstrap --mon-ip 192.168.54.63 --allow-fqdn-hostname

    Ceph Dashboard is now available at:

            URL: https://ceph01:8443/
            User: admin
        Password: f4aa9dyvp8 

    Enabling client.admin keyring and conf on hosts with "admin" label
    Enabling autotune for osd_memory_target
    You can access the Ceph CLI as following in case of multi-cluster or non-default config:

        sudo /sbin/cephadm shell --fsid da90ed10-2a0e-11ee-9c8c-fa163e98cb29 -c /etc/ceph/ceph.conf -k /etc/ceph/ceph.client.admin.keyring

    Or, if you are only running a single cluster on this host:

        sudo /sbin/cephadm shell 

    Please consider enabling telemetry to help improve Ceph:

        ceph telemetry on

    For more information see:

        https://docs.ceph.com/en/pacific/mgr/telemetry/

    Bootstrap complete.

### Thêm các host       
Phân phối khóa công khai ceph.pub đã được tạo bằng bootstrap

    # ssh-copy-id -f -i /etc/ceph/ceph.pub root@ceph02
    # ssh-copy-id -f -i /etc/ceph/ceph.pub root@ceph03
 

### Add các máy chủ vào cluster

    # ceph orch host add ceph02 192.168.54.64 --labels _admin
    Added host 'ceph02'
    # ceph orch host add ceph03 192.168.54.78 --labels _admin
    Added host 'ceph03' 

    [root@ceph01 ceph]# ceph orch host ls
    HOST    ADDR           LABELS  STATUS  
    ceph01  192.168.54.63  _admin          
    ceph02  192.168.54.64  _admin              
    ceph03  192.168.54.78  _admin               
    3 hosts in cluster       

### Add mon
    # ceph orch daemon add mon ceph02:192.168.54.63
    # ceph orch daemon add mon ceph02:192.168.54.78

    # ceph orch apply mon --placement="ceph1,ceph2,ceph3" --dry-run
    # ceph orch apply mon --placement="ceph1,ceph2,ceph3"
### Tạo OSD
Có nhiều cách để tạo OSD mới, apply này sẽ sử dụng tất cả các thiết bị lưu trữ có sẵn và chưa được sử dụng

    ceph orch apply osd --all-available-devices


    [root@ceph01 ceph]# ceph orch device ls
    HOST    PATH      TYPE  DEVICE ID              SIZE  AVAILABLE  REFRESHED  REJECT REASONS                                                      
    ceph01  /dev/vdb  hdd   fcf2547f-6fc6-4d68-9  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked       
    ceph01  /dev/vdc  hdd   472ebc66-63d7-49e6-8  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked       
    ceph02  /dev/vdb  hdd   34996f2d-c968-4ab5-8  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked       
    ceph02  /dev/vdc  hdd   57ebc100-ded6-4568-a  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked       
    ceph03  /dev/vdb  hdd   d3b30194-8989-4770-a  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked       
    ceph03  /dev/vdc  hdd   a6b4c8cd-57c7-497e-b  10.7G             2m ago     Insufficient space (<10 extents) on vgs, LVM detected, locked 


    # ceph -s
    cluster:
        id:     c7afeaa8-2a8f-11ee-ae23-fa163e98cb29
        health: HEALTH_OK
    
    services:
        mon: 3 daemons, quorum ceph01,ceph02,ceph03 (age 8m)
        mgr: ceph01.xfqiyv(active, since 22m), standbys: ceph02.dltjxq
        osd: 6 osds: 6 up (since 6m), 6 in (since 6m)
    
    data:
        pools:   1 pools, 1 pgs
        objects: 0 objects, 0 B
        usage:   31 MiB used, 60 GiB / 60 GiB avail
        pgs:     1 active+clean


Nếu xảy ra HEALTH_WARN MON_CLOCK_SKEW thì là do thời gian giữa các node chưa được đồng bộ, sử dụng:

    # apt install -y chrony




<!-- [root@ceph01 ceph]# ceph orch device ls
HOST    PATH      TYPE  DEVICE ID              SIZE  AVAILABLE  REFRESHED  REJECT REASONS  
ceph01  /dev/vdb  hdd   fcf2547f-6fc6-4d68-9  10.7G  Yes        2m ago                     
ceph01  /dev/vdc  hdd   472ebc66-63d7-49e6-8  10.7G  Yes        2m ago                     
ceph02  /dev/vdb  hdd   34996f2d-c968-4ab5-8  10.7G  Yes        11s ago                    
rm -rf /var/lib/ceph                             
rm -rf /etc/ceph  
wipefs -af /dev/vdb
wipefs -af /dev/vdc
