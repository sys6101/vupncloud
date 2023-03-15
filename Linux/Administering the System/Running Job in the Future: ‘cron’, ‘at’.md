# Running Job in the Future: ‘cron’, ‘at’

## cron
Cron là một tiện ích dành cho hệ điều hành Unix/Linux, cho phép lập lịch chạy các công việc (commands hoặc scripts) tự động tại các khoảng thời gian định kỳ. Nó rất hữu ích khi bạn muốn tự động thực hiện các tác vụ như sao lưu dữ liệu, cập nhật hệ thống, gửi email thông báo, v.v.

Gõ lệnh `crontab -e` để mở tệp cấu hình cron cho người dùng hiện tại. Nếu đây là lần đầu tiên bạn sử dụng crontab, hệ thống có thể yêu cầu bạn chọn trình soạn thảo văn bản (ví dụ: nano, vim).



Thêm lệnh cron theo định dạng trên. Ví dụ, để chạy một script `backup.sh` vào lúc 2 giờ sáng hàng ngày, bạn có thể thêm dòng sau:
```
0 2 * * * /path/to/backup.sh
```

- 0: Phút (0-59). Trong trường hợp này, giá trị là 0, nghĩa là chạy vào đầu giờ.
- 2: Giờ (0-23). Giá trị là 2, nghĩa là chạy vào lúc 2 giờ sáng.
- *: Ngày trong tháng (1-31). Dấu * nghĩa là không giới hạn, tức là chạy hàng ngày.
- *: Tháng (1-12). Dấu * nghĩa là không giới hạn, tức là chạy hàng tháng.
- *: Ngày trong tuần (0-7), trong đó 0 và 7 đều đại diện cho Chủ Nhật. Dấu * nghĩa là không giới hạn, tức là chạy hàng tuần.
- /path/to/backup.sh: Đường dẫn đến script bạn muốn chạy. 
## at
`at`: Được sử dụng để lập lịch chạy một công việc duy nhất vào một thời điểm xác định trong tương lai. At thích hợp cho các tác vụ không định kỳ, không cần lặp lại, hoặc cần chạy tại một thời điểm cụ thể. Để sử dụng at, bạn cần cài đặt tiện ích at (nếu chưa có) và sau đó sử dụng lệnh at kèm theo thời gian chạy mong muốn.

Để chạy một script `one-time-job.sh` vào lúc 16:00 ngày hôm nay, bạn có thể sử dụng lệnh sau:
```
at 16:00
```
Sau khi gõ lệnh trên, bạn sẽ được chuyển đến màn hình nhập công việc. Gõ đường dẫn đến script bạn muốn chạy:
```
at Wed Mar 15 16:00:00 2023
at> 
```
Ấn `Ctrl+D` để thoát và lưu lịch trình.