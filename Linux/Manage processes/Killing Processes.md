Để kill một process trên Linux chúng ta sử dụng lệnh `kill` Lệnh này sẽ gửi một tín hiệu(signal) đến một tiến trình , yêu cầu nó đừng lại. Có một số loại signal được sử dụng để kill process:
- `SIGTERM`(signal 15): Đây là tín hiệu mặc định được sử dụng bởi lệnh `kill`. Nó yêu cầu tiến trình dừng lại và thoát. Tiên trình vẫn có thể chặn signal này và không thoát
- `SIGKILL`(signal 9): Đây là tín hiệu mạnh nhất và sẽ buộc tiến trình dừng lại ngay lập tức mà không cho phép nó chặn hay xử lí tín hiệu này.Lưu ý signal này có thể làm hỏng dữ liệu hoặc gây ra tính huống bất ngờ
- `SIGINT`(signal 2): Đây là tín hiệu được gửi khi người dùng nhấn Ctrl-C trên bàn phím.Nó cho phép tiến trình hoàn thành các công việc cần thiết trước khi thoát
- `SIGQUIT`(signal 3)" Đây là tín hiệu được gửi khi người dùng nhấn  Ctrl-\ trên bàn phím . Nó yêu cầu tiến trình dưng lại và hiển thị thông tin gỡ lỗi


Ngoài lệnh kill, trong Linux còn có hai lệnh khác để kết thúc các tiến trình là `killall` và `pkill`.

`killall` cho phép kết thúc tất cả các tiến trình có tên cụ thể. Cú pháp của lệnh `killall` là:

```
killall [options] <process_name>
```

Lệnh `pkill` cũng tương tự như lệnh `killall` nhưng cho phép kết thúc các tiến trình dựa trên một biểu thức chính quy. Cú pháp của lệnh `pkill` là:

```
pkill [options] <pattern>
```

