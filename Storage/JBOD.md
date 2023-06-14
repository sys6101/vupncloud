JBOD (Just a Bunch of Disks) và RAID (Redundant Array of Independent Disks) là hai phương pháp sử dụng ổ cứng để lưu trữ dữ liệu, tuy nhiên chúng có những điểm khác nhau quan trọng. Dưới đây là thông tin về JBOD và sự so sánh với RAID:

JBOD (Just a Bunch of Disks):

JBOD chỉ đơn giản là một nhóm các ổ cứng riêng lẻ được kết nối với hệ thống.
Các ổ cứng trong JBOD không được ghép lại thành một hệ thống lưu trữ duy nhất.
Mục đích chính của JBOD là mở rộng dung lượng lưu trữ bằng cách sử dụng nhiều ổ cứng và hiển thị chúng như một không gian lưu trữ duy nhất.
JBOD không cung cấp khả năng chịu lỗi hoặc bảo vệ dữ liệu. Nếu một ổ cứng trong JBOD gặp sự cố, dữ liệu trên ổ cứng đó có thể bị mất.
RAID (Redundant Array of Independent Disks):

RAID là một kỹ thuật kết hợp nhiều ổ cứng thành một hệ thống lưu trữ có khả năng chịu lỗi và cung cấp bảo vệ dữ liệu.
RAID cung cấp các cấp độ khác nhau (RAID 0, RAID 1, RAID 5, RAID 6, v.v.) để cung cấp khả năng sao lưu dữ liệu, tăng tốc độ đọc/ghi dữ liệu và/hoặc cung cấp khả năng chịu lỗi.
RAID sử dụng các kỹ thuật như striping (chia nhỏ dữ liệu thành các phần nhỏ và lưu trữ trên nhiều ổ cứng), mirroring (tạo bản sao dữ liệu trên nhiều ổ cứng), parities (tính toán các dữ liệu dự phòng) để đảm bảo tính toàn vẹn và khả năng phục hồi dữ liệu khi có sự cố xảy ra.
RAID có thể cung cấp sự chịu lỗi và bảo vệ dữ liệu thông qua sao lưu dữ liệu trên các ổ cứng khác nhau.
So sánh JBOD và RAID:

JBOD không cung cấp bất kỳ khả năng chịu lỗi hoặc bảo vệ dữ liệu nào, trong khi RAID cung cấp các cấp độ bảo vệ dữ liệu.
JBOD chỉ đơn giản là một nhóm các ổ cứng riêng lẻ, trong khi RAID kết hợp các ổ cứng thành một hệ thống lưu trữ.
RAID có thể cung cấp tăng tốc độ đọc/ghi dữ liệu và khả năng phục hồi dữ liệu khi có sự cố xảy ra, trong khi JBOD không có các tính năng này.
JBOD thường được sử dụng khi chỉ cần mở rộng dung lượng lưu trữ, trong khi RAID được sử dụng khi yêu cầu bảo vệ dữ liệu và khả năng chịu lỗi cao hơn.
Tóm lại, JBOD và RAID là hai phương pháp sử dụng ổ cứng cho lưu trữ dữ liệu, nhưng có các tính năng và mục đích khác nhau. RAID thường được ưu tiên sử dụng trong các môi trường yêu cầu độ tin cậy và an toàn dữ liệu cao hơn.








Kiểm tra danh sách các ổ cứng được nhận diện bằng lệnh sau:


`sudo fdisk -l`

```
I/O size (minimum/optimal): 512 bytes / 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

/dev/sda2     4096  1054719  1050624  513M EFI System
/dev/sda3  1054720 41940991 40886272 19,5G Linux filesystem


Disk /dev/sdb: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdc: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

```

Xác định các ổ cứng bạn muốn gộp thành JBOD. Chắc chắn rằng các ổ cứng không chứa dữ liệu quan trọng, vì JBOD không cung cấp bảo vệ dữ liệu.

Sử dụng công cụ mdadm để tạo JBOD trên các ổ cứng đã chọn. Cài đặt mdadm nếu chưa có bằng lệnh:


`sudo apt-get install mdadm`    
Sử dụng lệnh sau để tạo JBOD:


`sudo mdadm --create /dev/md0 --level=linear --raid-devices=2 /dev/sdb /dev/sdc ...`          
Trong đó:

`/dev/md0` là tên cho JBOD mới tạo ra. Bạn có thể đặt tên khác tùy ý.           
-`-level=linear` chỉ định rằng JBOD được tạo thành dạng tuyến tính.         
`--raid-devices=2` là số lượng ổ cứng được sử dụng trong JBOD.                
       

`cat /proc/mdstat`      
Lệnh này sẽ hiển thị thông tin về JBOD, bao gồm tên, trạng thái và các ổ cứng thành phần.

Định dạng JBOD với hệ thống tập tin. Sử dụng lệnh sau để tạo một hệ thống tập tin mới trên JBOD:



`sudo mkfs.ext4 /dev/md0`   
Thay /dev/md0 bằng đường dẫn của JBOD tương ứng.

Gắn JBOD vào hệ thống tập tin. Tạo một thư mục trên hệ thống và gắn JBOD vào thư mục đó bằng lệnh:



`sudo mount /dev/md0 /path/to/mount`    
Thay /dev/md0 bằng đường dẫn của JBOD và /path/to/mount bằng đường dẫn đến thư mục mà bạn muốn gắn JBOD.


Kiểm tra JBOD đã được gắn kết thành công bằng lệnh:
`df -h`     
JBOD sẽ hiển thị như một hệ thống tập tin được gắn kết với đường dẫn mà bạn đã chỉ định.
```
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           389M  2,1M  387M   1% /run
/dev/sda3        20G   11G  7,4G  60% /
tmpfs           1,9G     0  1,9G   0% /dev/shm
tmpfs           5,0M  4,0K  5,0M   1% /run/lock
/dev/sda2       512M  6,1M  506M   2% /boot/efi
tmpfs           389M  2,5M  387M   1% /run/user/1000
/dev/md0         40G   24K   38G   1% /media/cl1               
```

Lưu ý: Các bước trên chỉ cung cấp một khái niệm tổng quan về cách sử dụng JBOD trên Linux. Việc cấu hình chi tiết và tùy chỉnh có thể khác nhau tùy thuộc vào phiên bản Linux và sự đa dạng của hệ thống lưu trữ. Hãy đảm bảo bạn hiểu rõ trước khi thực hiện các thao tác trên dữ liệu quan trọng.