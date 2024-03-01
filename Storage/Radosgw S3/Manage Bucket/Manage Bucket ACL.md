**Quản lý Bucket ACL**  

Quản lý quyền truy cập

- `private `Chủ sở hữu có toàn quyền (FULL_CONTROL), không ai khác có quyền truy cập (mặc định)

Khi đang để private:        
```
bucket = s3res.Bucket(bucket_name)
bucket.Acl().put(ACL='private')
acl = s3.get_bucket_acl(Bucket=bucket_name)
print(acl)  

Output:

{'ResponseMetadata': {'RequestId': 'tx00000b6b6e1c0e665cd65-0065d6f31d-3c8fa-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amz-request-id': 'tx00000b6b6e1c0e665cd65-0065d6f31d-3c8fa-default', 'content-type': 'application/xml', 'content-length': '429', 'date': 'Thu, 22 Feb 2024 07:09:17 GMT', 'connection': 'Keep-Alive'}, 'RetryAttempts': 0}, 'Owner': {'DisplayName': 'Vu Phung', 'ID': 'vupn'}, 'Grants': [{'Grantee': {'DisplayName': 'Vu Phung', 'ID': 'vupn', 'Type': 'CanonicalUser'}, 'Permission': 'FULL_CONTROL'}]}
```
![alt text](/Storage/Radosgw%20S3/Image/ACL1.png)

- `public-read` Chủ sở hữu có toàn quyền (FULL_CONTROL), Tất cả người dùng khác có quyền đọc (READ)

- `public-read-write` Chủ sở hữu có to àn quyền (FULL_CONTROL), Tất cả người dùng khác có quyền đọc (READ) và ghi (WRITE)

- `authenticated-read` Chủ sở hữu có toàn quyền (FULL_CONTROL), Tất cả người dùng đăng nhập khác có quyền đọc (READ)

Khi có quyền READ:  
```
{'ResponseMetadata': {'RequestId': 'tx00000d3b2c8a0ae5b8987-0065d6f4b9-3c8fa-default', 'HostId': '', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amz-request-id': 'tx00000d3b2c8a0ae5b8987-0065d6f4b9-3c8fa-default', 'content-type': 'application/xml', 'content-length': '621', 'date': 'Thu, 22 Feb 2024 07:16:09 GMT', 'connection': 'Keep-Alive'}, 'RetryAttempts': 0}, 'Owner': {'DisplayName': 'Vu Phung', 'ID': 'vupn'}, 'Grants': [{'Grantee': {'Type': 'Group', 'URI': 'http://acs.amazonaws.com/groups/global/AllUsers'}, 'Permission': 'READ'}, {'Grantee': {'DisplayName': 'Vu Phung', 'ID': 'vupn', 'Type': 'CanonicalUser'}, 'Permission': 'FULL_CONTROL'}]}
```

![alt text](/Storage/Radosgw%20S3/Image/ACL2.png)