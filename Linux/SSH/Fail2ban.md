# Cấu hình bảo mật cho SSH bằng Fail2ban trên Ubuntu 22.04

h
## Giới thiệu

SSH là phương pháp kết nối trên thực tế với máy chủ đám mây. Nó bền và có thể mở rộng, khi các tiêu chuẩn mã hóa mới được phát triển, chúng có thể được sử dụng để tạo các SSH key mới, đảm bảo rằng giao thức cốt lõi vẫn an toàn. Tuy nhiên, không có giao thức hoặc phần mềm stack nào an toàn tuyệt đối, và SSH được triển khai rộng rãi trên internet có nghĩa là nó đại diện cho một sự tấn công bề mặt (attack surface) hoặc tấn công vector (attack vector), mà thông qua đó mọi người có thể truy cập.

Bất kỳ dịch vụ nào tiếp xúc với mạng Internet đều là mục tiêu tiềm năng của việc này. Nếu bạn xem lại nhật ký SSH service của mình chạy trên bất kỳ máy chủ nào, bạn sẽ thấy các nỗ lực đăng nhập có hệ thống, lặp đi lặp lại thể hiện các cuộc tấn công bạo lực của người dùng và bot là như nhau. Mặc dù bạn có thể thực hiện một số tối ưu hóa cho SSH service để giảm khả năng thành công của các cuộc tấn công này xuống gần 0, chẳng hạn như vô hiệu hóa xác thực mật khẩu có lợi cho các SSH key, chúng vẫn có thể đặt ra một trách nhiệm nhỏ.

Việc triển khai sản xuất quy mô lớn mà trách nhiệm này không được chấp nhận, thường sẽ thực hiện VPN như WireGuard trước SSH service của họ, do đó không thể kết nối trực tiếp với cổng SSH 22 mặc định từ internet bên ngoài mà không có phần mềm bổ sung hoặc các gateway. Các giải pháp VPN này được mọi người tin cậy, nhưng sẽ phức tạp và có thể phá vỡ một số quá trình tự động hóa hoặc các software hook nhỏ khác.

Trước hay ngoài việc cam kết thiết lập VPN đầy đủ, bạn có thể triển khai một công cụ có tên là **Fail2ban**. Fail2ban có thể giảm thiểu đáng kể các cuộc tấn công brute force bằng cách tạo các quy tắc tự động thay đổi cấu hình tường lửa của bạn, để cấm các IP cụ thể sau một số lần đăng nhập không thành công. Điều này sẽ cho phép máy chủ của bạn tự bảo vệ trước những nỗ lực truy cập này mà không cần sự can thiệp của bạn.

Trong hướng dẫn này, bạn sẽ thấy cách cài đặt và sử dụng Fail2ban trên máy chủ Ubuntu 22.04.

## Điều kiện

Để thực hiện theo hướng dẫn này, bạn sẽ cần có:

- Một máy chủ Ubuntu 22.04 và non-root user có đặc quyền sudo.
- Ngoài ra, máy chủ thứ hai mà bạn có thể kết nối với máy chủ đầu tiên của mình để sử dụng kiểm tra việc bị cấm.

## Bước 1 ****Cài đặt Fail2ban****

Fail2ban có sẵn trong kho phần mềm của Ubuntu. Bắt đầu bằng cách chạy các lệnh dưới đây với tư cách non-root user để cập nhật danh sách gói của bạn và cài đặt Fail2ban:

```
$ sudo apt update
$ sudo apt install fail2ban

```

Fail2ban sẽ tự động thiết lập một background service sau khi được cài đặt. Tuy nhiên, nó bị tắt theo mặc định, vì một số cài đặt mặc định của nó có thể gây ra các hiệu ứng không mong muốn. Bạn có thể xác minh điều này bằng cách sử dụng lệnh `systemctl`:

```
$ systemctl status fail2ban.service

```

```
Output
○ fail2ban.service - Fail2Ban Service
     Loaded: loaded (/lib/systemd/system/fail2ban.service; disabled; vendor preset: enabled
     Active: inactive (dead)
       Docs: man:fail2ban(1)

```

Bạn có thể bật Fail2ban ngay lập tức, nhưng trước tiên, bạn hãy xem xét một số tính năng của nó.

## **Bước 2: Cấu hình Fail2ban**

Fail2ban service giữ các tệp cấu hình của nó trong thư mục `/ etc / fail2ban`. Có một tệp với các giá trị mặc định được gọi là `jail.conf`. Hãy đi tới thư mục đó và in 20 dòng đầu tiên của tệp đó bằng `head -20`:

