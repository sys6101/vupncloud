**Quản lý Bucket Website**  





Bucket Website - Tính năng biến Bucket thành Static Hosting
Bucket Website là tính năng cho phép biến một bucket Cloud Storage thành một web server tĩnh để chứa các file static như HTML, CSS, JavaScript, hình ảnh, v.v. Tính năng này rất phù hợp cho các trường hợp sau:


Bucket Website có thể được sử dụng để lưu trữ code frontend, việc lưu trữ code frontend trên Bucket Website giúp bạn dễ dàng chia sẻ code với người khác và triển khai website.
Lợi ích của việc sử dụng Bucket Website:

Tiết kiệm chi phí: Bucket Website là dịch vụ miễn phí, bạn chỉ cần trả phí lưu trữ cho các file được lưu trữ trong bucket.
Dễ dàng sử dụng: Việc thiết lập và quản lý Bucket Website rất đơn giản.
Khả năng mở rộng cao: Bucket Website có thể mở rộng để đáp ứng nhu cầu lưu trữ của bạn.
Hiệu suất tốt: Bucket Website được tối ưu hóa cho việc phân phối nội dung tĩnh, giúp website của bạn hoạt động nhanh chóng và mượt mà.


PUT WEBSITE
```
import boto3
import json
from boto3.s3.transfer import TransferConfig
import botocore
import requests
from bs4 import BeautifulSoup
import json

access_key = '6LTZ9JO2QX6NA6JPOCX9'
secret_key = 'vNeYkOihowCOaurNmn7fY4gov2uyjEnAp4VWrlXf'
endpoint_url = 'http://vupn.cloud/'

s3 = boto3.client(
    's3',
    aws_access_key_id=access_key,
    aws_secret_access_key=secret_key,
    endpoint_url=endpoint_url,
    verify=False
)
s3res = boto3.resource('s3',aws_access_key_id=access_key,
            aws_secret_access_key=secret_key,
                endpoint_url=endpoint_url)

bucket_name = 'test-bucket'
#s3.create_bucket(Bucket=bucket_name)

bucket = s3res.Bucket(bucket_name)

s3res.Object(bucket_name, 'index.html').upload_file('/root/s3/html/index.html',ExtraArgs={'ACL': 'public-read', 'ContentType': 'text/html'})                                                               
s3res.Object(bucket_name, 'error.html').upload_file('/root/s3/html/error.html',ExtraArgs={'ACL': 'public-read', 'ContentType': 'text/html'})                                                               

website_configuration = {
    'ErrorDocument': {'Key': 'error.html'},
    'IndexDocument': {'Suffix': 'index.html'},
}
s3.put_bucket_website(
    Bucket=bucket_name,
    WebsiteConfiguration=website_configuration
)
web = s3.get_bucket_website(Bucket=bucket_name)
print(web)

```

Output
```
{'ResponseMetadata': {'RequestId': 'tx0000008196c7459d8ff7c-0065dd6832-19c063-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amz-request-id': 'tx0000008196c7459d8ff7c-0065dd6832-19c063-default', 'content-type': 'application/xml', 'content-length': '241', 'date': 'Tue, 27 Feb 2024 04:42:26 GMT'}, 'RetryAttempts': 0}, 'IndexDocument': {'Suffix': 'index.html'}, 'ErrorDocument': {'Key': 'error.html'}}
```