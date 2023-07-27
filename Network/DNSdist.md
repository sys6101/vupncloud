# **Load Balancing With DNSdist(BIND9)**

## **Giới thiệu**

DNSDist là một proxy DNS hiệu suất cao được phát triển bởi nền tảng PowerDNS. Nó được thiết kế để cung cấp giải pháp tăng cường hiệu suất và cân bằng tải cho hệ thống DNS, giúp nâng cao khả năng chịu tải và độ tin cậy của mạng DNS.

Với DNSDist, người dùng có thể xử lý hàng triệu truy vấn DNS mỗi giây và phân phối tải giữa các máy chủ DNS để giảm thiểu tình trạng quá tải và đảm bảo rằng hệ thống luôn hoạt động hiệu quả. DNSDist hỗ trợ nhiều chiến lược cân bằng tải, bao gồm round-robin, least outstanding, random, và sequential, giúp người quản trị có thể điều chỉnh cấu hình theo nhu cầu cụ thể của hệ thống.

Một số tính năng chính của DNSDist bao gồm:

- Cân bằng tải thông minh: DNSDist sử dụng các chiến lược cân bằng tải thông minh để đảm bảo rằng các truy vấn DNS được phân phối đều đặn giữa các máy chủ DNS, giúp tránh tình trạng quá tải và đáng tin cậy cao hơn.
- Proxy và Cache: DNSDist hoạt động như một proxy DNS, giúp giải quyết các truy vấn từ các DNS client và chuyển tiếp chúng đến các máy chủ DNS upstream. Nó cũng hỗ trợ bộ nhớ cache để giảm thiểu thời gian phản hồi cho các truy vấn phổ biến và giảm tải cho các máy chủ DNS chính.
- Quản lý truy vấn và phản hồi: DNSDist cung cấp các công cụ quản lý mạnh mẽ để theo dõi và ghi lại các truy vấn và phản hồi DNS, giúp phân tích và giám sát hiệu suất hệ thống một cách hiệu quả.
- Bảo mật: DNSDist hỗ trợ nhiều tính năng bảo mật, bao gồm giới hạn truy cập dựa trên địa chỉ IP, hạn chế truy cập đến các tài nguyên nhất định và hỗ trợ DNS over TLS (DoT) và DNS over HTTPS (DoH) để bảo mật việc truyền tải dữ liệu DNS.

DNSDist đã được sử dụng rộng rãi trong môi trường lớn và yêu cầu khắt khe về hiệu suất, cân bằng tải và bảo mật. Nó là một công cụ mạnh mẽ và linh hoạt cho việc cải thiện hiệu suất và độ tin cậy của hệ thống DNS và giúp tăng cường trải nghiệm người dùng khi sử dụng mạng DNS.

## **LAB DNSDist**

Mô hình:

![Alt text](/Picture/Linux/dnsdist.png)
### Điều kiện cần

Có 4 máy chủ chạy hệ điều hành Ubuntu 22.04 và chúng được kết nối với nhau qua một switch:
 - 1 server là DNSDist
 - 2 server là DNS Upstream(Resolver1, Resolver2)
 - 1 server DNSClient

### Cấu hình

**Trên 2 Resolver**:

Cài đặt bind9

```makefile
sudo apt install -y bind9 bind9utils bind9-doc dnsutils
```

Thêm đoạn config sau vào `/etc/bind/named.conf.options` bằng command `sudo vi /etc/bind/named.conf.options`:

```markdown
listen-on port 53 { any; };
allow-query { 10.0.218.107; };
allow-recursion { 10.0.218.107; };
```

- Trong đó `10.0.218.107` là địa chỉ của DNSDist

Khởi động lại bind9 khi cấu hình xong:

```makefile
sudo systemctl restart bind9
```

**Trên DNSDist**

Cài đặt DNSDist sử dụng command: `sudo apt install dnsdist`

Cấu hình DNSdist `sudo vi /etc/dnsdist/dnsdist.conf`:

