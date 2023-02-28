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