```
$ cd /etc/fail2ban
$ head -20 jail.conf

```

```
Output
#
# WARNING: heavily refactored in 0.9.0 release.  Please review and
#          customize settings for your setup.
#
# Changes:  in most of the cases you should not modify this
#           file, but provide customizations in jail.local file,
#           or separate .conf files under jail.d/ directory, e.g.:
#
# HOW TO ACTIVATE JAILS:
#
# YOU SHOULD NOT MODIFY THIS FILE.
#
# It will probably be overwritten or improved in a distribution update.
#
# Provide customizations in a jail.local file or a jail.d/customisation.local.
# For example to change the default bantime for all jails and to enable the
# ssh-iptables jail the following (uncommented) would appear in the .local file.
# See man 5 jail.conf for details.
#
# [DEFAULT]

```

Như bạn thấy, một số dòng đầu tiên của tệp này được **commented out** (ghi chú) - chúng bắt đầu bằng ký tự `#` cho biết rằng chúng sẽ được đọc dưới dạng tài liệu thay vì dưới dạng cài đặt. Và bạn có thể thấy, những ghi chú này hướng dẫn bạn không được trực tiếp sửa đổi tệp. Thay vào đó, bạn có hai lựa chọn: tạo hồ sơ riêng lẻ cho Fail2ban trong nhiều tệp ở thư mục `jail.d /`, hoặc tạo và thu thập tất cả cài đặt cục bộ của bạn trong tệp `jail.local`. Tệp `jail.conf` sẽ được cập nhật định kỳ khi Fail2ban được cập nhật và sẽ được sử dụng làm nguồn cài đặt mặc định mà bạn chưa tạo bất kỳ overrides (ghi đè) nào.

Trong hướng dẫn này, bạn sẽ tạo `jail.local`. Bạn có thể làm điều đó bằng cách sao chép `jail.conf`:

```
$ sudo cp jail.conf jail.local

```

Bây giờ bạn có thể bắt đầu thực hiện các thay đổi cấu hình. Mở tệp bằng `nano` hoặc trình soạn thảo văn bản yêu thích của bạn:

```
$ sudo vim jail.local

```

Trong khi bạn đang chuyển qua tệp, hướng dẫn này sẽ xem xét một số tùy chọn mà bạn có thể muốn cập nhật. Các cài đặt nằm trong phần `[DEFAULT]` gần đầu tệp sẽ được áp dụng cho tất cả các dịch vụ được Fail2ban hỗ trợ. Ở những nơi khác trong tệp, có các tiêu đề cho `[sshd]` và các dịch vụ khác, chứa các cài đặt dành riêng cho dịch vụ sẽ áp dụng thay thế các cài đặt mặc định.

```makefile
[DEFAULT]
. . .
bantime = 10m
. . .

```

Tham số `bantime` đặt khoảng thời gian mà khách hàng sẽ bị cấm khi họ không xác thực chính xác. Tham số này được đo bằng giây. Theo mặc định, thời gian sẽ được đặt thành 10 phút.

```
[DEFAULT]
. . .
findtime = 10m
maxretry = 5
. . .

```

Hai tham số tiếp theo là `findtime` và `maxretry`. Chúng làm việc cùng nhau để thiết lập các điều kiện, khi khách hàng bị phát hiện là người dùng bất hợp pháp thì sẽ bị cấm.

Biến `maxretry` sẽ đặt số lần thử mà khách hàng phải xác thực trong khoảng thời gian được xác định bởi `findtime`, trước khi bị cấm. Với cài đặt mặc định, dịch vụ Fail2ban sẽ cấm khách hàng trong vòng 10 phút khi đăng nhập không thành công 5 lần.

```
[DEFAULT]
. . .
action = $(action_)s
. . .

```

Tham số này thiết lập cấu hình hành động mà Fail2ban thực hiện khi nó muốn thực hiện lệnh cấm. Giá trị `action_` được xác định trong tệp ngay trước tham số này. Hành động mặc định là cập nhật cấu hình tường lửa của bạn để từ chối lưu lượng truy cập từ máy chủ vi phạm cho đến khi hết thời gian cấm.

Có các tập lệnh `action_` khác được cung cấp theo mặc định mà bạn có thể thay thế `$ (action_)` ở trên:

```
…
# ban & send an e-mail with whois report to the destemail.
action_mw = %(action_)s
            %(mta)s-whois[sender="%(sender)s", dest="%(destemail)s", protocol="%(protocol)s", chain="%(chain)s"]

# ban & send an e-mail with whois report and relevant log lines
# to the destemail.
action_mwl = %(action_)s
             %(mta)s-whois-lines[sender="%(sender)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]

# See the IMPORTANT note in action.d/xarf-login-attack for when to use this action
#
# ban & send a xarf e-mail to abuse contact of IP address and include relevant log lines
# to the destemail.
action_xarf = %(action_)s
             xarf-login-attack[service=%(__name__)s, sender="%(sender)s", logpath="%(logpath)s", port="%(port)s"]

# ban IP on CloudFlare & send an e-mail with whois report and relevant log lines
# to the destemail.
action_cf_mwl = cloudflare[cfuser="%(cfemail)s", cftoken="%(cfapikey)s"]
                %(mta)s-whois-lines[sender="%(sender)s", dest="%(destemail)s", logpath="%(logpath)s", chain="%(chain)s"]
…

```

Ví dụ: `action_mw` thực hiện hành động và gửi email, `action_mwl` thực hiện hành động, gửi email và bao gồm ghi nhật ký, `action_cf_mwl` thực hiện tất cả những điều trên cộng thêm việc gửi bản cập nhật cho Cloudflare API được liên kết với tài khoản của bạn để cấm người vi phạm ở đó.

### **Cài đặt Individual Jail cho dich vụ cần theo dõi**

Tiếp theo là phần của tệp cấu hình liên quan đến các dịch vụ riêng lẻ. Chúng được chỉ định bởi các tiêu đề phân đoạn như là `[sshd]`.

Mỗi phần trong số này cần được kích hoạt riêng bằng cách thêm một dòng `enabled = true` dưới tiêu đề, cùng với các cài đặt khác của chúng.

```
[jail_to_enable]
. . .
enabled =true
. . .

```

Theo mặc định, dịch vụ SSH được bật và tất cả các dịch vụ khác đều bị tắt. Một số cài đặt khác được đặt ở đây là `filter` sẽ được sử dụng để quyết định xem một dòng trong nhật ký có chỉ ra một xác thực không thành công hay không, và `logpath` cho Fail2ban biết vị trí của các nhật ký cho dịch vụ cụ thể đó.

Giá trị `filter` thực ra là một tham chiếu đến một tệp nằm trong thư mục `/etc/fail2ban/filter.d`, với phần mở rộng `.conf` của nó đã bị xóa. Các tệp này chứa regular expressions (biểu thức chính quy) (một cách viết tắt phổ biến để phân tích cú pháp văn bản) để xác định xem một dòng trong nhật ký có phải là một lần xác thực không thành công hay không. Chúng tôi sẽ không trình bày sâu về các tệp này trong bài hướng dẫn ở đây, vì chúng khá phức tạp và các cài đặt được xác định trước rất khớp với các dòng riêng biệt.

Tuy nhiên, bạn có thể xem loại filter nào có sẵn bằng cách xem xét thư mục đó:

```
$ ls /etc/fail2ban/filter.d

```

Nếu bạn thấy một tệp có vẻ liên quan đến dịch vụ bạn đang sử dụng, bạn nên mở tệp đó bằng trình soạn thảo văn bản. Hầu hết các tệp đều được giải thích khá tốt và ít nhất bạn sẽ có thể biết được loại tình trạng mà script (ngôn ngữ kịch bản) được thiết kế để bảo vệ không bị tổn hại. Hầu hết các filter này đều có các phần riêng biệt (bị vô hiệu hóa) trong tệp `jail.conf`, mà chúng ta có thể bật trong tệp `jail.local` nếu muốn.

Ví dụ: hãy tưởng tượng rằng bạn đang phục vụ một trang web bằng Nginx, và nhận ra rằng một phần được bảo vệ bằng mật khẩu trang web của bạn đang bị đóng khi đăng nhập. Bạn có thể yêu cầu Fail2ban sử dụng tệp `nginx-http-auth.conf` để kiểm tra tình trạng này trong tệp `/var/log/nginx/error.log`.

Điều này đã được thiết lập trong một phần có tên `[nginx-http-auth]` trong tệp `/etc/fail2ban/jail.conf` của bạn. Bạn chỉ cần thêm thông số `enabled`:

```
. . .
[nginx-http-auth]

enabled =true
. . .

```

Khi bạn hoàn tất chỉnh sửa, hãy lưu và đóng tệp. Tại thời điểm này, bạn có thể kích hoạt dịch vụ Fail2ban của mình để nó tự động chạy từ bây giờ. Đầu tiên, chạy `systemctl enable`:

```
$ sudo systemctl enable fail2ban

```

Sau đó, khởi động thủ công lần đầu tiên với `systemctl start`:

```
$ sudo systemctl start fail2ban

```

Bạn có thể xác minh nó đang chạy với `systemctl status`:

```
$ sudo systemctl status fail2ban

```

```
Output
● fail2ban.service - Fail2Ban Service
     Loaded: loaded (/lib/systemd/system/fail2ban.service; enabled; vendor preset: enab>
     Active: active (running) since Tue 2022-06-28 19:29:15 UTC; 3s ago
       Docs: man:fail2ban(1)
   Main PID: 39396 (fail2ban-server)
      Tasks: 5 (limit: 1119)
     Memory: 12.9M
        CPU: 278ms
     CGroup: /system.slice/fail2ban.service
             └─39396 /usr/bin/python3 /usr/bin/fail2ban-server -xf start

Jun 28 19:29:15 fail2ban20 systemd[1]: Started Fail2Ban Service.
Jun 28 19:29:15 fail2ban20 fail2ban-server[39396]: Server ready

```

Trong bước tiếp theo, bạn sẽ chứng minh Fail2ban đang hoạt động.

## **Bước 3: Kiểm tra Chính sách Cấm (Tùy chọn)**

Từ máy chủ khác, trong tương lai một máy chủ sẽ không cần đăng nhập vào Fail2ban server của bạn, bạn có thể kiểm tra các quy tắc bằng cách cấm máy chủ thứ hai đó. Sau khi đăng nhập vào máy chủ thứ hai của bạn, hãy thử SSH vào máy chủ Fail2ban. Bạn có thể thử kết nối bằng một nonexistent name:

```
$ ssh blah@your_server
```

Nhập các ký tự ngẫu nhiên vào lời nhắc mật khẩu. Lặp lại điều này một vài lần. Tại một số điểm, lỗi bạn nhận được sẽ thay đổi từ `Permission denied` thành `Connection refused`. Điều này báo hiệu rằng máy chủ thứ hai của bạn đã bị cấm khỏi máy chủ Fail2ban.

Trên máy chủ Fail2ban, bạn có thể xem quy tắc mới bằng cách kiểm tra đầu ra `iptables` của mình. `iptables` là lệnh tương tác với các quy tắc tường lửa và cổng cấp thấp trên máy chủ của bạn. Nếu bạn đã làm theo hướng dẫn của CloudFly để thiết lập máy chủ ban đầu, bạn sẽ được sử dụng `ufw` để quản lý các quy tắc tường lửa ở cấp cao hơn. Chạy `iptables -S` sẽ hiển thị cho bạn tất cả các quy tắc tường lửa mà `ufw` đã tạo:

```
$ sudo iptables -S

```

```
Output
-P INPUT DROP
-P FORWARD DROP
-P OUTPUT ACCEPT
-N f2b-sshd
-N ufw-after-forward
-N ufw-after-input
-N ufw-after-logging-forward
-N ufw-after-logging-input
-N ufw-after-logging-output
-N ufw-after-output
-N ufw-before-forward
-N ufw-before-input
-N ufw-before-logging-forward
-N ufw-before-logging-input
-N ufw-before-logging-output
…

```

Nếu bạn chuyển đầu ra của `iptables -S` thành `grep` để tìm kiếm trong các quy tắc đó cho chuỗi `f2b`, bạn có thể thấy các quy tắc đã được thêm vào bởi Fail2ban:

```
$ sudo iptables -S | grep f2b

```

```
Output
-N f2b-sshd
-A INPUT -p tcp -m multiport --dports 22 -j f2b-sshd
-A f2b-sshd -s134.209.165.184/32 -j REJECT --reject-with icmp-port-unreachable
-A f2b-sshd -j RETURN

```

Dòng chứa `REJECT --reject-with icmp-port-unreachable` sẽ được Fail2ban thêm vào và phản ánh địa chỉ IP của máy chủ thứ hai của bạn.

## **Kết luận**

Bây giờ bạn có thể thiết lập cấu hình một số chính sách cấm cho các dịch vụ của mình. Fail2ban là một cách hữu ích để bảo vệ bất kỳ loại dịch vụ nào sử dụng xác thực. Nếu bạn muốn tìm hiểu thêm về cách Fail2ban hoạt động, bạn có thể xem hướng dẫn của chúng tôi về Cách hoạt động của các quy tắc và tệp Fail2ban.