```markdown
-- Listen on port 53 for incoming DNS queries from all IP addresses
setLocal("0.0.0.0:53")

-- Allow queries from all IP addresses
setACL("0.0.0.0/0")

-- IP addresses of your two Resolvers
resolver1 = newServer({
    address = "10.0.218.194",
    order = 11,
    checkType = "A",
    checkName = "resov1",
    maxCheckFailures = 5
})

resolver2 = newServer({
    address = "10.0.218.20",
    order = 11,
    checkType = "A",
    checkName = "resov2",
    maxCheckFailures = 5
})

-- Set the load balancing strategy to 'leastOutstanding'
setServerPolicy(leastOutstanding)

-- Create a new packet cache with the specified settings
pc = newPacketCache(10000, 86400, 0, 60, 60)

-- Set the packet cache for the default pool ('') to the newly created cache
getPool(""):setCache(pc)
```

Trong đó:

- `10.0.218.194` và `10.0.218.20` và IP của 2 Resolver
- `setServerPolicy(leastOutstanding)` là một lệnh dùng để đặt chiến lược cân bằng tải cho máy chủ DNS trong một pool của DNSDist.Khi một truy vấn DNS đến DNSDist, DNSDist sẽ xem xét các máy chủ DNS có trong pool mà truy vấn cần được chuyển tiếp tới. Chiến lược leastOutstanding sẽ chọn máy chủ DNS mà hiện đang có ít truy vấn chờ xử lý nhất, tức là máy chủ có tải công việc thấp hơn so với các máy chủ khác trong pool.

Tiếp theo chạy câu lệnh **`dnsdist -i 10.0.218.107`**, DNSDist sẽ bắt đầu chạy và lắng nghe các yêu cầu DNS từ địa chỉ IP 10.0.218.107 trên máy chủ. 

**Trên DNS Client**

Cấu hình DNS client `sudo vi /etc/resolv.conf` với ip DNSDist là `10.0.218.107`

```makefile
nameserver 10.0.218.107
options edns0
```

Từ DNSlient kiểm tra xem DNSDist hoạt động hay chưa:

```makefile
root@dnsclient:/etc# dig [google.com](http://google.com/) @10.0.218.107

; <<>> DiG 9.11.3-1ubuntu1.11-Ubuntu <<>> [google.com](http://google.com/) @10.0.218.107
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29055
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 4, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: c419bf0699dadf79b930d3a264c1e9b1ede3747489964e5c (good)
;; QUESTION SECTION:
;[google.com](http://google.com/).                    IN      A

;; ANSWER SECTION:
[google.com](http://google.com/).             300     IN      A       172.217.27.14

;; AUTHORITY SECTION:
[google.com](http://google.com/).             172060  IN      NS      [ns3.google.com](http://ns3.google.com/).
[google.com](http://google.com/).             172060  IN      NS      [ns1.google.com](http://ns1.google.com/).
[google.com](http://google.com/).             172060  IN      NS      [ns2.google.com](http://ns2.google.com/).
[google.com](http://google.com/).             172060  IN      NS      [ns4.google.com](http://ns4.google.com/).

;; Query time: 63 msec
;; SERVER: 10.0.218.107#53(10.0.218.107)
;; WHEN: Thu Jul 27 05:51:13 CEST 2023
;; MSG SIZE  rcvd: 155
```

Khi sử dụng DNSDist, nếu bạn tắt một trong hai Resolver (máy chủ DNS upstream) trong pool, DNSDist vẫn có thể tiếp tục phân giải các truy vấn DNS bằng cách sử dụng Resolver còn lại. Tuy nhiên, khi bạn tắt cả hai Resolver trong pool, DNSDist sẽ không thể phân giải truy vấn DNS và sẽ không hoạt động.

Điều này xảy ra vì DNSDist được cấu hình với một pool gồm hai Resolver. Khi một Resolver bị tắt, DNSDist sẽ tự động chuyển hướng các truy vấn đến Resolver còn lại trong pool để duy trì khả năng phân giải DNS. Tuy nhiên, khi không còn Resolver hoạt động trong pool, DNSDist không còn máy chủ nào để chuyển tiếp truy vấn DNS và sẽ không thể phân giải địa chỉ IP cho các tên miền được yêu cầu.

Để đảm bảo khả năng hoạt động liên tục của DNSDist, bạn nên đảm bảo rằng ít nhất một Resolver luôn hoạt động trong pool. Nếu bạn muốn tăng cường tính dự phòng, bạn cũng có thể cấu hình thêm các Resolver khác trong pool để đảm bảo rằng khi một Resolver bị tắt, các truy vấn vẫn được chuyển tiếp đến Resolver khác trong pool.