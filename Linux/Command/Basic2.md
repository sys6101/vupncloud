## Grep

Nó khá hữu ích cho viêc sử dụng hàng ngày
Nó cho phép tìm kiếm qua tất cả văn bản trong một tệp nhất định  
Cú pháp: `grep 'word' [file]`   
Vd: `grep cloud test.txt` sẽ tìm kiếm từ `cloud` trong tệp `test.txt`  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep1.png)

- `-r` tìm kiếm tất cả các file trong thư mục và thư mục con hiện tại   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep2.png)
- `-i` tìm theo cả trường hợp ký tự viết hoa
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep3.png)
- `-c` số lần xuất hiện của từ khoá   
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep4.png)
- `-n` từ khoá xuất hiện dòng số bao nhiêu    
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep5.png)
- `-v` in các dòng mà **không có** từ khoá  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep6.png)
- `-w` chỉ in dòng có đúng từ khoá
  - `grep -w 'test' test.txt` sẽ chỉ tìm theo đúng từ _test_ chứ không có _tester..._   
  ![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/grep7.png)


- `--color` tô màu từ khoá trong kết quả

## sed

Lệnh "sed" (viết tắt của Stream Editor) là một công cụ dòng lệnh có sẵn trong Unix và các hệ điều hành tương tự, được sử dụng để xử lý và biên tập văn bản đầu vào theo các quy tắc được xác định trước.
Nó được dùng để lọc văn bản , sửa đổi nội dung file, đặt nội dung vào một file mới.   
`sed 's/pattern/replacement/g' inputfile > outputfile`  
Trong đó:
`s`  được sử dụng để chỉ định cho sed rằng bạn muốn thực hiện một thay thế chuỗi (string substitution)  
`pattern` là chuỗi hoặc biểu thức chính quy (regex) mà bạn muốn tìm kiếm trong đầu vào.
`replacement` là chuỗi mà bạn muốn thay thế cho `pattern`.  
`g` là tùy chọn toàn cục (global) để thay thế tất cả các trường hợp của `pattern` trong mỗi dòng. Nếu bạn muốn chỉ thay thế trường hợp đầu tiên, bạn có thể bỏ tùy chọn này.  

## join

Dùng để  kết hợp 2 tệp dữ liệu dựa trên một trường chung. Kết quả đàu ra sẽ bao gồm các dòng chưa các giá trị từ hai tệp ban đầu đã kết hợp với nhau dựa trên trường chung.   
Cú pháp `join [options] filename1 filename2`    
Trong đó `options` là các tùy chọn để chỉ định các trường và cách kết hợp dữ liệu.    
Các tùy chọn phổ biến của lệnh `join`
- `-a filenum`: kết hợp các dòng của file1 và hoặc file 2
- `-t char`: sử dụng ký tự char như là ký tự phân tách trường ( mặc định là dấu cách)
-  `-1 FIELD`: Chỉ định trường trong file1 để kết hợp (FIELD là số thứ tự của trường).
- `-2 FIELD`: Chỉ định trường trong file2 để kết hợp (FIELD là số thứ tự của trường).

##  paste

Dùng để ghép 1 file với các file khác theo từng cột dữ liệu  
Cú pháp `paste [options] file1 file2`   
`-d` chọn ký tự giữa các cột thay vì `tab` ở mặc định   

## awk

Lệnh `awk` là một công cụ xử lý văn bản mạnh mẽ trên hệ điều hành Linux. Nó cho phép bạn tìm kiếm, sắp xếp, xử lý và hiển thị dữ liệu từ các tệp tin hoặc đầu vào chuẩn. Dưới đây là một số ví dụ về cách sử dụng lệnh `awk`:

- Hiển thị toàn bộ nội dung của tệp tin:

  `awk '{print}' filename`

- Hiển thị tất cả các dòng chứa từ khóa "pattern" trong tệp tin:

  `awk '/pattern/ {print}' filename`

- Hiển thị số lượng các dòng trong tệp tin:

  `awk 'END {print NR}' filename`

- Hiển thị nội dung của cột thứ hai trong tệp tin, ngăn cách bởi dấu phẩy:

  `awk -F',' '{print $2}' filename`

- Tính tổng các số trong tệp tin:

  `awk '{sum += $1} END {print sum}' filename`

- Hiển thị dòng đầu tiên của tệp tin:

  `awk 'NR==1 {print}' filename`

- Sắp xếp các dòng trong tệp tin theo thứ tự tăng dần của cột thứ hai:

  `awk '{print $0 | "sort -n -k2"}' filename`

- Hiển thị tất cả các dòng có độ dài lớn hơn 80 ký tự trong tệp tin:

  `awk 'length > 80 {print}' filename`


## Các lệnh ‘tac’, ‘sort’, ‘split’, ‘uniq’, ‘nl’

- Lệnh `tac` dùng để đảo ngược thứ tự các dòng của một tệp văn bản. VD: `tac filename.txt` sẽ in ra các dòng của tệp theo thứ tự ngược lại    
- Lệnh `sort` dùng để sắp xếp  các dòng của một tệp văn bản.    
- Lệnh `split` dùng để chia nhỏ một tệp văn bản thành các tệp nhỏ hơn dựa trên số lượng dòng hoặc kích thước. vd: `split -l 100 filename --verbose thông báo sau khi tách` sẽ chia tệp thành các tệp nhỏ hơn có 100 dòng mỗi tệp.Trong đó --vervose là thông báo sau khi tách   
- Lệnh `uniq` để loaij bỏ các dòng trùng lặp trong một tệp văn bản .    
- lệnh `nl` dùng để đánh số thứ tự các dòng của một tệp văn bản.  
