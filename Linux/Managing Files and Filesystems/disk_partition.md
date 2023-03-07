# Disk partition
Partition là một khái niệm trong hệ thống đĩa của máy tính. Nó giúp phân chia đĩa cứng (hoặc ổ đĩa) thành nhiều phần để sử dụng cho mục đích riêng biệt. Mỗi phân vùng (partition) được coi là một đĩa độc lập với các phân vùng khác.

## Tạo phân vùng
Fdisk là một công cụ command line được sử dụng để phân chia phân vùng ổ đĩa trên linux/unix.
Tạo một partition: (ví dụ ở đây tạo /dev/sda3p1 từ phân vùng của root là /dev/sda3)
1. Trước tiên kiểm tra phân vùng trên một đĩa cụ thể:
```
$ sudo fdisk -l /dev/sda3
Disk /dev/sda3: 21,9 GiB, 23457693696 bytes, 45815808 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```
2. Tiến hành phân vùng /dev/sda3
```
$ sudo fdisk /dev/sda3
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x11119dd8.
```
3. Tạo một partition mới
- `n`: để tạo
- `d`: để xoá

```
command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 
First sector (2048-45815807, default 2048): 
Last sector, +sectors or +size{K,M,G,T,P} (2048-45815807, default 45815807): +2G                 

Created a new partition 1 of type 'Linux' and of size 2 GiB.
```
- `p`:dành cho phân vùng chính
- `e`:dành cho phân vùng mở rộng
4. Kiểm tra lại /dev/sda3 sẽ xuất hiện phân vùng đã được tạo thành công:
```
Device      Boot Start     End Sectors Size Id Type
/dev/sda3p1       2048 4196351 4194304   2G 83 Linux
```