# Fio
Fio (Flexible I/O Tester), một công cụ được sử dụng để đo lường hiệu suất I/O (đầu vào/đầu ra) của các thiết bị lưu trữ như ổ đĩa cứng hoặc ổ đĩa SSD. Dưới đây là giải thích các thông số trong đầu ra này:

- Laying out IO file (1 file / 4096MiB): fio đang chuẩn bị tệp tin IO có kích thước 4096MiB để thực hiện các thao tác I/O trên tệp tin này.
- Jobs: 1 (f=1): [m(1)][100.0%][r=8448KiB/s,w=2910KiB/s][r=2112,w=727 IOPS][eta 00m:00s]: fio đang thực hiện một công việc (job) với một tệp tin I/O (f=1). Công việc đang thực hiện ở 100% hiệu suất, đọc với tốc độ 8.448 KiB/s và ghi với tốc độ 2.910 KiB/s. Tốc độ đọc và ghi được tính theo đơn vị KiB/s và IOPS (I/O operations per second).
- (groupid=0, jobs=1): err= 0: pid=5781: Thu Jun 29 10:18:43 2023: Thông tin về nhóm thực hiện công việc và thông tin thời gian bắt đầu công việc.
- read: IOPS=2229, BW=8920KiB/s (9134kB/s)(3070MiB/352440msec): Tốc độ đọc được tính bằng cách đo số lần thực hiện các thao tác đọc trên đơn vị thời gian và tốc độ trung bình của các thao tác đọc trong đơn vị thời gian. Tốc độ đọc ở đây là 2.229 IOPS và 8920 KiB/s (9.134 kB/s). Thời gian thực hiện là 3.52440 giây và kích thước tệp đọc là 3.070 MiB.
- write: IOPS=745, BW=2981KiB/s (3053kB/s)(1026MiB/352440msec); 0 zone resets: Tốc độ ghi được tính bằng cách đo số lần thực hiện các thao tác ghi trên đơn vị thời gian và tốc độ trung bình của các thao tác ghi trong đơn vị thời gian. Tốc độ ghi ở đây là 745 IOPS và 2981 KiB/s (3.053 kB/s). Thời gian thực hiện là 3.52440 giây và kích thước tệp ghi là 1.026 MiB.
- cpu: Thông tin về sử dụng CPU cho quá trình I/O.
- IO depths: Thông tin về độ sâu IO (IO depth) được sử dụng để thực hiện các thao tác I/O.
- submit: Thông tin về số lượng các yêu cầu I/O được gửi đến thiết bị lưu trữ.
- complete: Thông tin về số lượng các yêu cầu I/O hoàn thành trên thiết bị lưu trữ.
- issued rwts: Tổng số yêu cầu I/O được gửi đến thiết bị lưu trữ và số lượng yêu cầu I/O bị rút ngắn hoặc bị bỏ qua.
- latency: Thông tin về thời gian trễ (latency) của các yêu cầu I/O. Ở đây, thời gian trễ mục tiêu (target) và cửa sổ (window) được đặt là 0, và các yêu cầu I/O đượcxếp hạng theo phân vị 100.00%. Độ sâu IO được sử dụng là 64.

- Run status group 0 (all jobs): Thông tin về hiệu suất chung của các công việc I/O được thực hiện.
- Disk stats (read/write): Thông tin về số lượng yêu cầu I/O được gửi đến thiết bị lưu trữ và thời gian hoàn thành các yêu cầu I/O. Thông tin về số lượng yêu cầu I/O được gộp lại (merge) và số lượng ticks (thời gian xử lý) của thiết bị lưu trữ. Thông tin về số lượng yêu cầu I/O đang đợi trong hàng đợi (in_queue) và tỷ lệ sử dụng tài nguyên của thiết bị lưu trữ (util). Trong đầu ra này, tên thiết bị lưu trữ là "sda".
## Ví dụ về sequential

