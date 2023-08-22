# Preparing Ceph OSDs using ceph-volume

## Prepair and Active

Quá trình "osd prepare" (hoặc OSD Prepare) trong Ceph liên quan đến việc chuẩn bị một ổ đĩa hoặc thiết bị lưu trữ cụ thể để sử dụng làm OSD (Object Storage Daemon). OSDs là thành phần quan trọng trong hệ thống Ceph, chịu trách nhiệm lưu trữ dữ liệu đối tượng và thực hiện các hoạt động như đọc, ghi và tái phân phối dữ liệu.

Khi bạn thực hiện quá trình "osd prepare," bạn đang thông báo cho hệ thống Ceph rằng bạn muốn sử dụng một ổ đĩa hoặc thiết bị cụ thể để tạo một OSD. Điều này đòi hỏi hệ thống kiểm tra và chuẩn bị ổ đĩa để có thể được sử dụng làm một phần của cụm Ceph.

Tạo và cấu hình OSD:

    ceph-volume lvm create --bluestore --data /dev/sdX

Thay thế --bluestore bằng --filestore nếu bạn muốn sử dụng Filestore thay vì Bluestore.         
Khi ổ đĩa đã được phân vùng và định dạng, hệ thống Ceph sẽ tạo một OSD trên ổ đĩa đó. OSD này sẽ được cấu hình với các thông số như OSD ID, pool mặc định và các thiết lập khác liên quan đến hiệu suất và bảo mật.

Kích hoạt OSD:

    ceph-volume lvm activate --bluestore <OSD id> <OSD uuid>
    hoặc 
    ceph-volume lvm activate --all

Cuối cùng, OSD mới được kích hoạt và tham gia vào hoạt động của cụm Ceph. Nó sẽ thực hiện các nhiệm vụ như lưu trữ dữ liệu, đồng bộ hóa và phân phối dữ liệu trên cụm.


Hiển thị thông tin các OSD:

    ceph osd tree

Lệnh này sẽ hiển thị cây OSD trong cụm Ceph, bao gồm thông tin về OSD ID và tình trạng.

## Bluestore & Filestore
Trong hệ thống lưu trữ phân tán Ceph, Bluestore và Filestore là hai loại cơ sở dữ liệu khác nhau được sử dụng để lưu trữ dữ liệu của các OSDs (Object Storage Daemons). Dưới đây là sự phân biệt giữa Bluestore và Filestore trong Ceph:

**Bluestore**

Bluestore là một cơ sở dữ liệu lưu trữ dựa trên block, được phát triển bởi Ceph nhằm cải thiện hiệu suất và quản lý tài nguyên hơn so với Filestore. Đây là một cơ sở dữ liệu tối ưu hóa để sử dụng trên các ổ đĩa SSD hoặc NVMe, với mục tiêu cải thiện hiệu suất đọc và ghi cũng như giảm thiểu tỷ lệ bỏ gói (garbage collection).

Ưu điểm của Bluestore:

Hiệu suất cao hơn so với Filestore đặc biệt trên các ổ đĩa nhanh như SSDs.
Tiết kiệm không gian đĩa do hạn chế bỏ gói dữ liệu.
Khả năng tương thích tốt hơn với các ổ đĩa hiện đại.

**Filestore**

Filestore là cơ sở dữ liệu lưu trữ dựa trên tệp, hoạt động bằng cách lưu trữ các đối tượng dữ liệu dưới dạng tệp tin. Filestore thường được sử dụng trên các phiên bản Ceph cũ hơn và không thể hiện hiệu suất tốt như Bluestore trên các ổ đĩa nhanh.

Ưu điểm của Filestore:

Phù hợp cho việc sử dụng các ổ đĩa cơ học (HDDs) và trong các phiên bản Ceph cũ hơn.
Đơn giản hóa việc nâng cấp từ phiên bản Ceph cũ lên phiên bản mới hơn.
Lựa chọn giữa Bluestore và Filestore phụ thuộc vào mục tiêu sử dụng và cấu hình phần cứng của hệ thống Ceph của bạn. Nếu bạn đang triển khai một hệ thống mới hoặc muốn tối ưu hiệu suất, Bluestore có thể là lựa chọn tốt hơn. Tuy nhiên, nếu bạn đang nâng cấp từ phiên bản cũ hơn hoặc sử dụng ổ đĩa cơ học, Filestore vẫn có thể là một lựa chọn hợp lý.


## Example

**Các bước tạo OSD**

Trước khi tạo OSD (Object Storage Daemon) trên Ceph, quá trình "zap" được thực hiện để xóa hoặc xoá sạch các thông tin, dấu vết, cấu trúc dữ liệu cũ trên ổ đĩa hoặc thiết bị mà bạn định sử dụng cho OSD. Quá trình này là quan trọng để đảm bảo rằng không còn dữ liệu hoặc thông tin cũ nào ảnh hưởng đến hoạt động của OSD mới.

    $ ceph-volume lvm zap /dev/vdb --destroy

Tạo và cấu hình OSD:

    $ ceph-volume lvm prepare --bluestore --data  /dev/vdb

    Running command: /usr/bin/ceph-authtool --gen-print-key
    Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 7eabb31a-0d05-4b7f-9170-245d24621e52
    Running command: vgcreate --force --yes ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2 /dev/vdb
    stdout: Physical volume "/dev/vdb" successfully created.
    stdout: Volume group "ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2" successfully created
    Running command: lvcreate --yes -l 2559 -n osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2
    stdout: Logical volume "osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52" created.
    Running command: /usr/bin/ceph-authtool --gen-print-key
    Running command: /usr/bin/mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-0
    Running command: /usr/sbin/restorecon /var/lib/ceph/osd/ceph-0
    Running command: /usr/bin/chown -h ceph:ceph /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52
    Running command: /usr/bin/chown -R ceph:ceph /dev/dm-1
    Running command: /usr/bin/ln -s /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 /var/lib/ceph/osd/ceph-0/block
    Running command: /usr/bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-0/activate.monmap
    stderr: 2023-08-22T10:14:24.262+0000 7fab5ef05700 -1 auth: unable to find a keyring on /etc/ceph/ceph.client.bootstrap-osd.keyring,/etc/ceph/ceph.keyring,/etc/ceph/keyring,/etc/ceph/keyring.bin,: (2) No such file or directory
    2023-08-22T10:14:24.262+0000 7fab5ef05700 -1 AuthRegistry(0x7fab5805c198) no keyring found at /etc/ceph/ceph.client.bootstrap-osd.keyring,/etc/ceph/ceph.keyring,/etc/ceph/keyring,/etc/ceph/keyring.bin,, disabling cephx
    stderr: got monmap epoch 1
    --> Creating keyring file for osd.0
    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/keyring
    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/
    Running command: /usr/bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 0 --monmap /var/lib/ceph/osd/ceph-0/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-0/ --osd-uuid 7eabb31a-0d05-4b7f-9170-245d24621e52 --setuser ceph --setgroup ceph
    stderr: 2023-08-22T10:14:24.922+0000 7f0abfaa5200 -1 bluestore(/var/lib/ceph/osd/ceph-0/) _read_fsid unparsable uuid
    --> ceph-volume lvm prepare successful for: /dev/vdb

