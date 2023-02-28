# Biến môi trường  

Biến môi trường Linux được các ứng dụng sử dụng để lấy thông tin về môi trường và mỗi biến môi trường là một biến có tên và giá trị liên quan.  
Các biến có định dạng sau và theo quy ước có tên viết hoa. Mặc dù chúng phân biệt chữ hoa chữ thường, vì vậy có thể có tên chữ thường. Ngoài ra, không có khoảng trống xung quanh biểu tượng bằng =.    
## Hiển thị     
`env` – Điều này cho phép bạn chạy một chương trình khác trong môi trường tùy chỉnh mà không sửa đổi chương trình hiện tại. Khi được sử dụng mà không có đối số, nó sẽ in danh sách các biến môi trường hiện tại.

`printenv` – Thao tác này in tất cả các biến môi trường đã chỉ định. Ví dụ: để hiển thị giá trị của loại biến môi trường HOME   
Xem các biến môi được thiết lập : `printenv`    
Hiển thị giá trị của một biến cụ thể vd: 
``` 
echo $HOME
/home/vupn  
``` 
## Gán biến
Gán biến tạm thời, reboot sẽ mất: 
```
VUPN=value123
echo $VUPN
value123
```

Để đảm bảo các biến môi trường này tồn tại vĩnh viễn thì cần phải đặt vào file cấu hình phù hợp.
- “/etc/environment” cho các biến trên toàn hệ thống
- “/etc/profile” dành cho thiết lập các biến shell.
- “~/.bashrc” dành cho mục đích các cá nhân
