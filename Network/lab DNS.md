![Screenshot from 2023-03-23 17-08-29](https://user-images.githubusercontent.com/54473576/227170721-d767fd6d-00e2-4783-ae48-e9d094df9ca8.png)
# Trên máy gateway:

## Thay đổi IPv4 của interface ens37 

Dùng: `sudo vim /etc/netplan/00-installer-config.yaml`.

```
network:
  ethernets:
    ens33:
      dhcp4: true
    ens37:
      addresses:
      - 10.0.0.1/24
  version: 2
```
Dùng `sudo netplan apply` để lưu lại đã thay đổi

Dùng `ip a` để xem cài đặt đã thay đổi


## Cài đặt isc-dhcp-server

Cài đặt isc-dhcp-server

```
sudo apt install isc-dhcp-server
```
Dùng `sudo vim /etc/dhcp/dhcpd.conf` để sửa file config

```
# To specify the default and maximum lease time 
default-lease-time 600;

max-lease-time 7200;

ddns-update-style none;

authoritative;

subnet 10.0.0.0 netmask 255.255.255.0
{
        range 10.0.0.100 10.0.0.200;
        option routers 10.0.0.1;
        option subnet-mask 255.255.255.0;
        option domain-name-servers 10.0.1.10;
}
```

Dùng `sudo systemctl restart isc-dhcp-server` để khởi động dịch vụ `isc-dhcp-server`

Dùng `sudo systemctl enable isc-dhcp-server` để bật dịch vụ `isc-dhcp-server`


## Enable IPv4_forward:

Để nhận các gói tin từ NIC này sang NIC khác của máy

Uncomment dòng `net.ipv4.ip_forward=1` trong file /etc/sysctl.conf.

Mở file /etc/sysctl.conf dùng `sudo vim /etc/sysctl.conf`. Nhấn `i` để vào chế độ insert và sửa. Sửa xong thì nhấn phím `Esc` và gõ `:wq`

Áp dụng thay đổi tức thì mà không cần reboot dùng: `sudo sysctl -p /etc/sysctl.conf`

## Cài đặt NAT:

Cài đặt iptables-persistent: dùng `sudo apt-get install iptables-persistent`

Dùng `sudo iptables --table nat -L -v` để xem các chain và rule trong bảng `nat`

```
gw1@gateway:~$ sudo iptables --table nat -L -v
Chain PREROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
```
Dùng `sudo iptables --table nat --append POSTROUTING -s 10.0.0.0/24 --out-interface ens33 --jump MASQUERADE` để thêm rule NAT

Lưu cài đặt đã thay đổi với iptables: `sudo iptables-save > /etc/iptables/rules.v4`

# Trên DNS server

Install bind9:
```
sudo apt install -y bind9 bind9utils bind9-doc dnsutils
```

## Cài đặt DNS Forwarding

Sửa file config DNS dùng:
```
sudo vim /etc/bind/named.conf.options
```
```
acl trustedclients {
        localhost;
        localnets;
        10.0.0.0/24;
        10.0.1.0/24;
        10.0.2.0/24;
};

options {
        directory "/var/cache/bind";

        recursion yes;
        allow-query { trustedclients; };
        allow-query-cache { trustedclients; };
        allow-recursion { trustedclients; };
        forwarders {
                8.8.8.8;
                8.8.4.4;
        };

        dnssec-validation auto;

        listen-on-v6 { any; };
        listen-on port 53 { 127.0.0.1; 10.0.1.10; };
};

```

## DNS local

### Define zone files

Dùng `sudo vim named.conf.local` để mở file defile zone

```
zone "labvm.com" {
        type master;
        file "/etc/bind/db.labvm.com";
};

zone "1.0.10.in-addr.arpa" {
        type master;
        file "/etc/bind/db.10.0.1";
};
```
Kiểm tra lại các lỗi với câu lệnh :
```
sudo named-checkconf
```

### Create a forward lookup zone

Tạo 1 file forwarding file bằng cách:
```
sudo cp db.local db.labvm.com
```

Trong file `db.labvm.com` sau khi tạo có như:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     localhost. root.localhost. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      localhost.
@       IN      A       127.0.0.1
@       IN      AAAA    ::1
```
Sửa lại :
```
;
; BIND data file for local labvm.com zone
;
$TTL    604800
@       IN      SOA     ns1.labvm.com. admin.labvm.com. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.labvm.com.

ns1     IN      A       10.0.1.10
dhcp1   IN      A       10.0.1.1
```

Kiểm tra lỗi cú pháp với: 
```
sudo named-checkzone labvm.com db.labvm.com
```

### Create a reverse lookup zone
Để tạo 1 file reverse zone dùng:
```
sudo cp db.127 db.10.0.1
```

Mở file `db.10.0.1` được như:

```
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     localhost. root.localhost. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      localhost.
1.0.0   IN      PTR     localhost.
```

Sửa lại:
```
;
; BIND reverse data file for local labvm.com zone
;
$TTL    604800
@       IN      SOA     ns1.labvm.com. admin.labvm.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.labvm.com.

10      IN      PTR     ns1.labvm.com.
1       IN      PTR     dhcp1.labvm.com.
```

Kiểm tra lại cú pháp với:
```
sudo named-checkzone 1.0.10.in-addr.arpa db.10.0.1
```
### Sửa DNS entry của server để tự dùng DNS của chính nó
Sửa file config interface:
```
sudo vim /etc/netplan/00-installer-config.yaml 
```
Sửa lại dòng addresses của nameservers thành địa chỉ loop của server
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens36:
      addresses: [10.0.1.10/24]
      gateway4: 10.0.1.1
      nameservers:
        addresses:
        - 127.0.0.1
        search:
        - labvm.com
  version: 2
```

Lưu lại cài đặt đã sửa với netplan:
```
sudo netplan apply
```


## NSLOOKUP
![Alt](https://github.com/sys6101/vupncloud/raw/main/Picture/Network/dns.png)  