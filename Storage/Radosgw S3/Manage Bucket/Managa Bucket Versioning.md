**Quản lý Bucket Versioning**         

Bucket Versioning là tính năng cho phép bạn lưu trữ nhiều phiên bản của cùng một đối tượng trong Cloud Storage. Khi bạn bật Bucket Versioning, sẽ tự động tạo một phiên bản lưu trữ mỗi khi file bị ghi đè hoặc xoá, cho phép người dùng có thể khôi phục file về các trạng thái trước đó.

**On/Off bucket versioning**
```
bucket_versioning = s3.BucketVersioning(bucketname)
bucket_versioning.enable()
bucket_versioning.suspend()
```

Xem trạng thái hiện tại:
```
bucket_versioning = s3.BucketVersioning(bucketname)
print(bucket_versioning.status)
```

![alt text](image-1.png)


List vesioning
Phiên bản đã xóa sẽ được đánh dấu ở `DeleteMarkers`

```
{'ResponseMetadata': {'RequestId': 'tx000007d3691391e182fcd-0065e18b60-1e9aae-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'transfer-encoding': 'chunked', 'x-amz-request-id': 'tx000007d3691391e182fcd-0065e18b60-1e9aae-default', 'content-type': 'application/xml', 'date': 'Fri, 01 Mar 2024 08:01:36 GMT', 'connection': 'Keep-Alive'}, 'RetryAttempts': 0}, 'IsTruncated': False, 'KeyMarker': '', 'VersionIdMarker': '', 'Versions': [{'ETag': '"e3a49993ae9d968981ebac84c637c840"', 'Size': 6, 'StorageClass': 'STANDARD', 'Key': 'error.html', 'VersionId': '8lL8CZOTkgNPRKXlMwbG8W2rFGjnIxR', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 3, 1, 7, 59, 3, 127000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}, {'ETag': '"e3a49993ae9d968981ebac84c637c840"', 'Size': 6, 'StorageClass': 'STANDARD', 'Key': 'error.html', 'VersionId': 'null', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 2, 27, 4, 42, 26, 953000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}, {'ETag': '"5aef7055bb8f1991840878e207fa0d78"', 'Size': 480, 'StorageClass': 'STANDARD', 'Key': 'index.html', 'VersionId': 'cYWVAFshFHhfVnVXfD7a72HAYiRgkdK', 'IsLatest': True, 'LastModified': datetime.datetime(2024, 3, 1, 7, 59, 3, 21000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}, {'ETag': '"5aef7055bb8f1991840878e207fa0d78"', 'Size': 480, 'StorageClass': 'STANDARD', 'Key': 'index.html', 'VersionId': 'null', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 2, 27, 4, 42, 26, 911000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}], 'DeleteMarkers': [{'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}, 'Key': 'error.html', 'VersionId': 'Zn2xxnGBz5NhDzcoX-IMLipXyxAckpM', 'IsLatest': True, 'LastModified': datetime.datetime(2024, 3, 1, 7, 59, 49, 180000, tzinfo=tzutc())}, {'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}, 'Key': 'error.html', 'VersionId': '9aZxFO.nmb0X1k0wWzPIGgjYC-buThz', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 2, 28, 8, 58, 2, 310000, tzinfo=tzutc())}], 'Name': 'test-bucket', 'Prefix': '', 'MaxKeys': 1000, 'EncodingType': 'url'}
```
