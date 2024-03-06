## Object Lock

Đây là tính năng giúp bảo vệ dữ liệu , tính năng này sẽ chống lại việc xóa dữ liệu. Kể cả trong trường hợp cố tình hay vô ý.

S3 cung cấp 2 mode để thực hiện Object locjk:           
- Governance : ở mode này có thể bảo vệ dữ liệu khỏi phần lớn người sử dụng khỏi việc xóa dữ liệu, tuy nhiên vẫn sẽ có một số người sử dụng với đặc quyền có thể xóa với việc cho phép người có quyền s3:BypassGovernanceRetention
- Compliance : mở mode này không một ai có thể xóa được dữ liệu trong khoảng thời gian được chỉ định, kể cả với user có quyền cao nhất

Để sử dung tính năng này:
- Phải enable tính năng bucket versioning
- Phải enable tính năng Object Lock khi tạo bucket , các bucket đã tạo rồi không thể enable tính năng này nữa

Ví dụ

```
bucket_name=<BUCKET-NAME>
s3_client.create_bucket(Bucket=bucket_name,ObjectLockEnabledForBucket=True)
response = s3_client.put_bucket_versioning(
   Bucket=bucket_name,
   VersioningConfiguration={
       'Status': 'Enabled'
   }
)
```

Chọn mode Compliance ,khóa object trong 1 năm
```
response = s3_client.put_object_lock_configuration(
   Bucket=bucket_name,
   ObjectLockConfiguration={
       'ObjectLockEnabled': 'Enabled',
       'Rule': {
           'DefaultRetention': {
               'Mode': 'COMPLIANCE',
               'Years': 1
           }
       }
   },
)
 ```
Thử xóa 1 version
```
response = s3.delete_object(
   Bucket=bucket_name,
   Key='error.html',
   VersionId='fhjDVHiPVsKoNO06sCfY2M3MRA1kAyo',
)
bucket_versioning = s3res.BucketVersioning(bucket_name)
object_versions = s3.list_object_versions(Bucket=bucket_name)
print(object_versions)


Output:
{'ResponseMetadata': {'RequestId': 'tx00000c9c10b521a9b9b23-0065e594cf-19e3d4-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amz-request-id': 'tx00000c9c10b521a9b9b23-0065e594cf-19e3d4-default', 'content-type': 'application/xml', 'content-length': '224', 'date': 'Mon, 04 Mar 2024 09:30:55 GMT'}, 'RetryAttempts': 0}, 'ObjectLockConfiguration': {'ObjectLockEnabled': 'Enabled', 'Rule': {'DefaultRetention': {'Mode': 'COMPLIANCE', 'Years': 1}}}}
{'ResponseMetadata': {'RequestId': 'tx000007f4c50eb5979b164-0065e594cf-261c00-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'transfer-encoding': 'chunked', 'x-amz-request-id': 'tx000007f4c50eb5979b164-0065e594cf-261c00-default', 'content-type': 'application/xml', 'date': 'Mon, 04 Mar 2024 09:30:55 GMT'}, 'RetryAttempts': 0}, 'IsTruncated': False, 'KeyMarker': '', 'VersionIdMarker': '', 'Versions': [{'ETag': '"e3a49993ae9d968981ebac84c637c840"', 'Size': 6, 'StorageClass': 'STANDARD', 'Key': 'error.html', 'VersionId': 'Rg5ocPIAB2hyHHuZHTgWoivAl9Vo6RJ', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 3, 4, 7, 36, 12, 489000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}, {'ETag': '"5aef7055bb8f1991840878e207fa0d78"', 'Size': 480, 'StorageClass': 'STANDARD', 'Key': 'index.html', 'VersionId': '**fhjDVHiPVsKoNO06sCfY2M3MRA1kAyo**', 'IsLatest': True, 'LastModified': datetime.datetime(2024, 3, 4, 7, 39, 1, 864000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}, {'ETag': '"5aef7055bb8f1991840878e207fa0d78"', 'Size': 480, 'StorageClass': 'STANDARD', 'Key': 'index.html', 'VersionId': 'M9ug1NWh28hmtW4TUxmjYVsWOq9gCrE', 'IsLatest': False, 'LastModified': datetime.datetime(2024, 3, 4, 7, 36, 12, 437000, tzinfo=tzutc()), 'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}}], 'DeleteMarkers': [{'Owner': {'DisplayName': 'VuPhung', 'ID': 'vupn'}, 'Key': 'error.html', 'VersionId': 'QsKXfArm3vS3qF4Yf.d4RJ9qTYw6P3M', 'IsLatest': True, 'LastModified': datetime.datetime(2024, 3, 4, 7, 36, 57, 671000, tzinfo=tzutc())}], 'Name': 'test-bucket2', 'Prefix': '', 'MaxKeys': 1000, 'EncodingType': 'url'}
```
Kiểm tra thấy version `fhjDVHiPVsKoNO06sCfY2M3MRA1kAyo` chưa bị xóa