# Để kiểm tra dung lượng đĩa và các thư mục / tệp tin, hai lệnh thường được sử dụng trên Linux là `df` và `du`
## 1. Lệnh `df` 
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/df1.png) 

Lệnh `df` được sử dụng để hiển thị thông tin về dung lượng đĩa và các phân vùng. Các tùy chọn thường được sử dụng với lệnh `df` là:

- `-h`: hiển thị thông tin với đơn vị dung lượng có ý nghĩa (KB, MB, GB,...)   

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/df2.png) 

- `-T`: hiển thị loại hệ thống tệp của các phân vùng   

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/df3.png) 

- `-i`: hiển thị thông tin inode 
   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/df4.png)   

Inode chứa các thông tin quan trọng về tệp, bao gồm:

Số hiệu inode: là một số duy nhất được gán cho mỗi tệp trong hệ thống tệp.
Các quyền truy cập tệp: bao gồm quyền đọc, ghi và thực thi.
Thông tin về người dùng và nhóm sở hữu tệp.
Kích thước tệp.
Ngày và giờ tạo, truy cập và sửa đổi tệp.
Địa chỉ vật lý của các khối dữ liệu tệp trên đĩa.   
## 2. Lệnh `du`
Lệnh `du` được sử dụng để hiển thị thông tin về dung lượng của các thư mục và tệp tin. Các tùy chọn thường được sử dụng với lệnh `du` là:

`-h`: hiển thị thông tin với đơn vị dung lượng có ý nghĩa (KB, MB, GB,...)  

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/du1.png)    

`-s`: chỉ hiển thị tổng dung lượng cho mỗi thư mục   

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/du2.png)    

`-c`: hiển thị tổng dung lượng cho tất cả các thư mục và tệp tin được chỉ định    

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/du3.png)  

`-x`: không hiển thị dung lượng của các tệp tin nằm trong các phân vùng khác nhau   
