# Ceph authentication


Ceph Authentication là quá trình xác thực người dùng khi sử dụng các dịch vụ của Ceph. Các client trong Ceph có thể là end-user, admin hoặc các service/daemon liên quan đến Ceph, ví dụ OSD, monitor hoặc Object Gateways. Để xác định chính xác các loại client cũng như tránh khỏi can thiệp từ các bên thứ 3, Ceph cung cấp hệ thống xác thực tên cephx.

Trong hệ thống xác thực Ceph, mỗi client có một username và một khóa bí mật riêng để xác thực. User yêu cầu ceph client liên hệ với một monitor. Mỗi monitor đều có khả năng xác thực người dùng và phân phối key, do vậy không có "single point of failure" hoặc bottleneck khi sử dụng cephx. Monitor trả về một bộ dữ liệu xác thực gọi là ticket chứa session key để sử dụng các service của Ceph. Session key được mã hóa với các tham số bí mật của user, do vậy chỉ có user đã được chỉ định mới có thể yêu cầu dịch vụ từ Ceph monitor. Client sử dụng session key này để yêu cầu dịch vụ từ monitor, và monitor cung cấp cho user một ticket, ticket này sẽ xác thực người dùng để sử dụng osd. Ticket này giúp client có thể tương tác với bất kỳ osd hoặc mds. Ticket này có giới hạn thời gian.





Trong tổng quan, hệ thống xác thực Ceph đảm bảo tính bảo mật và tin cậy của hệ thống lưu trữ phân tán Ceph bằng cách sử dụng các chứng chỉ và mật khẩu riêng tư, và hỗ trợ nhiều phương thức xác thực khác nhau để đáp ứng nhu cầu sử dụng và cấu hình của hệ thống.

## Quá trình xác thực 

![Alt text](/Picture/Storage/auth2.png)

- Để sử dụng cephx, trước hết quản trị viên phải cấu hình các users. Trong quá trình này, user client.admin sẽ sử dụng lệnh `ceph auth get-or-create-key` để sinh ra username và khóa bí mật. Hệ thống sẽ sinh ra một username và khóa bí mật, lưu một bản sao lại cho mons và chuyển khóa bí mật lại cho user client.admin. Quá trình này mô tả client và mon chia sẻ khóa bí mật.

- Để xác xác thực monitor client chuyển user name cho monitor và monitor sẽ tạo session key và mã hoá nó cùng với sercet key có liên quan đến user name. Sau đó monitor sẽ truyên ticket được mã hoá trở về client. Client sẽ giải mã session key với sercet key và request ticket, monitor sẽ tạo và mã hoá ticket cùng với sercet key của user rồi truyền trở lại cho client. Client sẽ giải mã ticket và dùng nó để sign request đến OSDs và metadata server trong cluster.
- 
![Alt text](/Picture/Storage/auth3.png)

Giao thức cephx xác thực các liên lạc đang diễn ra giữa client và ceph server. Mỗi thông điệp được gửi giữa client và server được ký bởi ticket mà các monitor, OSD, MDS có thể xác thực bằng key chia sẻ:

![Alt text](/Picture/Storage/auth4.png)

 ## Các lệnh thao tác xác thực trên Ceph

- Hiển thị toàn bộ các key authen của cụm Ceph

