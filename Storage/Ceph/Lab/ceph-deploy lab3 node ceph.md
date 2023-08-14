# Triển khai Ceph Pacific bằng ceph-deploy
## Mô hình 
![Alt text](/Picture/Storage/cephpacific.png)

Mô hình IP      
|  Host name | Public IP | Cluster IP | Disk |
|  --------- | --------- | ---------- | ---- |
|  ceph01  | 192.168.54.63 | 192.168.54.63 | 1 x 20GB (vda), 2 x 10GB(vdb, vdc) |
|  ceph02  | 192.168.54.64 | 192.168.54.64 | 1 x 20GB (vda), 2 x 10GB(vdb, vdc) |
|  ceph03  | 192.168.54.78 | 192.168.54.78 | 1 x 20GB (vda), 2 x 10GB(vdb, vdc) |

## Triển khai trên 3 node
    cat << EOF > /etc/hosts
    127.0.0.1 `hostname` localhost
    192.168.54.63 ceph01
    192.168.54.64 ceph02
    192.168.54.78 ceph03
    EOF

Đồng bộ thời gian cho 3 node:

    # yum install -y chrony    

Tạo user cephuser và nhập mật khẩu

    # sudo useradd -d /home/cephuser -m cephuser
    # sudo passwd cephuser    

Cấp quyền sudo cho cephuser

    # echo "cephuser ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/cephuser
    # sudo chmod 0440 /etc/sudoers.d/cephuser    

Khai báo repo của Ceph Pacific

    # wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
    OK
    # echo deb https://download.ceph.com/debian-pacific/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
    # yum update

## Sử dụng ceph-deploy trên ceph01
Đứng trên ceph01 (ceph-admin) để cài đặt ceph-deploy và thao tác với ceph02, ceph03 từ xa.

    # yum install -y ceph-deploy
Từ đây sẽ chỉ làm việc bằng user cephuser

    #  su - cephuser
Tạo private key và public key cho user cephuser và copy sang các node còn lại

    $ ssh-keygen
    $ ssh-copy-id cephuser@ceph01
    $ ssh-copy-id cephuser@ceph02
    $ ssh-copy-id cephuser@ceph03

Khởi tạo các node ceph trong cluser.

    $ ceph-deploy new ceph01 ceph02 ceph03 
Sau khi khởi tạo, các file cấu hình sẽ được tạo ra trong thư mục hiện tại

    $ ls -alh
    total 24K
    drwxrwxr-x 2 cephuser cephuser 4.0K Jul 21 06:57 .
    drwxr-xr-x 6 cephuser cephuser 4.0K Jul 21 06:57 ..
    -rw-rw-r-- 1 cephuser cephuser  238 Jul 21 06:57 ceph.conf
    -rw-rw-r-- 1 cephuser cephuser 5.1K Jul 21 06:57 ceph-deploy-ceph.log
    -rw------- 1 cephuser cephuser   73 Jul 21 06:57 ceph.mon.keyring

Trong đó:

- ceph.conf là file config được tự động khởi tạo
- ceph-deploy-ceph.log là file log của toàn bộ thao tác đối với việc sử dụng lệnh ceph-deploy
- ceph.mon.keyring là key monitoring được ceph sinh ra tự động để khởi tạo Cluster

Cấu hình file ceph.conf trước khi thực hiện cài đặt các gói cần thiết cho ceph trên các node       '

    cat << EOF >> ceph.conf
    public network = 192.168.54.0/24
    cluster network = 192.168.54.0/24
    osd objectstore = bluestore
    mon_allow_pool_delete = true
    osd pool default size = 3
    osd pool default min size = 1 
    EOF



Các thành phần
- public network: Đường trao đổi thông tin giữa các node Ceph và cũng là đường client kết nối vào
- cluster network: Đường đồng bộ dữ liệu    
- public network và cluster network: Chỉ định địa chỉ mạng mà Ceph sử dụng cho lưu lượng công cộng và cụm tương ứng. Trong ví dụ này, đều dùng 192.168.54.0/24
- Note: Các IP file /etc/hosts phải thuộc subnet của public network 
- osd objectstore: Chỉ định backend lưu trữ đối tượng mà Ceph sẽ sử dụng. Trong ví dụ này, nó được thiết lập thành bluestore, đây là backend được khuyến nghị cho Ceph.
- mon_allow_pool_delete: Thiết lập cho phép xóa các pool khỏi cụm Ceph.
- osd pool default size: Chỉ định số OSD (Object Storage Daemons) mà mỗi nhóm đặt chỗ nên được sao chép đến. Trong ví dụ này, kích thước mặc định được thiết lập để là 3.
- osd pool default min size: Chỉ định số tối thiểu của OSD mà mỗi nhóm đặt chỗ nên được sao chép đến. Trong ví dụ này, kích thước tối thiểu mặc định được thiết lập để là 1.

Tiến hình cài đặt Ceph Pacific trên các node

    $ ceph-deploy install --release pacific ceph01 ceph02 ceph03
Khi cài đặt thành công thì sẽ có thể kiểm tra version của Ceph trên cả 3 node

    $ ceph -v
    ceph version 16.2.13 (5378749ba6be3a0868b51803968ee9cde4833a3e) pacific (stable)

