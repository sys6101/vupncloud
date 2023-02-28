# Lệnh Linux cơ bản     

1. `pwd`    
Lệnh `pwd` dùng để tùm ra đường dẫn thư mục làm việc hiện tại   

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
- `cat filename1 filename2>filename3` kết hợp tệp 1 và 2 lưu trữ kết quả vào tệp 3  
- Chuyển đổi một tệp sang sử dụng chữ hoa hoặc chữ thường `cat filename | tr a-z A-Z>output.txt`( "tr" là viết tắt của từ "translate" trong Unix )  

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
Có thể sử dụng lệnh "rm" mà không sử dụng tùy chọn "-r". Trong trường hợp này, lệnh "rm" sẽ xóa tệp tin được chỉ định, nhưng nếu đó là một thư mục, lệnh sẽ không hoạt động và báo lỗi. 

Vì vậy, nếu bạn muốn xóa một tệp tin duy nhất, bạn có thể sử dụng lệnh "rm <tên tệp>" để xóa tệp đó. Tuy nhiên, nếu bạn muốn xóa một thư mục và tất cả các tệp tin và thư mục con của nó, bạn cần sử dụng tùy chọn "-r" để xóa đệ quy toàn bộ cây thư mục con.  

9. 


