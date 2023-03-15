# Using System Log Files: syslogd, rotating log files
Trong hệ thống Unix và Linux, syslogd là một tiến trình hệ thống quản lý các thông điệp hệ thống, bao gồm các thông báo lỗi, thông tin khởi động và tắt máy, cũng như các thông tin về hoạt động hệ thống. syslogd nhận thông điệp từ các ứng dụng và tiến trình khác trên hệ thống và ghi chúng vào các file nhật ký(log files).

 Một số tệp nhật ký quan trọng và thường được sử dụng trong hệ thống Unix và Linux bao gồm:

`/var/log/messages`: chứa các thông điệp hệ thống chung.    
`/var/log/auth.log`: chứa các thông điệp về xác thực và quyền truy cập của người dùng.  
`/var/log/syslog`: chứa các thông điệp hệ thống từ các ứng dụng và tiến trình khác. 
`/var/log/kern.log`: chứa các thông điệp liên quan đến kernel của hệ thống. 
`/var/log/cron.log`: chứa các thông điệp liên quan đến việc lập lịch và thực thi các công việc định kỳ.