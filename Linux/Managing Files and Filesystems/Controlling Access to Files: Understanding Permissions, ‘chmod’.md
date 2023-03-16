# Controlling Access to Files: Understanding Permissions, ‘chmod’
 ## chmod 
 `chmod` dùng để thay đổi quyền truy cập cho nhiều tệp tin hoặc thư mục bằng cách sử dụng ký hiệu đại diện cho các tệp tin hoặc thư mục. 

 ![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/chmod1.png)



| Giá trị                           | Quyền truy cập                 | 
| --------------------------------- | -------------------------------| 
| 0                                 | Không quyền truy cập           |
| 1                                 | Quyền thực thi                 | 
| 2                                 | Quyền ghi                      | 
| 3                                 | Quyền ghi và thực thi          | 
| 4                                 | Quyền đọc                      | 
| 5                                 | `Quyền đọc và thực thi         | 
| 6                                 | Quyền đọc và ghi               | 
| 7                                 | Quyền đọc, ghi và thực thi     | 



**Hoặc sử dụng các ký tự đại diện**

- u (user) : biểu thị chủ sở hữu của tệp tin hoặc thư mục.
- g (group) : biểu thị nhóm sở hữu của tệp tin hoặc thư mục.
- o (others) : biểu thị người dùng khác.
- a (all) : biểu thị tất cả các đối tượng trên (bao gồm u, g và o).
- đọc (r), ghi (w) và thực thi (x)

```
chmod u=rwx,g=rx,o= /path/to/file
```