## Cài đặt
```
pip install boto3
```
## Tính năng Secure Token
CREATE A RADOSGW USER FOR S3 ACCESS

```
sudo radosgw-admin user create --uid="vupn" --display-name="Vu Phung"

{
        "user_id": "vupn",
        "display_name": "Vu Phung",
        "email": "",
        "suspended": 0,
        "max_buckets": 1000,
        "auid": 0,
        "subusers": [],
        "keys": [{
                "user": "vupn",
                "access_key": "WXDBPRUZM70071QSZ8SP",
                "secret_key": "jbsPtFZzWTw77sZRBGNXA9A2N0DVUtYONo9hp1QO"
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


Code
```
import boto3
import json
from boto3.s3.transfer import TransferConfig
import botocore
# Khởi tạo client S3
access_key = 'G92OQKC9105KQGRS7LVT'
secret_key = 'pkWTdyf0DglAuFAElBRhbFYC4bb3FZfvuIntB1iE'
endpoint_url = 'http://10.3.53.210:8080'

s3client = boto3.client(                                                         
    's3',
    aws_access_key_id=access_key,
    aws_secret_access_key=secret_key,
    endpoint_url=endpoint_url,
    verify=False 
)

bucket_name = 'test-bucket'
s3res = boto3.resource('s3',aws_access_key_id=access_key,
            aws_secret_access_key=secret_key,
                endpoint_url=endpoint_url)

bucket_name = 'test-bucket'

# Tạo bucket
#s3.create_bucket(Bucket=bucket_name)

#Dùng bucket
bucket = s3res.Bucket(bucket_name)

for bucket in s3res.buckets.all():
    print(bucket.name)
```

