# Pool benchmark
Tạo pool với command 

    ceph osd pool create test 100 100

Tạo image :

    rbd create --size {megabytes} {pool-name}/{image-name}
    rbd create --size 1024 test/image01

Tạo file config fio :

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

Bắt đầu benchmark:

    $ fio test.fio

    rbd_iodepth32: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=rbd, iodepth=32                                                                                                   
    fio-3.19
    Starting 1 process
    Jobs: 1 (f=1): [m(1)][100.0%][r=5694KiB/s,w=5862KiB/s][r=1423,w=1465 IOPS][eta 00m:00s]
    rbd_iodepth32: (groupid=0, jobs=1): err= 0: pid=340590: Thu Aug 10 08:48:17 2023
    read: IOPS=1787, BW=7148KiB/s (7320kB/s)(512MiB/73329msec)
        slat (nsec): min=1278, max=10727k, avg=43300.59, stdev=206926.50
        clat (usec): min=36, max=66353, avg=3959.81, stdev=3250.50
        lat (usec): min=691, max=66368, avg=4003.11, stdev=3257.85
        clat percentiles (usec):
        |  1.00th=[ 1074],  5.00th=[ 1369], 10.00th=[ 1582], 20.00th=[ 1958],
        | 30.00th=[ 2311], 40.00th=[ 2638], 50.00th=[ 3032], 60.00th=[ 3490],
        | 70.00th=[ 4080], 80.00th=[ 5014], 90.00th=[ 7177], 95.00th=[10290],
        | 99.00th=[17695], 99.50th=[20841], 99.90th=[29230], 99.95th=[31851],
        | 99.99th=[40109]
    bw (  KiB/s): min= 5213, max= 8487, per=100.00%, avg=7157.08, stdev=658.25, samples=146
    iops        : min= 1303, max= 2121, avg=1789.03, stdev=164.60, samples=146
    write: IOPS=1787, BW=7152KiB/s (7323kB/s)(512MiB/73329msec); 0 zone resets
        slat (usec): min=2, max=12554, avg=49.91, stdev=211.14
        clat (usec): min=3223, max=75943, avg=13840.55, stdev=5811.67
        lat (usec): min=3236, max=75952, avg=13890.46, stdev=5815.53
        clat percentiles (usec):
        |  1.00th=[ 6521],  5.00th=[ 7767], 10.00th=[ 8455], 20.00th=[ 9503],
        | 30.00th=[10421], 40.00th=[11338], 50.00th=[12256], 60.00th=[13435],
        | 70.00th=[14877], 80.00th=[17171], 90.00th=[21365], 95.00th=[25560],
        | 99.00th=[34341], 99.50th=[38011], 99.90th=[47449], 99.95th=[53216],
        | 99.99th=[63701]
    bw (  KiB/s): min= 5085, max= 8320, per=100.00%, avg=7159.22, stdev=617.46, samples=146
    iops        : min= 1271, max= 2080, avg=1789.58, stdev=154.45, samples=146
    lat (usec)   : 50=0.01%, 100=0.01%, 250=0.01%, 500=0.01%, 750=0.01%
    lat (usec)   : 1000=0.25%
    lat (msec)   : 2=10.36%, 4=23.88%, 10=25.53%, 20=33.26%, 50=6.66%
    lat (msec)   : 100=0.04%
    cpu          : usr=5.32%, sys=3.37%, ctx=134121, majf=0, minf=49
    IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=100.0%, >=64=0.0%
        submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
        complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.1%, 64=0.0%, >=64=0.0%
        issued rwts: total=131040,131104,0,0 short=0,0,0,0 dropped=0,0,0,0
        latency   : target=0, window=0, percentile=100.00%, depth=32

    Run status group 0 (all jobs):
    READ: bw=7148KiB/s (7320kB/s), 7148KiB/s-7148KiB/s (7320kB/s-7320kB/s), io=512MiB (537MB), run=73329-73329msec                                                                                                 
    WRITE: bw=7152KiB/s (7323kB/s), 7152KiB/s-7152KiB/s (7323kB/s-7323kB/s), io=512MiB (537MB), run=73329-73329msec                                                                                                 

    Disk stats (read/write):
    vda: ios=49/3713, merge=0/245, ticks=42/5403, in_queue=5445, util=3.53%                                                                                              