Thử kiếm tra xem:

    $ ceph osd df tree

    ID   CLASS  WEIGHT   REWEIGHT  SIZE    RAW USE  DATA     OMAP    META     AVAIL    %USE   VAR   PGS  STATUS  TYPE NAME          
    -14         0.02939         -  30 GiB  2.2 GiB   23 MiB  20 KiB  2.2 GiB   28 GiB   7.27  1.08    -          root ssd           
    -9         0.00980         -  10 GiB  1.1 GiB  7.7 MiB   8 KiB  1.1 GiB  8.9 GiB  10.60  1.57    -              host ceph01_ssd
    1    hdd  0.00980   1.00000  10 GiB  1.1 GiB  7.7 MiB   8 KiB  1.1 GiB  8.9 GiB  10.60  1.57   32      up          osd.1      
    -10         0.00980         -  10 GiB  165 MiB  7.7 MiB   6 KiB  157 MiB  9.8 GiB   1.61  0.24    -              host ceph02_ssd
    3    hdd  0.00980   1.00000  10 GiB  165 MiB  7.7 MiB   6 KiB  157 MiB  9.8 GiB   1.61  0.24   32      up          osd.3      
    -11         0.00980         -  10 GiB  984 MiB  7.7 MiB   6 KiB  977 MiB  9.0 GiB   9.62  1.42    -              host ceph03_ssd
    5    hdd  0.00980   1.00000  10 GiB  984 MiB  7.7 MiB   6 KiB  977 MiB  9.0 GiB   9.62  1.42   32      up          osd.5      
    -1         0.01959         -  20 GiB  1.2 GiB  1.0 GiB  12 KiB  186 MiB   19 GiB   5.98  0.88    -          root default       
    -3               0         -     0 B      0 B      0 B     0 B      0 B      0 B      0     0    -              host ceph01    
    -5         0.00980         -  10 GiB  612 MiB  519 MiB   5 KiB   92 MiB  9.4 GiB   5.97  0.88    -              host ceph02    
    2    hdd  0.00980   1.00000  10 GiB  612 MiB  519 MiB   5 KiB   92 MiB  9.4 GiB   5.97  0.88   33      up          osd.2      
    -7         0.00980         -  10 GiB  613 MiB  519 MiB   7 KiB   93 MiB  9.4 GiB   5.98  0.89    -              host ceph03    
    4    hdd  0.00980   1.00000  10 GiB  613 MiB  519 MiB   7 KiB   93 MiB  9.4 GiB   5.98  0.89   33      up          osd.4      
    0               0   1.00000     0 B      0 B      0 B     0 B      0 B      0 B      0     0    0    down  osd.0 

Ta thấy OSD được tạo nhưng đang down, vì thế cần activate nó:

    $ ceph-volume lvm activate --bluestore 0 7eabb31a-0d05-4b7f-9170-245d24621e52

    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    Running command: /usr/bin/ceph-bluestore-tool --cluster=ceph prime-osd-dir --dev /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 --path /var/lib/ceph/osd/ceph-0 --no-mon-config
    Running command: /usr/bin/ln -snf /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 /var/lib/ceph/osd/ceph-0/block
    Running command: /usr/bin/chown -h ceph:ceph /var/lib/ceph/osd/ceph-0/block
    Running command: /usr/bin/chown -R ceph:ceph /dev/dm-1
    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    Running command: /usr/bin/systemctl enable ceph-volume@lvm-0-7eabb31a-0d05-4b7f-9170-245d24621e52
    stderr: Created symlink /etc/systemd/system/multi-user.target.wants/ceph-volume@lvm-0-7eabb31a-0d05-4b7f-9170-245d24621e52.service → /usr/lib/systemd/system/ceph-volume@.service.
    Running command: /usr/bin/systemctl enable --runtime ceph-osd@0
    Running command: /usr/bin/systemctl start ceph-osd@0
    --> ceph-volume lvm activate successful for osd ID: 0

Có thể lấy  OSD id và OSD uuid bằng command `ceph-volume lvm list`

Bây giờ OSD đã hoạt động 

    $ ceph-volume lvm activate --bluestore 0 7eabb31a-0d05-4b7f-9170-245d24621e52

    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    Running command: /usr/bin/ceph-bluestore-tool --cluster=ceph prime-osd-dir --dev /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 --path /var/lib/ceph/osd/ceph-0 --no-mon-config
    Running command: /usr/bin/ln -snf /dev/ceph-b12219e8-90a5-4ec1-aa95-b19fa7bcc4f2/osd-block-7eabb31a-0d05-4b7f-9170-245d24621e52 /var/lib/ceph/osd/ceph-0/block
    Running command: /usr/bin/chown -h ceph:ceph /var/lib/ceph/osd/ceph-0/block
    Running command: /usr/bin/chown -R ceph:ceph /dev/dm-1
    Running command: /usr/bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    Running command: /usr/bin/systemctl enable ceph-volume@lvm-0-7eabb31a-0d05-4b7f-9170-245d24621e52
    stderr: Created symlink /etc/systemd/system/multi-user.target.wants/ceph-volume@lvm-0-7eabb31a-0d05-4b7f-9170-245d24621e52.service → /usr/lib/systemd/system/ceph-volume@.service.
    Running command: /usr/bin/systemctl enable --runtime ceph-osd@0
    Running command: /usr/bin/systemctl start ceph-osd@0
    --> ceph-volume lvm activate successful for osd ID: 0

