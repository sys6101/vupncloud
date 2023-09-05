## Tách osd qua 2 pool bằng cách move qua bucket mới
    ceph osd crush move {bucket-name} {bucket-type}={bucket-name}, [...]
    ceph osd crush move osd.0 root=ssd host=ceph01_ssd
    ceph osd crush move osd.0 root=ssd host=ceph02_ssd
    ceph osd crush move osd.0 root=ssd host=ceph03_ssd


**Kiểm tra tree**

    ceph osd tree
    ID   CLASS  WEIGHT   TYPE NAME            STATUS  REWEIGHT  PRI-AFF
    -14         0.02939  root ssd                                      
    -13         0.00980      host ceph03_ssd                           
     x5    hdd  0.00980          osd.5            up   1.00000  1.00000
    -19         0.00980      host ceph1_ssd                            
      1    hdd  0.00980          osd.1            up   1.00000  1.00000
    -17         0.00980      host ceph2_ssd                            
      3    hdd  0.00980          osd.3            up   1.00000  1.00000
     -1         0.02939  root default                                  
     -3         0.00980      host ceph01                               
      0    hdd  0.00980          osd.0            up   1.00000  1.00000
     -5         0.00980      host ceph02                               
      2    hdd  0.00980          osd.2            up   1.00000  1.00000
     -7         0.00980      host ceph03                               
      4    hdd  0.00980          osd.4            up   1.00000  1.00000


Thêm `osd crush update on start = false` vào ceph.conf để khi reboot không bị reset

**Taọ lần lượt 2 pool**     

        ceph osd pool create {pool-name} [{pg-num} [{pgp-num}]] [replicated] \
         [crush-rule-name] [expected-num-objects]
        ceph osd pool create pool1 100 100 3
        ceph osd pool create pool2 100 100 3

**Check pool**

    ceph osd pool ls detail
    pool 1 'device_health_metrics' replicated size 3 min_size 2 crush_rule 0 object_hash rjenkins pg_num 1 pgp_num 1 autoscale_mode on last_change 36 flags hashpspool stripe_width 0 pg_num_max 32 pg_num_min 1 application mgr_devicehealth
    pool 2 'pool1' replicated size 3 min_size 2 crush_rule 1 object_hash rjenkins pg_num 32 pgp_num 32 autoscale_mode on last_change 553 lfor 0/429/427 flags hashpspool,selfmanaged_snaps stripe_width 0
    pool 3 'pool2' replicated size 3 min_size 2 crush_rule 2 object_hash rjenkins pg_num 32 pgp_num 32 autoscale_mode on last_change 565 lfor 0/427/425 flags hashpspool,selfmanaged_snaps stripe_width 0

- replicated của pool là 3, cần ít nhất 2 replicated để hoạt động


**Tạo rule**        
Tạo hai rule Crush Map mới ("rulessd" và "rulehdd"), mỗi rule quy định cách dữ liệu sẽ được phân phối trên các thiết bị lưu trữ dựa trên mức độ phân tán cấp "host".
    ceph osd crush rule create-replicated <rule-name> <root> <failure-domain> <class>
    ceph osd crush rule create-replicated rulessd ssd host
    ceph osd crush rule create-replicated rulehdd default host

**Set rule  cho 2 pool ssd và hdd**

    ceph osd pool set pool1 crush_rule rulessd
    ceph osd pool set pool2 crush_rule rulehdd


Tạo image :

    rbd create --size {megabytes} {pool-name}/{image-name}
    rbd create --size 1024 pool1/image01
    rbd create --size 1024 pool2/image02
Tạo file config fio :

    $ vim p1.fio
    [global]
    ioengine=rbd
    clientname=admin
    pool=pool1
    rbdname=image01
    rw=randrw
    bs=4k
    [rbd_iodepth32]
    iodepth=32
                          

    $ vim p2.fio
    [global]
    ioengine=rbd
    clientname=admin
    pool=pool2
    rbdname=image02
    rw=randrw
    bs=4k
    [rbd_iodepth32]
    iodepth=32

Bắt đầu benchmark:

    fio p1.fio
- pool1 gồm osd.1 osd.3 osd.5 thuộc disk vdc   

![Alt text](/Picture/Storage/pool1.png)

    fio p2.fio
- pool2 gồm osd.0 osd.2 osd.4 thuộc disk vdc  

![Alt text](/Picture/Storage/pool2.png)