**Quản lý Bucket Lifecycle**  

- Bucket Lifecycle là tính năng giúp người dùng thực hiện các thao tác như sau :

- Tự động xóa các file sau một khoảng thời gian nhất định (ví dụ là 3 ngày, 1 tuần ), có thể sử dụng tính năng này biến bucket thành backup

- Tự động di chuyển object tới một storage class khác, nhằm đưa object vào archive hay đưa tới những storage class nơi yêu cầu truy suất cao hơn

- Tự động Abort Incomplete MultiPart Upload (tự động hủy các multi-part upload mà chưa hoàn thành), giúp dọn dẹp các object upload bị lỗi trong quá trình upload

- Tự động xóa các object version mà không phải object version mới nhất (NoncurrentVersionExpiration) sau n ngày

```
bucket_lifecycle = s3res.BucketLifecycle('test-bucket')
lifecycle_configuration = {
    'Rules': [
        {
            'ID': 'ExampleRule',
            'Status': 'Enabled',
            'Prefix': 'index.html',  # The objects to which the rule applies
            'Transitions': [
                {
                    'Days': 30,  # Number of days after which the objects should transition
                    'StorageClass': 'STANDARD_IA'  # The target storage class
                },
                {
                    'Days': 365,
                    'StorageClass': 'GLACIER'
                }
            ],
            'NoncurrentVersionExpiration': {
                    'NoncurrentDays': 1
                },
            'AbortIncompleteMultipartUpload': {
                    'DaysAfterInitiation': 1
                }
        }
    ]
}

s3.put_bucket_lifecycle_configuration(Bucket='test-bucket', LifecycleConfiguration=lifecycle_configuration)
lifecycle = s3.get_bucket_lifecycle_configuration(Bucket='test-bucket')
print(lifecycle)
```
Output
```
{'ResponseMetadata': {'RequestId': 'tx00000677224ec4fe52291-0065e1340e-1e9aae-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amz-request-id': 'tx00000677224ec4fe52291-0065e1340e-1e9aae-default', 'content-type': 'application/xml', 'content-length': '578', 'date': 'Fri, 01 Mar 2024 01:49:02 GMT', 'connection': 'Keep-Alive'}, 'RetryAttempts': 0}, 'Rules': [{'ID': 'ExampleRule', 'Prefix': 'index.html', 'Status': 'Enabled', 'Transitions': [{'Days': 365, 'StorageClass': 'GLACIER'}, {'Days': 30, 'StorageClass': 'STANDARD_IA'}], 'NoncurrentVersionExpiration': {'NoncurrentDays': 1}, 'AbortIncompleteMultipartUpload': {'DaysAfterInitiation': 1}}]}
```


Ở đây object index.html sẽ được chuyển qua STANDARD_IA sau 30 ngày, chuyển qua GLACIER sau 365 ngày