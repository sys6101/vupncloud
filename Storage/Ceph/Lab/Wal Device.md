# Wal Device
Ceph Write-Ahead Log (WAL) device là một thiết bị được sử dụng trong Ceph để lưu trữ nhật ký ghi trước (write-ahead log). WAL device cung cấp một bộ đệm cho các hoạt động ghi vào OSD (Object Storage Daemon), giúp tăng cường hiệu suất và đảm bảo tính nhất quán của dữ liệu.

Dưới đây là một số lệnh và mô tả liên quan đến Ceph WAL device:

**Tạo WAL device**:

      vgcreate waldv /dev/vdc
      lvcreate -L 1GB -n wal3 waldv

```
 ceph-volume lvm prepare --bluestore --data /dev/vdb --block.wal waldv/wal1
 ceph-volume lvm activate --bluestore 0 13c6c914-c294-4c1c-86d8-387227daab82
```

   Lệnh trên tạo một WAL device trên thiết bị `dev/vdc` cho OSD với dữ liệu được lưu trữ trên `dev/vdb`. `<OSD_UUID>` là UUID duy nhất của OSD.

**Xóa WAL device**:
```
ceph-volume lvm destroy --osd-uuid <OSD_UUID>
```
   Lệnh trên xóa WAL device cho OSD với UUID `<OSD_UUID>`. Điều này sẽ gỡ bỏ kết nối giữa OSD và WAL device và xóa WAL device khỏi hệ thống.

**Kiểm tra thông tin WAL device**:

```
ceph-volume lvm list
```

      ceph osd df tree
      ID CLASS WEIGHT  REWEIGHT SIZE   RAW USE DATA    OMAP   META     AVAIL   %USE  VAR  PGS STATUS TYPE NAME       
      -1       0.02939        - 30 GiB 6.0 GiB 3.0 GiB 48 KiB  3.0 GiB  24 GiB 20.06 1.00   -        root default    
      -3       0.00980        - 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00   -            host ceph01 
      0   hdd 0.00980  1.00000 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00  32     up         osd.0   
      -5       0.00980        - 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00   -            host ceph02 
      1   hdd 0.00980  1.00000 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00  32     up         osd.1   
      -7       0.00980        - 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00   -            host ceph03 
      2   hdd 0.00980  1.00000 10 GiB 2.0 GiB 1.0 GiB 16 KiB 1024 MiB 8.0 GiB 20.06 1.00  32     up         osd.2   
                        TOTAL 30 GiB 6.0 GiB 3.0 GiB 49 KiB  3.0 GiB  24 GiB 20.06                                 
      MIN/MAX VAR: 1.00/1.00  STDDEV: 0





**Tạo pool với command**

      ceph osd pool create test 32 32

**Tạo image** :

      rbd create --size 1024 test/image01

**Tạo file config fio** :

      $ vim test.fio
      [global]
      ioengine=rbd
      clientname=admin
      pool=test
      rbdname=image01
      rw=randrw
      bs=4k
      [rbd_iodepth32]
      iodepth=32
      Wal device

![Alt text](/Picture/Storage/wal.png)

**Với cách tạo OSD bình thường**           

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


![Alt text](/Picture/Storage/wal1.png)
**Nhận xét**:

- Nhận thấy tốc độ đọc ghi khi sử dụng wal device tốt hơn khi add osd thông thường.   

**Cách thêm OSD thông thường**:
   - Trong cách thêm OSD thông thường, bạn sẽ cung cấp một thiết bị lưu trữ (ví dụ: ổ cứng) cho OSD.
   - OSD sẽ sử dụng thiết bị lưu trữ để lưu trữ cả dữ liệu và nhật ký ghi trước (write-ahead log).
   - Cách thêm OSD thông thường phù hợp cho các trường hợp khi không cần sự tách rời giữa dữ liệu và nhật ký ghi trước.

**Ceph Write-Ahead Log (WAL) device**:
   - Trong trường hợp sử dụng WAL device, bạn sẽ cung cấp hai thiết bị: một thiết bị cho dữ liệu OSD và một thiết bị riêng cho WAL.
   - Thiết bị dữ liệu OSD được sử dụng để lưu trữ dữ liệu chính của OSD.
   - Thiết bị WAL được sử dụng để lưu trữ nhật ký ghi trước (write-ahead log), giúp tăng cường hiệu suất và tính nhất quán của dữ liệu.
   - Sử dụng WAL device phù hợp trong các trường hợp yêu cầu hiệu suất ghi cao hoặc độ tin cậy cao hơn, vì việc tách rời dữ liệu và nhật ký ghi trước giúp tránh xung đột và tăng khả năng phục hồi.