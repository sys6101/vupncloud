**Trong Linux, các lệnh xem nội dung tệp tin (file-viewing commands) như `head, tail, less, cut và wc` được sử dụng để xem và xử lý nội dung của các tệp tin. Dưới đây là giải thích chi tiết về mỗi lệnh:**

- Lệnh `head`: Sử dụng lệnh `head` để hiển thị một số dòng đầu tiên của một tệp văn bản. Cú pháp của lệnh như sau: `head [options] [filename]`. Ví dụ, để hiển thị 10 dòng đầu tiên của tệp `filename.txt`, ta có thể sử dụng lệnh `head -n 10 filename.txt`.   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/head.png)

- Lệnh `tail`: Sử dụng lệnh `tail` để hiển thị một số dòng cuối cùng của một tệp văn bản. Cú pháp của lệnh như sau: `tail [options] [filename]`. Ví dụ, để hiển thị 10 dòng cuối cùng của tệp `filename.txt`, ta có thể sử dụng lệnh `tail -n 10 filename.txt`. 
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/tail.png)

- Lệnh `less`: Sử dụng lệnh `less` để xem nội dung của một tệp văn bản và cho phép người dùng cuộn trang lên/xuống để xem toàn bộ nội dung. Cú pháp của lệnh như sau: `less [options] [filename]`. Ví dụ, để xem nội dung của tệp `filename.txt` sử dụng lệnh `less filename.txt`.  


- Lệnh `cut`: Sử dụng lệnh `cut` để cắt bỏ các trường không cần thiết trong một tệp văn bản. Cú pháp của lệnh như sau: cut [options] [filename]. Ví dụ, để cắt bỏ trường thứ hai drong các dòng của tệp `filename.txt`, ta có thể sử dụng lệnh `cut -f 2 --complement filename.txt`.

- Lệnh `wc`: Sử dụng lệnh `wc` để đếm số dòng, số từ, số ký tự trong một tệp văn bản. Cú pháp của lệnh như sau: `wc [options] [filename]`. Ví dụ, để đếm số dòng, số từ và số ký tự trong tệp `filename.txt`.   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/wc.png)