- IOPS (Input/Output Operations Per Second): Số lần thực hiện I/O (đọc/ghi) trên giây. Đây là một thước đo quan trọng về khả năng xử lý của hệ thống.

- Bandwidth (BW): Tốc độ truyền dữ liệu trung bình, thường tính bằng Megabytes hoặc Kilobytes trên giây. Đây cho bạn biết tốc độ chuyển dữ liệu giữa hệ thống và thiết bị lưu trữ.

- Latency: Độ trễ trong thực hiện I/O, thường được tính bằng micro giây (μs) hoặc mili giây (ms). Đây là thời gian mà hệ thống phải chờ để hoàn thành một hoạt động I/O.

- CPU Utilization: Sử dụng CPU, thể hiện phần trăm thời gian CPU được sử dụng trong quá trình kiểm tra. Điều này giúp bạn đánh giá mức độ sử dụng tài nguyên CPU.

- Throughput: Lưu lượng truyền tải, tức là tổng lượng dữ liệu được truyền trong một khoảng thời gian cụ thể.

- I/O Depth: là số lượng yêu cầu I/O đang được thực hiện cùng một lúc. Đây có thể ảnh hưởng đến hiệu suất hệ thống.

- Percentiles: Thời gian trễ tại các phần trăm khác nhau. Thường được sử dụng để hiểu rõ hơn về biểu đồ thời gian trễ.

- Error Rates: Tỷ lệ lỗi trong quá trình thực hiện kiểm tra. Điều này có thể giúp bạn xác định các vấn đề tiềm ẩn trong hệ thống.


## Tách thành 2 pool ssd và hdd
**Muốn tách osd  hành 2 class ssd và hdd thì cần xóa class cũ để set class mới:**

    [root@ceph01 centos]# ceph osd crush rm-device-class osd.0
    done removing class of osd(s): 0
    [root@ceph01 centos]# ceph osd crush set-device-class ssd osd.0
    set osd(s) 0 to class 'ssd'

- `osd.1` và `osd.2` tương tự

**Taọ lần lượt pool ssd và hdd**

    ceph osd pool create ssd 100 100
    ceph osd pool create hdd 100 100

**Tạo rule**        
Tạo hai rule Crush Map mới ("replicated_ssd" và "replicated_hdd"), mỗi rule quy định cách dữ liệu sẽ được phân phối trên các thiết bị lưu trữ của hai lớp khác nhau (SSD và HDD) dựa trên mức độ phân tán cấp "host".

    ceph osd crush rule create-replicated <rule-name> <root> <failure-domain> <class>
    ceph osd crush rule create-replicated replicated_ssd default host ssd
    ceph osd crush rule create-replicated replicated_hdd default host hdd

**Set rule  cho 2 pool ssd và hdd**

    ceph osd pool set ssd crush_rule feplicated_ssd
    ceph osd pool set hdd crush_rule feplicated_hdd

**Show shadow crush map tree**

    [root@ceph01 ~]# ceph osd crush tree --show-shadow
    ID   CLASS  WEIGHT   TYPE NAME          
    -12    ssd  0.02939  root default~ssd   
    -11    ssd  0.00980      host ceph01~ssd
    2    ssd  0.00980          osd.2      
    -9    ssd  0.00980      host ceph02~ssd
    0    ssd  0.00980          osd.0      
    -10    ssd  0.00980      host ceph03~ssd
    1    ssd  0.00980          osd.1      
    -2    hdd  0.02939  root default~hdd   
    -8    hdd  0.00980      host ceph01~hdd
    5    hdd  0.00980          osd.5      
    -4    hdd  0.00980      host ceph02~hdd
    3    hdd  0.00980          osd.3      
    -6    hdd  0.00980      host ceph03~hdd
    4    hdd  0.00980          osd.4      
    -1         0.05878  root default       
    -7         0.01959      host ceph01    
    5    hdd  0.00980          osd.5      
    2    ssd  0.00980          osd.2      
    -3         0.01959      host ceph02    
    3    hdd  0.00980          osd.3      
    0    ssd  0.00980          osd.0      
    -5         0.01959      host ceph03    
    4    hdd  0.00980          osd.4      
    1    ssd  0.00980          osd.1 

