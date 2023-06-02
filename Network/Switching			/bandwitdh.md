# Bandwidth
 Lệnh tc (Traffic Control) được sử dụng trong Linux để quản lý và cấu hình các cơ chế kiểm soát lưu lượng mạng
 ```
 sudo tc qdisc add dev eth1 root handle 1: htb default 12
sudo tc class add dev eth1 parent 1:1 classid 1:12 htb rate 1Gbit ceil 1Gbit
 ```

 Hai lệnh trên được sử dụng để tạo và cấu hình một lớp kiểm soát băng thông trên giao diện mạng eth1.

- sudo tc qdisc add dev eth1 root handle 1: htb default 12:

    sudo: chạy lệnh với quyền quản trị.     
    tc qdisc add: tạo một hàng đợi mới (queueing discipline).       
    dev eth1: trên giao diện mạng eth1.     
    root: ở vị trí root của cây hàng đợi.       
    handle 1:: gán ID 1: cho hàng đợi này để sau này có thể tham chiếu đến.         
    htb: sử dụng Hierarchy Token Bucket (HTB), một thuật toán cho phép kiểm soát băng thông theo cấp độ hiệu suất mong muốn.        
    default 12: khi một gói dữ liệu không khớp với bất kỳ lớp nào, nó sẽ được chuyển vào lớp mặc định 1:12.         

- sudo tc class add dev eth1 parent 1:1 classid 1:12 htb rate 1Gbit ceil 1Gbit:

    sudo: chạy lệnh với quyền quản trị.     
    tc class add: thêm một lớp mới vào hàng đợi.        
    dev eth1: trên giao diện mạng eth1.     
    parent 1:1: lớp này là con của lớp 1:1.     
    classid 1:12: gán ID 1:12 cho lớp này để sau này có thể tham chiếu đến.     
    htb: sử dụng thuật toán HTB cho lớp này.        
    rate 1Gbit: tốc độ băng thông tối thiểu đảm bảo cho lớp này là 1 Gbit/s.        
    ceil 1Gbit: tốc độ băng thông tối đa mà lớp này có thể sử dụng khi có băng thông không sử dụng từ các lớp khác là 1 Gbit/s.     