### Nguyên lý hoạt động của RAID là tạo một mảng đĩa từ nhiều ổ đĩa khác nhau để tăng tốc độ đọc/ghi dữ liệu và đảm bảo tính toàn vẹn dữ liệu. Các dữ liệu sẽ được phân chia thành các khối và phân tán trên các ổ đĩa khác nhau, từ đó tăng tốc độ đọc/ghi dữ liệu. Khi một ổ đĩa bị hỏng, dữ liệu được sao lưu trên các ổ đĩa khác để đảm bảo tính toàn vẹn dữ liệu.

Mdadm (Multiple Device Admin) là một công cụ quản lý RAID trong Linux. Nó cho phép tạo, quản lý và kiểm tra các mảng RAID trên Linux. Để sử dụng mdadm, bạn cần cài đặt gói mdadm trên hệ thống của mình.

Các bước để tạo và quản lý một mảng RAID bằng mdadm như sau:

Kiểm tra các ổ đĩa có sẵn trong hệ thống của bạn bằng lệnh sau:
```
sudo fdisk -l
```
Tạo một mảng RAID bằng lệnh sau:

```
sudo mdadm --create /dev/md0 --level=<RAID level> --raid-devices=<number of disks> /dev/sd[a,b,c,..]    
```
Ví dụ, để tạo một mảng RAID 5 với 3 ổ đĩa /dev/sda, /dev/sdb và /dev/sdc, bạn có thể sử dụng lệnh sau:

```

sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
```
Định dạng các phân vùng của mảng RAID bằng lệnh sau:
```

sudo mkfs.<filesystem type> /dev/md0
```
Ví dụ, để định dạng một phân vùng của mảng RAID bằng hệ thống tệp ext4, bạn có thể sử dụng lệnh sau:
```

sudo mkfs.ext4 /dev/md0
```
Mount mảng RAID bằng lệnh sau:
```

sudo mount /dev/md0 /mnt
```
Để kiểm tra trạng thái của mảng RAID, sử dụng lệnh sau:
```

sudo mdadm --detail /dev/md0
```
Để thêm một ổ đĩa mới vào mảng RAID, sử dụng lệnh sau:
```

sudo mdadm --add /dev/md0 /dev/sd[x]
```
Để tháo gỡ một ổ đĩa khỏi mảng RAID, sử dụng lệnh sau:
```

sudo mdadm --remove /dev/md0 /dev/sd[x]
```




Để xem danh sách các ổ đĩa sử dụng trong mảng RAID, sử dụng lệnh sau:
```

sudo mdadm --examine /dev/sd[a,b,c,..]
```
Để ghi lại cấu hình của mảng RAID, sử dụng lệnh sau:
```

sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```
Lệnh này sẽ lưu cấu hình của mảng RAID vào tệp /etc/mdadm/mdadm.conf.


Để khởi động lại mảng RAID khi khởi động hệ thống, sử dụng lệnh sau:


```
sudo mdadm --assemble --scan
```
Lệnh này sẽ khôi phục các mảng RAID được lưu trữ trong tệp cấu hình /etc/mdadm/mdadm.conf.