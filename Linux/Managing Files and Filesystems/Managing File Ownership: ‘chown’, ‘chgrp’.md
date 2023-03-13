# Managing File Ownership: ‘chown’, ‘chgrp’
## chown
`chown` là lệnh được sử dụng để thay đổi chủ sở hữu của một tệp tịn hoặc thư mục. Chủ sở hữu của một tệp tin hoặc thư mục được xác định bởi một ID người dùng (UID) hoặc tên người dùng (username). Khi sử dụng lệnh `chown` bạn cần cung cấp UID hoặc tên người dùng mới cho tệp tin hoặc thư mục mà bạn muốn thay đổi chủ sở hữu.

Cú pháp:
```
chown [option] [UID|username] [đường dẫn đến tệp tin hoặc thư mục]
```
## chgrp
`chgrp` là lệnh dùng để thay đổi nhóm sở hữu của một tệp tin hoặc là thư mục. Nhóm sở hữu của một tệp tin hoặc thư mục được xác định bởi một ID nhóm (GID) hoặc tên nhóm (group name). Khi sử dụng lệnh chgrp, bạn cần cung cấp GID hoặc tên nhóm mới cho tệp tin hoặc thư mục mà bạn muốn thay đổi nhóm sở hữu.

Cú pháp:
```
chgrp [option] [UID|New group] [đường dẫn đến tệp tin hoặc thư mục]
```