Đưa về bucket mong muốn và set weight:

    $ ceph osd crush move osd.0 root=default host=ceph01                                                                                                                                               
    moved item id 0 name 'osd.0' to location {host=ceph01,root=default} in crush map

    $ ceph osd crush reweight osd.0 0.00980
    reweighted item id 0 name 'osd.0' to 0.0098 in crush map

Kết quả

    $ceph osd df tree

    ID   CLASS  WEIGHT   REWEIGHT  SIZE    RAW USE  DATA     OMAP    META     AVAIL    %USE   VAR   PGS  STATUS  TYPE NAME          
    -14         0.02939         -  30 GiB  2.2 GiB   24 MiB  20 KiB  2.2 GiB   28 GiB   7.29  1.07    -  @        root ssd           
    -9         0.00980         -  10 GiB  1.1 GiB  8.1 MiB   8 KiB  1.1 GiB  8.9 GiB  10.61  1.56    -              host ceph01_ssd
    1    hdd  0.00980   1.00000  10 GiB  1.1 GiB  8.1 MiB   8 KiB  1.1 GiB  8.9 GiB  10.61  1.56   32      up          osd.1      
    -10         0.00980         -  10 GiB  166 MiB  8.2 MiB   6 KiB  158 MiB  9.8 GiB   1.62  0.24    -              host ceph02_ssd
    3    hdd  0.00980   1.00000  10 GiB  166 MiB  8.2 MiB   6 KiB  158 MiB  9.8 GiB   1.62  0.24   32      up          osd.3      
    -11         0.00980         -  10 GiB  986 MiB  8.2 MiB   6 KiB  977 MiB  9.0 GiB   9.63  1.41    -              host ceph03_ssd
    5    hdd  0.00980   1.00000  10 GiB  986 MiB  8.2 MiB   6 KiB  977 MiB  9.0 GiB   9.63  1.41   32      up          osd.5      
    -1         0.02939         -  30 GiB  1.9 GiB  1.3 GiB  12 KiB  598 MiB   28 GiB   6.32  0.93    -          root default       
    -3         0.00980         -  10 GiB  715 MiB  305 MiB     0 B  410 MiB  9.3 GiB   6.98  1.03    -              host ceph01    
    0    hdd  0.00980   1.00000  10 GiB  715 MiB  305 MiB     0 B  410 MiB  9.3 GiB   6.98  1.03   21      up          osd.0      
    -5         0.00980         -  10 GiB  613 MiB  520 MiB   5 KiB   93 MiB  9.4 GiB   5.99  0.88    -              host ceph02    
    2    hdd  0.00980   1.00000  10 GiB  613 MiB  520 MiB   5 KiB   93 MiB  9.4 GiB   5.99  0.88   33      up          osd.2      
    -7         0.00980         -  10 GiB  614 MiB  520 MiB   7 KiB   94 MiB  9.4 GiB   6.00  0.88    -              host ceph03    
    4    hdd  0.00980   1.00000  10 GiB  614 MiB  520 MiB   7 KiB   94 MiB  9.4 GiB   6.00  0.88   33      up          osd.4      
                            TOTAL  60 GiB  4.1 GiB  1.3 GiB  36 KiB  2.7 GiB   56 GiB   6.81                                        
    MIN/MAX VAR: 0.24/1.56  STDDEV: 2.91