```
fiotest sudo fio --ioengine=sync --rw=write --bs=4k --numjobs=1 --size=1G --filename=testfile --name=mytest
mytest: (g=0): rw=write, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=sync, iodepth=1
fio-3.28
Starting 1 process
mytest: Laying out IO file (1 file / 1024MiB)

mytest: (groupid=0, jobs=1): err= 0: pid=15267: Tue Jul 11 15:13:07 2023
  write: IOPS=410k, BW=1600MiB/s (1678MB/s)(1024MiB/640msec); 0 zone resets
    clat (nsec): min=1780, max=123801, avg=2219.61, stdev=1339.84
     lat (nsec): min=1809, max=123896, avg=2251.64, stdev=1344.31
    clat percentiles (nsec):
     |  1.00th=[ 1816],  5.00th=[ 1848], 10.00th=[ 1864], 20.00th=[ 1864],
     | 30.00th=[ 1880], 40.00th=[ 1896], 50.00th=[ 1928], 60.00th=[ 1976],
     | 70.00th=[ 2064], 80.00th=[ 2224], 90.00th=[ 2896], 95.00th=[ 3216],
     | 99.00th=[ 6240], 99.50th=[ 6880], 99.90th=[16320], 99.95th=[27520],
     | 99.99th=[47360]
   bw (  MiB/s): min= 1580, max= 1580, per=98.75%, avg=1580.05, stdev= 0.00, samples=1
   iops        : min=404492, max=404492, avg=404492.00, stdev= 0.00, samples=1
  lat (usec)   : 2=64.08%, 4=33.17%, 10=2.55%, 20=0.13%, 50=0.07%
  lat (usec)   : 100=0.01%, 250=0.01%
  cpu          : usr=24.26%, sys=75.27%, ctx=1, majf=0, minf=12
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,262144,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1

Run status group 0 (all jobs):
  WRITE: bw=1600MiB/s (1678MB/s), 1600MiB/s-1600MiB/s (1678MB/s-1678MB/s), io=1024MiB (1074MB), run=640-640msec

Disk stats (read/write):
  sda: ios=0/1, merge=0/0, ticks=0/0, in_queue=0, util=0.54%

```
## Vi du về random
```
$ fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=vutest1 --filename=vutest1 --bs=4k --iodepth=64 --size=4G --readwrite=randrw --rwmixread=75

##OUTPUT
sudo fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=vu --filename=vutest1 --bs=8k --iodepth=100 --size=4G --readwrite=randrw --rwmixread=75

vutest1: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=64
fio-3.28
Starting 1 process
vutest1: Laying out IO file (1 file / 4096MiB)
Jobs: 1 (f=1): [m(1)][99.8%][r=7751KiB/s,w=2678KiB/s][r=1937,w=669 IOPS][eta 00m:01s] 
vutest1: (groupid=0, jobs=1): err= 0: pid=15344: Thu Jun 29 17:05:47 2023
  read: IOPS=1628, BW=6514KiB/s (6671kB/s)(3070MiB/482580msec)
   bw (  KiB/s): min=  200, max=12456, per=100.00%, avg=6519.97, stdev=3595.69, samples=963
   iops        : min=   50, max= 3114, avg=1629.84, stdev=898.89, samples=963
  write: IOPS=544, BW=2177KiB/s (2229kB/s)(1026MiB/482580msec); 0 zone resets
   bw (  KiB/s): min=   56, max= 4296, per=100.00%, avg=2178.83, stdev=1210.42, samples=963
   iops        : min=   14, max= 1074, avg=544.57, stdev=302.55, samples=963
  cpu          : usr=2.42%, sys=7.35%, ctx=558649, majf=0, minf=9
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.1%, >=64=0.0%
     issued rwts: total=785920,262656,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=64

Run status group 0 (all jobs):
   READ: bw=6514KiB/s (6671kB/s), 6514KiB/s-6514KiB/s (6671kB/s-6671kB/s), io=3070MiB (3219MB), run=482580-482580msec
  WRITE: bw=2177KiB/s (2229kB/s), 2177KiB/s-2177KiB/s (2229kB/s-2229kB/s), io=1026MiB (1076MB), run=482580-482580msec

Disk stats (read/write):
  sda: ios=783636/265685, merge=1271/3320, ticks=25452713/4759237, in_queue=30227332, util=99.87%

```


Trên là một câu lệnh trong hệ điều hành Linux sử dụng công cụ FIO (Flexible I/O Tester) để kiểm tra hiệu suất I/O của ổ đĩa.

