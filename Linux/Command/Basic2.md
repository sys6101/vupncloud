## Grep

Nó khá hữu ích cho viêc sử dụng hàng ngày
Nó cho phép tìm kiếm qua tất cả văn bản trong một tệp nhất định  
Vd: `grep cloud test.txt` sẽ tìm kiếm từ `cloud` trong tệp `test.txt`  
Cú pháp: `grep 'word' [file]`

- `-r` tìm kiếm tất cả các file trong thư mục và thư mục con hiện tại
- `-i` tìm theo cả trường hợp ký tự viết hoa
- `-c` số lần xuất hiện của từ khoá
- `-n` từ khoá xuất hiện dòng số bao nhiêu
- `-v` in các dòng mà **không có** từ khoá
- `-w` chỉ in dòng có đúng từ khoá
  - `grep -w 'test' test.txt` sẽ chỉ tìm theo đúng từ _test_ chứ không có _tester..._
- `-l` chỉ hiện tên file có chứa từ khoá
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

