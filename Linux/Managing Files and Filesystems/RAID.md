## Các loại RAID

|Raid          | Info          |       Usecase       |Khả năng chịu lỗi|Dung lượng của hệ thống RAID|
|------------- | ------------- |---------------------|-----------------|-|
|Raid 0 - Striping  | Các ổ đĩa được kết hợp lại thành một không gian lưu trữ duy nhất.Dữ liệu được phân tán trên các ổ đĩa theo chu kỳ.Tối thiểu 2 ổ đĩa.|RAID 0 thường được sử dụng cho các ứng dụng yêu cầu tốc độ đọc/ghi dữ liệu cao như xử lý video, âm thanh hoặc game.|Không có khả năng chịu lỗi|Tổng dung lượng của các ổ đĩa gắn kết
|RAID 1 - Mirroring  | Các ổ đĩa được sao chép đầy đủ dữ liệu lên nhau.Dữ liệu được sao chép đồng thời lên cả hai ổ đĩa.Tối thiểu 2 ổ đĩa. |RAID 1 thường được sử dụng cho các ứng dụng yêu cầu độ tin cậy cao như các hệ thống lưu trữ dữ liệu quan trọng, cơ sở dữ liệu và máy chủ web.|Có khả năng chịu lỗi tối đa là n/2, trong đó "n" là số lượng ổ đĩa trong hệ thống|Dung lượng của 1 ổ đĩa
|RAID 5 - Striping with parity  | Dữ liệu được phân tán trên nhiều ổ đĩa, cùng với thông tin phục hồi sự cố (parity) được tính toán.Tối thiểu cần 3 ổ đĩa. |RAID 5 thường được sử dụng cho các ứng dụng yêu cầu tốc độ đọc/ghi dữ liệu tương đối cao như các hệ thống lưu trữ tập tin, máy chủ lưu trữ email, các ứng dụng phần mềm, các cơ sở dữ liệu có tính độ tin cậy trung bình.|Có khả năng chịu lỗi tối đa là 1 ổ đĩa|Tổng dung lượng của các ổ đĩa gắn kết trừ đi dung lượng của một ổ đĩa để lưu trữ thông tin parity.
|RAID 6 - Striping with double parity  |Tương tự như RAID 5, nhưng có thêm hai thông tin phục hồi sự cố.Tối thiểu cần 4 ổ đĩa.   |RAID 6 thường được sử dụng cho các ứng dụng yêu cầu độ tin cậy cao như các hệ thống lưu trữ dữ liệu quan trọng, các cơ sở dữ liệu lớn và máy chủ ảo hóa.|Có khả năng chịu lỗi tối đa là 2 ổ đĩa.|tổng dung lượng của các ổ đĩa gắn kết trừ đi dung lượng của hai ổ đĩa để lưu trữ thông tin parity
|RAID 10 - Mirrored Striping | Là một kết hợp giữa RAID 1 và RAID 0. RAID 10 kết hợp tốc độ của RAID 0 và tính độ tin cậy của RAID 1, tạo ra một hệ thống RAID với tốc độ cao và khả năng chống chịu lỗi tốt.Tối thiểu cần 4 ổ đĩa.|RAID 10 thường được sử dụng cho các ứng dụng yêu cầu tốc độ và độ tin cậy cao như các cơ sở dữ liệu quan trọng, máy chủ ảo hóa và các hệ thống lưu trữ dữ liệu lớn.|Có khả năng chịu lỗi tối đa là n/2, trong đó "n" là số lượng ổ đĩa trong hệ thống.|Nửa tổng dung lượng của các ổ đĩa gắn kết
- RAID 0 - Striping:
Lệnh để tạo RAID 0: 
```
mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sda1 /dev/sdb1
```
Lệnh để kiểm tra trạng thái RAID 0: `cat /proc/mdstat`  
Lệnh để xem thông tin chi tiết về RAID 0: `mdadm --detail /dev/md0`     
Lệnh để thêm ổ đĩa vào RAID 0: `mdadm --manage /dev/md0 --add /dev/sdc1`    
Lệnh để xóa ổ đĩa khỏi RAID 0: `mdadm --manage /dev/md0 --remove /dev/sdc1`
- RAID 1 - Mirroring:
Lệnh để tạo RAID 1: 
```
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
```
Lệnh để kiểm tra trạng thái RAID 1: `cat /proc/mdstat`  
Lệnh để xem thông tin chi tiết về RAID 1: `mdadm --detail /dev/md0`     
Lệnh để thêm ổ đĩa vào RAID 1: `mdadm --manage /dev/md0 --add /dev/sdc1`    
Lệnh để xóa ổ đĩa khỏi RAID 1: `mdadm --manage /dev/md0 --remove /dev/sdc1`
- RAID 5 - Striping with parity:
Lệnh để tạo RAID 5: 
```
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sda1 /dev/sdb1 /dev/sdc1
```
Lệnh để kiểm tra trạng thái RAID 5: `cat /proc/mdstat`  
Lệnh để xem thông tin chi tiết về RAID 5: `mdadm --detail /dev/md0`     
Lệnh để thêm ổ đĩa vào RAID 5: `mdadm --manage /dev/md0 --add /dev/sdd1`    
Lệnh để xóa ổ đĩa khỏi RAID 5: `mdadm --manage /dev/md0 --remove /dev/sdd1`
- RAID 6 - Striping with double parity:
Lệnh để tạo RAID 6: 
```
mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1
```
Lệnh để kiểm tra trạng thái RAID 6: `cat /proc/mdstat`  
Lệnh để xem thông tin chi tiết về RAID 6: `mdadm --detail /dev/md0`     
Lệnh để thêm ổ đĩa vào RAID 6: `mdadm --manage /dev/md0 --add /dev/sde1`    
Lệnh để xóa ổ đĩa khỏi RAID 6: `mdadm --manage /dev/md0 --remove /dev/sde1`

- RAID 10 - Mirrored Striping:
Lệnh để tạo RAID 10: 
```
mdadm --create /dev/md0 --level=10 --raid-devices=4 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1
```
Lệnh để kiểm tra trạng thái RAID 10: `cat /proc/mdstat`     
Lệnh để xem thông tin chi tiết về RAID 10: `mdadm --detail /dev/md0`        
Lệnh để thêm ổ đĩa vào RAID 10: `mdadm --manage /dev/md0 --add /dev/sde1`       
Lệnh để xóa ổ đĩa khỏi RAID 10: `mdadm --manage /dev/md0 --remove /dev/sde1`

### LAB Raid decive cũ là 1GB giờ nâng cái raid device md127 lên 2G mà bảo toàn dữ liệu
Bước 1 tạo thêm ổ đĩa. Tạo phân vùng mới bằng với kích thước phân vùng hiện tại đang RAID


Bước 2 Thêm phân vùng sdd1 vào RAID md127: sử dụng lệnh `mdadm --add /dev/md127 /dev/sdd1` để thêm phân vùng sdd1 vào RAID md127.