# Backup System: ‘rsnapshot’, ‘tar’, ‘dd’, ‘rsync’
## Rsync
`rsync` là một công cụ dùng để sao chép và đồng bộ hóa dữ liệu giữa hai thư mục , hai máy chủ hoặc giữa máy chủ và thiết bị lưu trữ bên  ngoài. 

Cú pháp cơ bản :
```
rsync [options] [source] [destination]

```
- `options`: Các tùy chọn cho việc sao chép.Ví dụ:
  -  `-a` cho archive mode  
    - `-v` cho verbose output     
    - `-z` cho nén dữ liệu truyền     
    - `-e` ssh để sử dụng SSH cho kết nối từ xa, v.v.).
- `source`: Thư mục nguồn cần sao lưu.
- `destination`: Thư mục đích nơi dữ liệu được sao chép đến.

Ví dụ sao lưu dữ liệu từ thư mục source_folder sang thư mục backup_folder trên cùng một máy:
```
rsync -avz Documents/ Downloads/

```
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Linux/rsync1.png)
## Rsnapshot
`rsnapshot` là một chương trình sao lưu dựa trên rsync, cho phép sao lưu dữ liệu và tạo các bản sao lưu theo thời gian (incremental backups) một cách hiệu quả. Rsnapshot tận dụng khả năng hard linking của hệ thống tập tin, giúp tiết kiệm không gian lưu trữ khi lưu nhiều bản sao lưu.     
Để sử dụng rsnapshot, bạn cần cài đặt và cấu hình nó:

Cài đặt `rsnapshot` từ kho lưu trữ của hệ điều hành (ví dụ: `apt-get install rsnapshot` trên Debian/Ubuntu).

Chỉnh sửa tệp cấu hình `/etc/rsnapshot.conf` và đặt các tham số như `snapshot_root, cmd_rsync, cmd_ssh, interval`, v.v. Ngoài ra, bạn cần thêm các đường dẫn đến thư mục nguồn và đích cho việc sao lưu.

Sau khi cấu hình xong, bạn có thể chạy `rsnapshot` với các mức sao lưu được định nghĩa trong tệp cấu hình, chẳng hạn như `hourly, daily, weekly, monthly`. Ví dụ:
```
sudo rsnapshot hourly

```
## tar và dd
`tar`: Là một công cụ nén dữ liệu trên Unix, tar thường được sử dụng để nén và giải nén các tập tin và thư mục. Nó cũng có thể được sử dụng để sao lưu toàn bộ hệ thống hoặc chỉ các tập tin cụ thể. `tar` tạo ra các bản sao lưu bằng cách nén tập tin và các thư mục thành một tệp duy nhất và giữ lại các thông tin về quyền truy cập và thời gian tạo.

`dd`: Là một công cụ được sử dụng để sao chép dữ liệu từ một ổ đĩa sang một ổ đĩa khác hoặc tạo ra các bản sao lưu toàn bộ của hệ thống. dd có thể sao lưu toàn bộ hệ thống bằng cách sao chép toàn bộ phân vùng hoặc ổ đĩa. Nó cũng có thể được sử dụng để tạo ra các bản sao lưu của các phân vùng cụ thể hoặc các tập tin.