Cụ thể, câu lệnh trên đang yêu cầu FIO thực hiện các thao tác I/O random trên tệp tin "vutest1" với dung lượng 4GB, kích thước khối dữ liệu là 4KB (block size), với tỷ lệ đọc/ghi là 75/25 (rwmixread=75). Để tăng hiệu suất, FIO sử dụng các thủ tục I/O trực tiếp (direct=1) và sử dụng libaio làm engine I/O (ioengine=libaio). Đồng thời, để giảm độ chính xác về thời gian đo đạt, tùy chọn --gtod_reduce=1 sẽ được sử dụng.

Chú thích các tham số:
- randrepeat=1: Lặp lại các chu kỳ I/O với dữ liệu ngẫu nhiên.
- ioengine=libaio: Sử dụng libaio làm engine I/O.
- direct=1: Sử dụng thủ tục I/O trực tiếp.
- gtod_reduce=1: Giảm độ chính xác về thời gian đo đạt.
- name=vutest1: Đặt tên cho công việc kiểm tra là "vutest1".
- filename=vutest1: Tên tệp tin sẽ được sử dụng để kiểm tra hiệu suất I/O.
- bs=4k: Kích thước khối dữ liệu (block size) là 4KB.
- iodepth=64: Số lượng thao tác I/O được sử dụng đồng thời.
- size=4G: Dung lượng tệp tin được sử dụng để kiểm tra hiệu suất I/O là 4GB.
- readwrite=randrw: Loại thao tác I/O sẽ được sử dụng, ở đây là random read write.
- rwmixread=75: Tỷ lệ đọc/ghi là 75/25.


Dưới đây là các thông số được đưa ra trong kết quả của câu lệnh FIO:

- IOPS (Input/Output Operations Per Second): Được đưa ra cho thao tác đọc (read) và thao tác ghi (write) riêng biệt. 

  + Thao tác đọc: IOPS=1628
  + Thao tác ghi: IOPS=544

- Throughput (tốc độ truyền dữ liệu): Được đưa ra cho thao tác đọc (read) và thao tác ghi (write) riêng biệt. 

  + Thao tác đọc: BW=6514KiB/s (6671kB/s)(3070MiB/482580msec)
  + Thao tác ghi: BW=2177KiB/s (2229kB/s)(1026MiB/482580msec)

- Latency (độ trễ):

- CPU utilization (tỉ lệ sử dụng CPU): Được đưa ra cho toàn bộ quá trình thực hiện kiểm tra hiệu suất I/O.

  + Tỉ lệ sử dụng CPU: usr=2.42%, sys=7.35%, ctx=558649, majf=0, minf=9

- Các thông số khác:

  + IO depths: Chỉ ra tỉ lệ yêu cầu I/O được gửi với các độ sâu khác nhau. Ví dụ: 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
  + Latency: Chỉ ra độ trễ tối đa và độ sâu yêu cầu I/O. Ví dụ: target=0, window=0, percentile=100.00%, depth=64
  + Disk stats: Chỉ ra các thông số thống kê cho các yêu cầu I/O trên ổ đĩa. Ví dụ: ios=783636/265685, merge=1271/3320, ticks=25452713/4759237, in_queue=30227332, util=99.87%
# dd  
```
$ dd if=/dev/zero of=/tmp/output bs=8k count=10k; rm -f /tmp/output
10240+0 records in
10240+0 records out
83886080 bytes (84 MB, 80 MiB) copied, 0.0749867 s, 1.1 GB/s
```


Lệnh `dd if=/dev/zero of=/tmp/output bs=8k count=10k; rm -f /tmp/output` tạo ra một tệp tin tên là `/tmp/output` và điền nó với các giá trị 0 bằng lệnh `dd`. `if` chỉ định tệp đầu vào, trong trường hợp này là `/dev/zero.`

`bs=8k` thiết lập kích thước khối là 8 kilobytes và cờ count=10k thiết lập số lượng khối là 10,000, do đó tổng cộng 80 megabytes được ghi vào tệp.

Lệnh rm xóa tệp `/tmp/output`.

Thông báo đầu ra 
```
10240+0 records in
10240+0 records out
83886080 bytes (84 MB, 80 MiB) copied, 0.0749867 s, 1.1 GB/s
```
Cho biết rằng lệnh đã sao chép 84 megabytes dữ liệu trong 0,0749867 giây, với tốc độ đạt 1.1 gigabytes mỗi giây. Thông báo cũng cho thấy rằng có 10240+0 bản ghi được đọc và ghi, và kích thước tệp là 84 megabytes (hoặc 80 mebibytes).
