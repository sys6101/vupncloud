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