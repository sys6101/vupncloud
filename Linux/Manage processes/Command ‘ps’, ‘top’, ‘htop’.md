**Dưới đây là các lệnh cơ bản để xem thông tin về CPU, RAM và quản lý các tiến trình trên hệ thống Linux:**

- Lệnh `top`: Hiển thị thông tin về tải CPU, RAM và danh sách các tiến trình đang chạy. Bạn có thể sử dụng các phím tắt trong top để sắp xếp và lọc danh sách các tiến trình.       
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/top.png)     
Trong đó:   
  - `PID`: `ID` của tiến trình.
  - `USER`: Tên người dùng chạy tiến trình đó.
  - `%CPU`: Tải `CPU` hiện tại của tiến trình.
  - `%MEM`: Tải `RAM` hiện tại của tiến trình.
  - `TIME+`: Tổng thời gian `CPU` đã sử dụng bởi tiến trình đó.
  - `COMMAND`: Tên của tiến trình.
- Lệnh `htop`: Tương tự như top nhưng cung cấp giao diện đồ họa và cho phép bạn sắp xếp và lọc danh sách các tiến trình một cách dễ dàng hơn.       
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/htop.png)

- Lệnh `free`: Hiển thị thông tin về dung lượng RAM tổng thể, dung lượng RAM đang sử dụng và dung lượng RAM còn trống.      
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/free.png)    
Trong đó:       
  - `total`: Tổng dung lượng của kênh bộ nhớ đó.
  - `used`: Dung lượng đang sử dụng của kênh bộ nhớ đó.
  - `free`: Dung lượng còn trống của kênh bộ nhớ đó.
  - `shared`: Dung lượng bộ nhớ được chia sẻ giữa các tiến trình.
  - `buff/cache`: Dung lượng bộ đệm hoặc cache.

- Lệnh `ps`: Hiển thị danh sách các tiến trình đang chạy trên hệ thống, bao gồm ID tiến trình, tên tiến trình và tải CPU và RAM của từng tiến trình.        
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/ps.png)  
Trong đó:   
  - `PID`: `ID` của tiến trình.
  - `TTY`: Đại diện cho Terminal Type
  - `TIME+`: Tổng thời gian `CPU` đã sử dụng bởi tiến trình đó.
  - `CMD`: Tên của tiến trình.

- Lệnh `kill`: Kết thúc một tiến trình cụ thể bằng cách sử dụng ID tiến trình hoặc tên tiến trình.