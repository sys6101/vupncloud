# Managing Users and Groups
## Các lệnh tạo, sửa user/group:

- `useradd`: tạo mới một user mới trong hệ thống
- `userdel`: xóa một user khỏi hệ thống
- `usermod`: sửa đổi các thông tin của user đã có trong hệ thống
- `groupadd`: tạo một nhóm mới
- `groupdel`: xóa một nhóm khỏi hệ thống
- `groupmod`: sửa đổi các thông tin của một nhóm đã có trong hệ thống
## Các option quan trọng:
`-m`: tạo thư mục home cho user khi sử dụng lệnh `useradd`  
`-g`: chỉ định nhóm mặc định cho user mới   
`-G`: thêm user vào một hoặc nhiều nhóm 
`-l`: chỉ định tên đăng nhập cho user khi sử dụng lệnh `useradd`    
`-p`: thiết lập mật khẩu cho user khi sử dụng lệnh useradd  
`-r`: tạo một user hệ thống 
## Phân biệt su, sudo, các file /etc/passwd, /etc/groups, /etc/sudoer:
1. `su`: cho phép đăng nhập vào một tài khoản khác, bao gồm cả tài khoản root. Khi sử dụng lệnh này, người dùng phải nhập mật khẩu của tài khoản muốn đăng nhập vào.     
2. `sudo`: cho phép người dùng thực hiện các lệnh với đặc quyền root hoặc tài khoản khác có đặc quyền, mà không cần phải đăng nhập trực tiếp vào tài khoản đó. Người dùng phải được phân quyền truy cập vào các lệnh đó bằng cách được thêm vào file `/etc/sudoers` hoặc các file cấu hình sudo tương tự.
3. `/etc/passwd`: là một file văn bản lưu trữ thông tin về tất cả các tài khoản user trong hệ thống. Nó bao gồm tên đăng nhập, UID, GID, thư mục home, shell mặc định và các thông tin khác của user.
4. `/etc/group`: là một file văn bản lưu trữ thông tin về tất cả các nhóm trong hệ thống. Nó bao gồm tên nhóm, GID và danh sách các thành viên của nhóm đó.
5. `/etc/sudoers`: là file cấu hình cho phép hoặc không cho phép người dùng sử dụng lệnh `sudo`. File này có thể chỉnh sửa bằng lệnh `visudo`.

## Ví dụ

Thêm user vào nhiều nhóm cùng lúc
```
sudo usermod -a -G group1,group2,group3 newuser
```
Sửa đổi tên đăng nhập của user:
```
sudo usermod -l newname oldname
```
Xóa nhóm
```
sudo groupdel groupname
```
Sử dụng visudo để chỉnh sửa file /etc/sudoers:
```
sudo visudo
```
----------------------------
v..v..v..v..v..