## Thành phần Monitor
### Thiết lập thành phần MON cho cả 3 node

    $ ceph-deploy mon create-initial

Khi thực hiện thành công, các 4 file keyring sẽ được thêm vào thư mục hiện tại my-cluster, có thể kiểm tra bằng ls -l

    ceph.bootstrap-mds.keyring
    ceph.bootstrap-mgr.keyring
    ceph.bootstrap-osd.keyring
    ceph.bootstrap-rgw.keyring

Thực hiện copy file ceph.client.admin.keyring sang các node trong cụm Ceph cluster. File này sẽ được copy vào thư mục /etc/ceph/ trên các node.

    $ ceph-deploy admin ceph01 ceph02 ceph03
Đứng trên node ceph01 phân quyền cho file /etc/ceph/ceph.client.admin.keyring trên cả 03 node.

    $ ssh cephuser@ceph01 'sudo chmod +r /etc/ceph/ceph.client.admin.keyring'
    $ ssh cephuser@ceph02 'sudo chmod +r /etc/ceph/ceph.client.admin.keyring'
    $ ssh cephuser@ceph03 'sudo chmod +r /etc/ceph/ceph.client.admin.keyring'

Đứng trên node ceph01 và thực hiện khai báo các OSD disk. Bước này sẽ thực hiện format các disk trên cả 3 node và join chúng vào làm các OSD (Thành phần chứa dữ liệu của CEPH).

    $ ceph-deploy osd create --data /dev/vdb ceph01
    $ ceph-deploy osd create --data /dev/vdc ceph01

    $ ceph-deploy osd create --data /dev/vdb ceph02
    $ ceph-deploy osd create --data /dev/vdc ceph02

    $ ceph-deploy osd create --data /dev/vdb ceph03
    $ ceph-deploy osd create --data /dev/vdc ceph03

Có thể kiểm tra kết quả format này trên cả 3 node

    $ lsblk
    NAME                                                                  MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
    vda                                                                   252:0    0  20G  0 disk          
    └─vda1                                                                252:1    0  20G  0 part /        
    vdb                                                                   252:16   0  10G  0 disk          
    └─ceph--1cfeda62--dfc7--4a0a--9f7d--3c0637477ea3-osd--block--96b1e619--5608--459c--9ef2--596da32f02d4  
                                                                        253:0    0  10G  0 lvm           
    vdc                                                                   252:32   0  10G  0 disk          
    └─ceph--5f72a565--6ffb--4f1f--87a1--47f1ad99d490-osd--block--bed2c96d--4ea2--4af9--a791--0d43162bafd5  
                                                                        253:1    0  10G  0 lvm           

Kiểm tra status của Ceph cluster lúc này thì có thể thấy trạng thái HEALTH_WARN, lý do bởi thành phần ceph-mgr vẫn chưa được khởi tạo.  

    $ ceph -s
    cluster:
        id:     23ffd00c-86c9-4632-ba17-6b7f475f0efc
        health: HEALTH_WARN
                mons are allowing insecure global_id reclaim                                               
                1 daemons have recently crashed

        services:
        mon: 3 daemons, quorum ceph01,ceph02,ceph03 (age 98m)                                              
        mgr: ceph01(active, since 102m)
        osd: 6 osds: 6 up (since 107m), 6 in (since 108m)                                                  

        data:
        pools:   1 pools, 1 pgs
        objects: 0 objects, 0 B
        usage:   32 MiB used, 60 GiB / 60 GiB avail
        pgs:     1 active+clean

Kích hoạt ceph-mgr và ceph-dashboard trên node ceph01

    $ ceph-deploy mgr create ceph01
    $ ceph mgr module enable dashboard --force
    $ ceph mgr module ls 


Tạo SSL cert cho ceph-dashboard (tuy nhiên đây chỉ là cert do ceph-dashboard tự tạo ra và trình duyệt có thể sẽ không tin tưởng CA này)

    $ sudo ceph dashboard create-self-signed-cert 
    Self-signed certificate created 
Tạo tài khoản cho ceph-dashboard, username vuadmin và password admin0712

    $ touch pass.secret && echo "admin0712" >> pass.secret
    $ ceph dashboard ac-user-create vuadmin  administrator -i pass.secret
    {"username": "vuadmin", "password": "$2b$12$FTJ734pEnACY3dLhriZ2YO1RxU5VBgPWMGJwe79sVVZTOtp2O515e", "roles": ["administrator"], "name": null, "email": null, "lastUpdate": 1689924401, "enabled": true, "pwdExpirationDate": null, "pwdUpdateRequired": false}

Kiểm tra xem ceph-dashboard đã được cài đặt thành công hay chưa

    $ ceph mgr services 
    {
        "dashboard": "https://192.168.54.63:8443/"
    }
    
Trong quá trình làm có WARN:        
Lỗi 1:

    1 daemons have recently crashed

FIX:
    ceph crash ls

    If you want to read the message:

    ceph crash info <id>

    then:

    ceph crash archive <id>

    or:

    ceph crash archive-all

Lỗi 2:

    AUTH_INSECURE_GLOBAL_ID_RECLAIM_ALLOWED: mons are allowing insecure global_id reclaim

FIX:

    ceph config set mon auth_allow_insecure_global_id_reclaim false