**rule dump**

        "rule_id": 2,
        "rule_name": "replicated_ssd",
        "ruleset": 2,
        "type": 1,
        "min_size": 1,
        "max_size": 10,
        "steps": [
            {
                "op": "take",
                "item": -12,
                "item_name": "default~ssd"
            },
            {
                "op": "chooseleaf_firstn",
                "num": 0,
                "type": "host"
            },
            {
                "op": "emit"
            }
        ]
    },
    {
        "rule_id": 3,
        "rule_name": "replicated_hdd",
        "ruleset": 3,
        "type": 1,
        "min_size": 1,
        "max_size": 10,
        "steps": [
            {
                "op": "take",
                "item": -2,
                "item_name": "default~hdd"
            },
            {
                "op": "chooseleaf_firstn",
                "num": 0,
                "type": "host"
            },
            {
                "op": "emit"
            }
        ]
    }

**Benchmark 2 pool**

- Pool ssd  


        [root@ceph01 ~]# fio ssd.fio
        rbd_iodepth32: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=rbd, iodepth=32                                                                                            
        fio-3.19
        Starting 1 process
        Jobs: 1 (f=1): [m(1)][100.0%][r=6534KiB/s,w=6638KiB/s][r=1633,w=1659 IOPS][eta 00m:00s]
        rbd_iodepth32: (groupid=0, jobs=1): err= 0: pid=383587: Fri Aug 11 03:23:07 2023
        read: IOPS=1806, BW=7228KiB/s (7401kB/s)(512MiB/72522msec)
            slat (nsec): min=1321, max=12112k, avg=37551.28, stdev=192011.67
            clat (usec): min=47, max=55800, avg=2770.55, stdev=3079.43
            lat (usec): min=312, max=55818, avg=2808.10, stdev=3085.24
            clat percentiles (usec):
            |  1.00th=[  506],  5.00th=[  676], 10.00th=[  816], 20.00th=[ 1057],
            | 30.00th=[ 1303], 40.00th=[ 1565], 50.00th=[ 1860], 60.00th=[ 2212],
            | 70.00th=[ 2704], 80.00th=[ 3458], 90.00th=[ 5276], 95.00th=[ 8586],
            | 99.00th=[15795], 99.50th=[19792], 99.90th=[30802], 99.95th=[34341],
            | 99.99th=[41681]
        bw (  KiB/s): min= 3928, max= 9312, per=100.00%, avg=7241.40, stdev=830.99, samples=144
        iops        : min=  982, max= 2328, avg=1810.13, stdev=207.74, samples=144
        write: IOPS=1807, BW=7231KiB/s (7405kB/s)(512MiB/72522msec); 0 zone resets
            slat (usec): min=3, max=11885, avg=44.00, stdev=191.11
            clat (msec): min=3, max=125, avg=14.84, stdev= 6.36
            lat (msec): min=3, max=125, avg=14.89, stdev= 6.37
            clat percentiles (usec):
            |  1.00th=[ 7046],  5.00th=[ 8291], 10.00th=[ 8979], 20.00th=[10159],
            | 30.00th=[11207], 40.00th=[12125], 50.00th=[13173], 60.00th=[14484],
            | 70.00th=[15926], 80.00th=[18482], 90.00th=[22676], 95.00th=[27395],
            | 99.00th=[37487], 99.50th=[41681], 99.90th=[54264], 99.95th=[61080],
            | 99.99th=[83362]
        bw (  KiB/s): min= 4056, max= 8734, per=100.00%, avg=7243.94, stdev=827.92, samples=144
        iops        : min= 1014, max= 2183, avg=1810.77, stdev=206.98, samples=144
        lat (usec)   : 50=0.01%, 100=0.01%, 250=0.02%, 500=0.45%, 750=3.23%
        lat (usec)   : 1000=4.98%
        lat (msec)   : 2=18.37%, 4=15.09%, 10=15.20%, 20=34.68%, 50=7.89%
        lat (msec)   : 100=0.08%, 250=0.01%
        cpu          : usr=5.45%, sys=3.50%, ctx=119576, majf=0, minf=50
        IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=100.0%, >=64=0.0%
            submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
            complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.1%, 64=0.0%, >=64=0.0%
            issued rwts: total=131040,131104,0,0 short=0,0,0,0 dropped=0,0,0,0
            latency   : target=0, window=0, percentile=100.00%, depth=32

        Run status group 0 (all jobs):
        READ: bw=7228KiB/s (7401kB/s), 7228KiB/s-7228KiB/s (7401kB/s-7401kB/s), io=512MiB (537MB), run=72522-72522msec                                                                                          
        WRITE: bw=7231KiB/s (7405kB/s), 7231KiB/s-7231KiB/s (7405kB/s-7405kB/s), io=512MiB (537MB), run=72522-72522msec                                                                                          

        Disk stats (read/write):
        vda: ios=1102/4821, merge=0/281, ticks=789/15389, in_queue=16178, util=5.73%

