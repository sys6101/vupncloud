# HAProxy
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro1.png)  
### 1. Cài đặt HAProxy tại gateway
    apt-get install haproxy

### 2. Cấu hình trên 2 webserver
File cấu hình `/etc/haproxy/haproxy.cfg`
```

     global
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /var/lib/haproxy
        stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
        stats timeout 30s
        user haproxy
        group haproxy
        daemon

        # Default SSL material locations
        ca-base /etc/ssl/certs
        crt-base /etc/ssl/private

        # Default ciphers to use on SSL-enabled listening sockets.
        # For more information, see ciphers(1SSL). This list is from:
        #  https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/
        # An alternative list with additional directives can be obtained from
        #  https://mozilla.github.io/server-side-tls/ssl-config-generator/?server=haproxy
        ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS
        ssl-default-bind-options no-sslv3

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
        errorfile 400 /etc/haproxy/errors/400.http
        errorfile 403 /etc/haproxy/errors/403.http
        errorfile 408 /etc/haproxy/errors/408.http
        errorfile 500 /etc/haproxy/errors/500.http
        errorfile 502 /etc/haproxy/errors/502.http
        errorfile 503 /etc/haproxy/errors/503.http
        errorfile 504 /etc/haproxy/errors/504.http



backend WebSv
    server web1.example.com  192.168.1.1:80 check
    server web2.example.com  192.168.1.2:80 check

frontend HaSv
    bind *:80
    mode http
    default_backend WebSv

```
Phần backend (WebSv):

Backend này có hai server:
Server thứ nhất có tên là "web1.example.com" và địa chỉ IP là 192.168.1.1, lắng nghe trên cổng 80.
Server thứ hai có tên là "web2.example.com" và địa chỉ IP là 192.168.1.2, lắng nghe trên cổng 80.
Mỗi server được đánh dấu là "check" để HAProxy kiểm tra tính khả dụng của nó.
Phần frontend (HaSv):

Định nghĩa frontend với tên là "HaSv".
Frontend này sử dụng bind để lắng nghe kết nối trên cổng 80.
Chế độ là "http", tức là frontend sẽ hoạt động dưới giao thức HTTP.
Backend mặc định được đặt là "WebSv", tức là các kết nối sẽ được chuyển tiếp đến backend "WebSv".


Sau khi cấu hình, kiểm tra lại trước khi restart áp dụng các cài đặt:

    haproxy -c -f /etc/haproxy/haproxy.cfg

Nếu kết quả trả lại và `valid` tức không có lỗi thì restart:

    service haproxy restart

Như vậy HAProxy sẽ thực loadbalancing giữa 2 webbackend theo các lần truy cập (F5).     
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro2.png)  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro3.png)  

- Các index ở webbackend đã được sửa lại theo IP để dễ quan sát quá trình điều hướng kết nối.
## Thiết lập kết nối https trên nginx

Tao PEM cho nginx

`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt`               

`sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096` 

Tạo file các file cấu hình để trỏ đến SSL
/etc/nginx/snippets/self-signed.conf và  /etc/nginx/snippets/ssl-params.conf        
`sudo vim /etc/nginx/snippets/self-signed.conf`
```
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```
`sudo vim /etc/nginx/snippets/ssl-params.conf` 

```
ssl_protocols TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/nginx/dhparam.pem; 
ssl_ciphers EECDH+AESGCM:EDH+AESGCM;
ssl_ecdh_curve secp384r1;
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable strict transport security for now. You can uncomment the following
# line if you understand the implications.
#add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

Để tạo tệp PEM cho HAProxy bằng OpenSSL, bạn cần thực hiện các bước sau:

Tạo khóa riêng (private key) cho HAProxy:

`openssl genrsa -out haproxy.key 2048`
Tạo chứng chỉ xác thực yêu cầu (Certificate Signing Request - CSR):

`openssl req -new -key haproxy.key -out haproxy.csr`
Trong quá trình này, bạn sẽ được yêu cầu nhập thông tin xác thực. Hãy cung cấp các chi tiết tương ứng với môi trường và yêu cầu của bạn.

Tự ký chứng chỉ (self-signed certificate): Nếu bạn đang tạo một chứng chỉ tự ký, bạn không cần chứng chỉ CA. Trong trường hợp này, bạn có thể loại bỏ các tham số -CA và -CAkey khỏi lệnh, và chỉ sử dụng tệp khóa riêng của HAProxy để ký chứng chỉ:

`openssl x509 -req -in haproxy.csr -signkey haproxy.key -out haproxy.crt -days 365`


Kết hợp khóa riêng và chứng chỉ đã ký thành tệp PEM:

`cat haproxy.crt haproxy.key > haproxy.pem`



Cấu hình lại config haproxy ở `/etc/haproxy/haproxy.cfg`

![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro4.png)  

Cấu hình lại config ở webackend 
```
server {
        listen 443 ssl;
        listen [::]:443 ssl;
        include snippets/self-signed.conf;
        include snippets/ssl-params.conf;
        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;
        server_name 192.168.1.2;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
server {
        listen 80;
        listen [::]:80;

        server_name 192.168.1.2;

        return 302 https://$server_name$request_uri;
        }


```
### Kết quả
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro55.jpg)  
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/hapro6.jpg)  