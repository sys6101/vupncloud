## Standard streams
**Trong Linux, có 3 loại streams chuẩn được sử dụng để xử lý thông tin:** 

1. Standard input (stdin): Đây là stream chuẩn để đọc dữ liệu từ bàn phím hoặc từ một tệp. Khi một chương trình đang chạy, nó sẽ yêu cầu dữ liệu nhập vào từ người dùng thông qua stdin.

2. Standard output (stdout): Đây là stream chuẩn để ghi dữ liệu ra màn hình hoặc ghi vào một tệp. Khi một chương trình hoàn thành công việc của nó, nó sẽ gửi kết quả đầu ra của nó thông qua stdout.

3. tandard error (stderr): Đây là stream chuẩn để ghi các thông báo lỗi và cảnh báo. Nó cũng có thể ghi vào màn hình hoặc vào một tệp riêng biệt.   

## Redirect Input và Output, cách Piping Data giữa các Programs
Trong Linux, Redirect Input và Output và Piping Data giữa các Programs là những kỹ thuật sử dụng trong command line để xử lý và chuyển tiếp dữ liệu giữa các chương trình.  
1. Redirect Input và Output:

- Redirect Input: Sử dụng ký tự "`<`" để chuyển hướng dữ liệu đầu vào từ một tệp hoặc từ một chương trình khác. Ví dụ: `command < input.txt` sẽ đọc dữ liệu từ tệp `input.txt`à đưa nó vào chương trình command.

- Redirect Output: Sử dụng ký tự "`>`" để chuyển hướng dữ liệu đầu ra đến một tệp hoặc một chương trình khác. Ví dụ: `command > output.txt` sẽ ghi đầu ra của command vào tệp `output.txt`.

2. Piping Data:
- Piping Data giữa các Programs: Sử dụng ký tự "`|`" để chuyển tiếp dữ liệu đầu ra của một chương trình thành đầu vào của một chương trình khác. Ví dụ: `command1 | command2` sẽ lấy đầu ra của `command1` và chuyển tiếp nó làm đầu vào cho `command2`.    

Các kỹ thuật này thường được sử dụng để xử lý dữ liệu trên command line. Ví dụ, nếu muốn đếm số từ trong một tệp văn bản, ta có thể sử dụng lệnh `wc -w filename.txt` để đếm số từ trong tệp `filename.txt`. Ta cũng có thể sử dụng Piping Data để chuyển đầu ra của lệnh cat (đọc nội dung tệp) thành đầu vào cho lệnh `wc` để đếm số từ của tệp đó, như sau: `cat filename.txt | wc -w`