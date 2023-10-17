## Cài đặt
```
pip install boto3
```
## Tính năng Secure Token
CREATE A RADOSGW USER FOR S3 ACCESS

```
sudo radosgw-admin user create --uid="testuser" --display-name="First User"

{
        "user_id": "testuser",
        "display_name": "First User",
        "email": "",
        "suspended": 0,
        "max_buckets": 1000,
        "auid": 0,
        "subusers": [],
        "keys": [{
                "user": "testuser",
                "access_key": "I0PJDPCIYZ665MW88W9R",
                "secret_key": "dxaXZ8U90SXydYzyS5ivamEP20hkLSUViiaR+ZDA"
        }],
        "swift_keys": [],
        "caps": [],
        "op_mask": "read, write, delete",
        "default_placement": "",
        "placement_tags": [],
        "bucket_quota": {
                "enabled": false,
                "max_size_kb": -1,
                "max_objects": -1
        },
        "user_quota": {
                "enabled": false,
                "max_size_kb": -1,
                "max_objects": -1
        },
        "temp_url_keys": []
}
```
**Quản lý Bucket ACL**  

Quản lý quyền truy cập

- `private `Chủ sở hữu có toàn quyền (FULL_CONTROL), không ai khác có quyền truy cập (mặc định)

- `public-read` Chủ sở hữu có toàn quyền (FULL_CONTROL), Tất cả người dùng khác có quyền đọc (READ)

- `public-read-write` Chủ sở hữu có to àn quyền (FULL_CONTROL), Tất cả người dùng khác có quyền đọc (READ) và ghi (WRITE)

- `authenticated-read` Chủ sở hữu có toàn quyền (FULL_CONTROL), Tất cả người dùng đăng nhập khác có quyền đọc (READ)

**Quản lý Bucket Policy**           
- Phân quyền chi tiết cho người dùng cụ thể

**Quản lý Bucket CORS**                 
- Cho phéo các website với các tên miền khác nhau cùng truy cập cùng một bucket và các resource bên trong

**Quản lý Bucket Versioning**           
Tạo một phiển bản lưu trữ mỗi khi 

**Quản lý Bucket Website**          

**Quản lý Bucket Lifecycle**  

- Bucket Lifecycle là tính năng giúp người dùng thực hiện các thao tác như sau :

- Tự động xóa các file sau một khoảng thời gian nhất định (ví dụ là 3 ngày, 1 tuần ), có thể sử dụng tính năng này biến bucket thành backup

- Tự động di chuyển object tới một storage class khác, nhằm đưa object vào archive hay đưa tới những storage class nơi yêu cầu truy suất cao hơn

- Tự động Abort Incomplete MultiPart Upload (tự động hủy các multi-part upload mà chưa hoàn thành), giúp dọn dẹp các object upload bị lỗi trong quá trình upload

- Tự động xóa các object version mà không phải object version mới nhất (NoncurrentVersionExpiration) sau n ngày



Code
```
import boto3
import json
from boto3.s3.transfer import TransferConfig
import botocore
# Khởi tạo client S3
access_key = 'G92OQKC9105KQGRS7LVT'
secret_key = 'pkWTdyf0DglAuFAElBRhbFYC4bb3FZfvuIntB1iE'
endpoint_url = 'http://ceph01:8080'

s3client = boto3.client(                                                         
    's3',
    aws_access_key_id=access_key,
    aws_secret_access_key=secret_key,
    endpoint_url=endpoint_url,
    verify=False  # Chỉ sử dụng nếu không có xác minh SSL
)

bucket_name = 'test-bucket'
config = TransferConfig(
    multipart_threshold=50*1024*1024,  
    max_concurrency=10,
    num_download_attempts=10
    )
s3res = boto3.resource('s3',aws_access_key_id=access_key,
            aws_secret_access_key=secret_key,
                endpoint_url=endpoint_url)
#s3res.Bucket("test-bucket").upload_file("/root/testboto/test.py", "/root/testboto", Config=config)
###---Danh sách file---###
#result = s3client.list_objects(Bucket="test-bucket",
#                                Prefix='/root/testboto/',
#                                Delimiter='/'
#                                )
#print(result)

###---List Object---###
response = s3client.list_objects(Bucket='test-bucket')
for obj in response.get('Contents', []):
    print(f"Object Key: {obj['Key']}")

name = s3res.Bucket("test-bucket")

###---Down, Up file---###
#s3client.upload_file('/root/testboto/aupload.py', 'test-bucket', '/root/testboto/aupload.py')
#s3res.Object(bucket_name, '/root/testboto/s3test.py').upload_file('/root/testboto/s3test.py')

s3res.Object(bucket_name, '/root/testboto/s3test.py').upload_file('/root/testboto/aupload.py')
print(f"Uploaded  {bucket_name}")
s3client.download_file('test-bucket', '/root/testboto/aupload.py', '/root/testboto/as3.py')
#with open('/root/atest2.py', 'wb') as data:
    #s3client.download_fileobj('test-bucket', '/root/testboto/s3test.py', data)
print(open('/root/testboto/as3.py').read())

###---Delete object---###
object = s3res.Object('test-bucket','/root/testboto')
object.delete() 

```