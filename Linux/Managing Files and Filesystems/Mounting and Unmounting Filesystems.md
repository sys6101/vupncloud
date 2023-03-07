### Trong Linux, để truy cập vào một phân vùng định dạng, bạn cần phải mount phân vùng đó vào một thư mục trên hệ thống tệp. Khi phân vùng được mount, nó sẽ xuất hiện trong cây thư mục hệ thống tệp và bạn có thể truy cập vào các tệp và thư mục trong đó.

Để mount một phân vùng định dạng, bạn có thể sử dụng lệnh `mount`. Cú pháp của lệnh này như sau:



```
sudo mount [tùy_chọn] [đường_dẫn_đến_phân_vùng] [đường_dẫn_đến_thư_mục_mount]
```   
Ví dụ, để mount phân vùng /dev/sdb1 vào thư mục /mnt, bạn có thể sử dụng lệnh sau:



```
sudo mount /dev/sdb1 /mnt
``` 
Khi phân vùng được mount, bạn có thể truy cập nó thông qua thư mục /mnt.

Ngoài ra, để unmount một phân vùng đang được mount, bạn có thể sử dụng lệnh `umount`. Cú pháp của lệnh này như sau:


```
sudo umount [đường_dẫn_đến_thư_mục_mount]
```

Ví dụ, để unmount phân vùng được mount trên thư mục /mnt, bạn có thể sử dụng lệnh sau:


```
sudo umount /mnt
``` 

Lưu ý rằng khi một phân vùng đang được sử dụng (chẳng hạn như đang được chạy một hệ thống tệp), bạn không thể unmount phân vùng đó.         
Để unmount phân vùng đó, bạn cần phải dừng các tiến trình liên quan đến phân vùng đó trước khi thực hiện lệnh `umount`. Nếu bạn không dừng các tiến trình này, bạn có thể gây ra các lỗi hệ thống và dữ liệu có thể bị mất hoặc bị hỏng.