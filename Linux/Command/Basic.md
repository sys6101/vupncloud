# Lệnh Linux cơ bản     

1. `pwd`    
Lệnh `pwd` dùng để tìm ra đường dẫn thư mục làm việc hiện tại   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/pwd.png) 
2. `ls`     
Lệnh `ls` được sử dụng để xem nội dung của một thư mục. Theo mặc định lệnh này sẽ hiển thị nội dung của thư mục làm việc hiện tại.  
Nếu muốn xem nội dung của thư mục khác thì nhập lệnh   `ls` rồi **đường dẫn của thư mục**   
- `ls -R` cũng sẽ liệt kê tất cả các tệp trong các thư mục con    
- `ls -a` sẽ hiển thị các tệp ẩn  
- `ls -al` sẽ liệt kê các tệp và thư mục với thông tin chi tiết như quyền, kích thước, chủ sở hữu, v.v.   
   
3. `cat`    
Đây là một trong những lệnh sử dụng thường xuyên trong Linux.   
Nó dùng để liệt kê nội dung của một tệp trên đầu ra chuẩn(sdout).   
Một số cách dùng `cat`  
- `cat > filename` để tạo một tệp mới tên là `filename`     
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/cat1.png)  

- Chuyển đổi một tệp sang sử dụng chữ hoa hoặc chữ thường `cat filename | tr a-z A-Z>output.txt`( "tr" là viết tắt của từ "translate" trong Unix )  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/cat2.png)

4. `cp` 
Sử dụng lệnh `cp` để sao chép từ thư mục hiện tại sang thư mục khác.    
Vd: `cp test.txt /home/vupn/Documents` sẽ tạo bản sao của test.txt từ thư mục hiện tại qua thư mục Documents 



5. `mv` 
Công dụng chính là di chuyển tệp ngoài ra còn có thể đổi tên tệp    
Vd: `cp test.txt /home/vupn/Documents` sẽ di chuyển  test.txt từ thư mục hiện tại qua thư mục Documents     
Để đổi tên tệp, lệnh Linux là `mv ten-cu.ext ten-moi.ext`   

6. `mkdir`  
Dùng để  **tạo một thư mục mới**    
Ngoài ra còn có thể tạo thư mục mới bên trong một thư mục khác `mkdir Documents/Newfile`    
`mkdir -p Documents/test/Newfile` tuỳ chọn `p` (viết tắt của parents) để tạo một thư mục giữ hai thư mục hiện có    

7. `rmdir`     
Sử dụng để xóa các thư mục trống    

8. `rm`     
Sử dụng để xóa các thư mục và nội dung bên trong chúng `rm -r`  
Có thể sử dụng lệnh `rm` mà không sử dụng tùy chọn `-r`. Trong trường hợp này, lệnh `rm` sẽ xóa tệp tin được chỉ định, nhưng nếu đó là một thư mục, lệnh sẽ không hoạt động và báo lỗi. 
Vì vậy, nếu bạn muốn xóa một tệp tin duy nhất, bạn có thể sử dụng lệnh `rm <tên tệp>` để xóa tệp đó. Tuy nhiên, nếu bạn muốn xóa một thư mục và tất cả các tệp tin và thư mục con của nó, bạn cần sử dụng tùy chọn `-r` để xóa đệ quy toàn bộ cây thư mục con.  

9. `touch`  
Lệnh `touch` cho phép bạn tạo một tệp mới trống thông qua dòng lệnh  
Vd: `touch /home/test.py` để tạo tệp python     

10. `locate`    
Dùng đẻ định vị tệp như lệnh tìm kiếm trong windows.    
Dùng đối số `-i` cùng với lệnh `locate` này sẽ làm cho nó ko phân  biệt chữ hoa chữ thường. 
Tìm kiếm một tệp chứ 2 từ trở lên thì dùng `*`  
Vd: `locate -i python*cat` sẽ tìm kiếm các tệp có chứ `python` và `cat` cho dù hoa hay thường. 

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/locate.png)

11. `find`  
Tương tự như lệnh `locate` chỉ khác là sử dụng để định các tệ trong một thư mục nhất định   
Các biến thể khác khi sử dụng tìm kiếm là:  

 
- Để tìm các tệp trong thư mục sử dụng hiện tại, hãy sử dụng `find -name notes.txt` 
- Để tìm kiếm thư mục sử dụng, `/ -type d -name notes.txt`  

12. `sudo`  
Là từ viết tắt của  **SuperUser Do** lệnh này cho phép bạn thực hiện các tác vụ yêu cầu quyền quản trị hoặc quyền root. 
Không nên sử dụng lệnh này hàng ngày vì có thể xảy ra lỗi nếu bạn làm sai.  