```
ceph auth ls
```

    osd.0
        key: AQBL1MlkZmz0FxAAm0a38UvdUSxF+oyQf8t8gA==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    osd.1
        key: AQBL1Mlk9nTHHBAAixxpRmJBeF8uF603iBs7pw==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    osd.2
        key: AQBMC8pkOE4iHBAAxc5zioEHTw1sbp6AOYUfvw==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    osd.3
        key: AQBQ1MlkMJeXBRAArDZM0d15RQpn4Zo02Wn74w==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    osd.4
        key: AQBQ1MlkVou0CxAA88MWeBTGdROUPBCRPQSjfA==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    osd.5
        key: AQBRC8pkI5InBBAAii/9/Ksyce38WHYEBFn8gw==
        caps: [mgr] allow profile osd
        caps: [mon] allow profile osd
        caps: [osd] allow *
    client.admin
        key: AQB808lkiEroMxAAf+b6IsNdjag0x1GJOFY64A==
        caps: [mds] allow *
        caps: [mgr] allow *
        caps: [mon] allow *
        caps: [osd] allow *
    client.bootstrap-mds
        key: AQCA08lktaftNRAAOG79j+CVGM+L9wMfzN8BDg==
        caps: [mon] allow profile bootstrap-mds
    client.bootstrap-mgr
        key: AQCA08lkNcHtNRAA5YvBaGxIBypgdlwWqwWjYw==
        caps: [mon] allow profile bootstrap-mgr
    client.bootstrap-osd
        key: AQCA08lk0tbtNRAAaSE56uBMIcgv6pCxdnWreA==
        caps: [mon] allow profile bootstrap-osd
    client.bootstrap-rbd
        key: AQCA08lkFeztNRAAhkJf14vKDN02nqNdcDxWOQ==
        caps: [mon] allow profile bootstrap-rbd
    client.bootstrap-rbd-mirror
        key: AQCA08lk8gHuNRAASIX6llzDzS9++HsRvoD2Ag==
        caps: [mon] allow profile bootstrap-rbd-mirror
    client.bootstrap-rgw
        key: AQCA08lkUBjuNRAAngYt11At6fwB9XNClafqBQ==
        caps: [mon] allow profile bootstrap-rgw
    client.crash.ceph01
        key: AQDf08lkvW+VBhAAnrT3w7u/tO+jHpDRg+plAw==
        caps: [mgr] profile crash
        caps: [mon] profile crash
    client.crash.ceph02
        key: AQAO1MlklfzXARAAUSXDG/Wxb9iVFVlc135yhg==
        caps: [mgr] profile crash
        caps: [mon] profile crash
    client.crash.ceph03
        key: AQAV1Mlk4oMCDBAAi96sQMNDSXyNAEDdqluljg==
        caps: [mgr] profile crash
        caps: [mon] profile crash
    mgr.ceph01.vvqzyw
        key: AQB908lks5a1GhAADdqoihPCp5pfb22xskaBNg==
        caps: [mds] allow *
        caps: [mon] profile mgr
        caps: [osd] allow *
    mgr.ceph03.kvtbwn
        key: AQAY1Mlk/UZEFBAAgRrql+xDvFEsRRSCL6hmkA==
        caps: [mds] allow *
        caps: [mon] profile mgr
        caps: [osd] allow *
    installed auth entries:

Để truy xuất thông tin của user dùng lệnh 
```
ceph auth get {TYPE.ID}
```
hoặc
```
ceph auth export {TYPE.ID}
```


Một người dùng thông thường có ít nhất khả năng đọc trên Ceph monitor  và khả năng đọc và ghi trên Ceph OSD. Quyền OSD của người dùng thường bị hạn chế để người dùng chỉ có thể truy cập vào một nhóm cụ thể. 

Ví dụ:

    ceph auth add client.john mon 'allow r' osd 'allow rw pool=liverpool'
    ceph auth get-or-create client.paul mon 'allow r' osd 'allow rw pool=liverpool'
    ceph auth get-or-create client.george mon 'allow r' osd 'allow rw pool=liverpool' -o george.keyring
    ceph auth get-or-create-key client.ringo mon 'allow r' osd 'allow rw pool=liverpool' -o ringo.key


- lệnh (1) thêm ứng dụng khách tên john có khả năng đọc trên Ceph monitor và khả năng đọc và ghi trên pool có tên là liverpool.
- lệnh (2) ủy quyền cho client tên paul có khả năng đọc Ceph monitor và khả năng đọc và ghi trên nhóm có tên là liverpool
- lệnh (3) ủy quyền cho khách hàng có tên là george có khả năng đọc trên Ceph monitor  và có khả năng đọc và ghi trên nhóm có tên là liverpool và sử dụng khóa có tên là george.keyring để thực hiện ủy quyền này
- lệnh (4) ủy quyền cho khách hàng có tên ringo có khả năng đọc trên Ceph monitor và khả năng đọc và ghi trên nhóm có tên là liverpool

DELETING A USER

    ceph auth del {TYPE}.{ID}

PRINTING A USER’S KEY

    ceph auth print-key {TYPE}.{ID}

CREATING A KEYRING

    sudo ceph-authtool -C /etc/ceph/ceph.keyring