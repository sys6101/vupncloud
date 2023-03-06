Cài đặt một gói phần mềm từ tệp tin .deb:


`sudo dpkg -i ten_goi_phat_hanh.deb`    
Xóa một gói phần mềm:   
`sudo dpkg -r ten_goi_phat_hanh`    
Hiển thị thông tin chi tiết về một gói phần mềm:

`dpkg -p ten_goi_phat_hanh`         
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/dkpg.png)    
Liệt kê tất cả các gói phần mềm đã cài đặt trên hệ thống:   

`dpkg -l`   
Sử dụng apt-get để quản lý Debian Package:  
Cập nhật danh sách các gói phần mềm có sẵn: 

`sudo apt-get update`   
Cài đặt một gói phần mềm:   

`sudo apt-get install ten_goi_phat_hanh`    
Xóa một gói phần mềm:

`sudo apt-get remove ten_goi_phat_hanh`  
Tìm kiếm gói phần mềm:  

`sudo apt-cache search ten_goi_phat_hanh`   
Hiển thị thông tin chi tiết về một gói phần mềm:    

`sudo apt-cache show ten_goi_phat_hanh`     
Cài đặt phần mềm trên Ubuntu sử dụng dpkg và apt-get:   
Tải về tệp tin .deb của phần mềm cần cài đặt.   
Sử dụng lệnh dpkg để cài đặt phần mềm:  

`sudo dpkg -i ten_goi_phat_hanh.deb`    
Nếu có các phụ thuộc của phần mềm cần cài đặt, sử dụng lệnh apt-get để cài đặt các phụ thuộc đó:

`sudo apt-get -f install`