- Pool hdd

        rbd_iodepth32: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=rbd, iodepth=32                                                                                            
        fio-3.19
        Starting 1 process
        Jobs: 1 (f=1): [m(1)][100.0%][r=6351KiB/s,w=6530KiB/s][r=1587,w=1632 IOPS][eta 00m:00s]
        rbd_iodepth32: (groupid=0, jobs=1): err= 0: pid=392524: Fri Aug 11 03:26:50 2023
        read: IOPS=1851, BW=7407KiB/s (7585kB/s)(512MiB/70763msec)
            slat (nsec): min=1329, max=9354.7k, avg=35906.36, stdev=177193.61
            clat (usec): min=35, max=79680, avg=2664.89, stdev=2990.95
            lat (usec): min=331, max=79686, avg=2700.80, stdev=2995.93
            clat percentiles (usec):
            |  1.00th=[  510],  5.00th=[  685], 10.00th=[  816], 20.00th=[ 1045],
            | 30.00th=[ 1270], 40.00th=[ 1516], 50.00th=[ 1811], 60.00th=[ 2147],
            | 70.00th=[ 2606], 80.00th=[ 3294], 90.00th=[ 5014], 95.00th=[ 8160],
            | 99.00th=[15401], 99.50th=[19268], 99.90th=[28443], 99.95th=[33817],
            | 99.99th=[52691]
        bw (  KiB/s): min= 2768, max= 9537, per=100.00%, avg=7429.22, stdev=931.86, samples=140
        iops        : min=  692, max= 2384, avg=1857.12, stdev=232.96, samples=140
        write: IOPS=1852, BW=7411KiB/s (7589kB/s)(512MiB/70763msec); 0 zone resets
            slat (usec): min=2, max=21146, avg=42.65, stdev=187.91
            clat (msec): min=4, max=153, avg=14.52, stdev= 6.29
            lat (msec): min=4, max=153, avg=14.57, stdev= 6.29
            clat percentiles (usec):
            |  1.00th=[ 7111],  5.00th=[ 8225], 10.00th=[ 8979], 20.00th=[10028],
            | 30.00th=[10945], 40.00th=[11863], 50.00th=[12780], 60.00th=[13960],
            | 70.00th=[15533], 80.00th=[17957], 90.00th=[22152], 95.00th=[26608],
            | 99.00th=[36439], 99.50th=[41681], 99.90th=[58983], 99.95th=[72877],
            | 99.99th=[84411]
        bw (  KiB/s): min= 2776, max= 8940, per=100.00%, avg=7431.84, stdev=894.84, samples=140
        iops        : min=  694, max= 2235, avg=1857.76, stdev=223.70, samples=140
        lat (usec)   : 50=0.01%, 100=0.01%, 250=0.02%, 500=0.42%, 750=3.30%
        lat (usec)   : 1000=5.18%
        lat (msec)   : 2=19.03%, 4=14.89%, 10=15.19%, 20=34.63%, 50=7.24%
        lat (msec)   : 100=0.11%, 250=0.01%
        cpu          : usr=5.60%, sys=3.53%, ctx=121591, majf=0, minf=50
        IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=100.0%, >=64=0.0%
            submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
            complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.1%, 64=0.0%, >=64=0.0%
            issued rwts: total=131040,131104,0,0 short=0,0,0,0 dropped=0,0,0,0
            latency   : target=0, window=0, percentile=100.00%, depth=32

        Run status group 0 (all jobs):
        READ: bw=7407KiB/s (7585kB/s), 7407KiB/s-7407KiB/s (7585kB/s-7585kB/s), io=512MiB (537MB), run=70763-70763msec
        WRITE: bw=7411KiB/s (7589kB/s), 7411KiB/s-7411KiB/s (7589kB/s-7589kB/s), io=512MiB (537MB), run=70763-70763msec

        Disk stats (read/write):
        vda: ios=411/4158, merge=0/173, ticks=505/15747, in_queue=16252, util=4.44%

<!-- Xóa bucket thì xóa thì xóa từ cái thấp nhất: `ceph osd crush remove` -->