**Trong Linux, để tạo một hệ thống tệp trên một phân vùng định dạng, bạn có thể sử dụng lệnh `mkfs` (make file system). Lệnh này cho phép bạn tạo các loại hệ thống tệp phổ biến như ext2, ext3, ext4, XFS, NTFS vv.**

Cú pháp của lệnh `mkfs` như sau:



`mkfs.[loại_hệ_thống_tệp] [tùy_chọn] [đường_dẫn_đến_phân_vùng]`       
Ví dụ, để tạo một hệ thống tệp ext4 trên phân vùng /dev/sdb1, bạn có thể sử dụng lệnh sau:



`sudo mkfs.ext4 /dev/sdb1`    
Các tùy chọn thường được sử dụng với lệnh `mkfs` bao gồm:

`-t, --type [loại_hệ_thống_tệp]`: chỉ định loại hệ thống tệp muốn tạo.  
`-c, --check`: kiểm tra phâ n vùng trước khi tạo hệ thống tệp.  
`-L, --label [nhãn]`: đặt nhãn cho hệ thống tệp.    
`-v, --verbose`: hiển thị thông tin chi tiết hơn về quá trình tạo hệ thống tệp.