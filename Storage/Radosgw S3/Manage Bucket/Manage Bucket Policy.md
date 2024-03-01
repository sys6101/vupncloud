**Quản lý Bucket Policy**           
- Phân quyền chi tiết cho người dùng cụ thể         
- Chức năng này được sử dụng để thực hiện chia sẻ tài nguyên dùng chung giữa nhiều tài khoản khác nhau.

Ví dụ:

```
bucket_policy = {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam:::user/vupn"
      },
      "Action": [
        "s3:ListBucket",
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::test-bucket",
        "arn:aws:s3:::test-bucket/*"
      ]
    }
  ]
}


bucket_policy = json.dumps(bucket_policy)
s3.put_bucket_policy(Bucket=bucket_name,Policy=bucket_policy)
bucket_policys = s3.get_bucket_policy(Bucket=bucket_name)
print(bucket_policy)


Output:

{"Version": "2012-10-17", "Statement": [{"Effect": "Allow", "Principal": {"AWS": "arn:aws:iam:::user/vupn"}, "Action": ["s3:ListBucket", "s3:PutObject", "s3:GetObject", "s3:DeleteObject"], "Resource": ["arn:aws:s3:::test-bucket", "arn:aws:s3:::test-bucket/*"]}]}
```
Ở đây phân quyền user vupn sẽ có quyền ListBucket, PutObject, GetObject, DeleteObject ở bucket `test-bucket`

Trong policy ở trên sẽ có phần

- `Version`: `2012-10-17` : Đây là xác định version của policy, phần này không thể thay đổi

- `Statement` Đây là danh sách quyền truy cập vào các tài nguyên của bucket , ví dụ như được truy cập vào bucket A và không được truy cập vào bucket B, có thể có nhiều quyền truy cập trong 1 Statement. Trong 1 thành phần Statement thì sẽ có cách thành phần con như sau:

- `Effect` có thể là 1 trong 2 giá trị là Allow hoặc Deny sẽ quyết định có quyền truy cập vào tài nguyên

- `Principal` là định danh của người dùng được phân quyền trong phần Effect trong đó vupn là ID của người dùng được phân quyền

- `Action` là danh sách các thao tác được cho phép hoặc bị cấm tác động lên bucket hoặc file bởi người dùng ở phần Principal 


Các action này có thể là 1 hoặc kết hợp nhiều action dưới đây :         

- AbortMultipartUpload: Quyền dừng tải lên một phần của đối tượng (multipart upload).

- CreateBucket: Quyền tạo một bucket mới.

- DeleteBucketPolicy: Quyền xóa các chính sách (policy) của bucket.

- DeleteBucket: Quyền xóa một bucket.

- DeleteBucketWebsite: Quyền xóa cấu hình trang web của bucket.

- DeleteObject: Quyền xóa một đối tượng trong bucket.

- DeleteObjectVersion: Quyền xóa một phiên bản cụ thể của một đối tượng trong bucket.

- DeleteReplicationConfiguration: Quyền xóa cấu hình bản sao của bucket.

- GetAccelerateConfiguration: Quyền xem cấu hình Accelerate.

- GetBucketAcl: Quyền xem Access Control List (ACL) của bucket.

- GetBucketCORS: Quyền xem cấu hình CORS của bucket.

- GetBucketLocation: Quyền xem vị trí (region) của bucket.

- GetBucketLogging: Quyền xem cấu hình logging của bucket.

- GetBucketNotification: Quyền xem cấu hình thông báo của bucket.

- GetBucketPolicy: Quyền xem chính sách của bucket.

- GetBucketRequestPayment: Quyền xem cấu hình request payment của bucket.

- GetBucketTagging: Quyền xem các tag đã được áp dụng cho bucket.

- GetBucketVersioning: Quyền xem cấu hình phiên bản của bucket.

- GetBucketWebsite: Quyền xem cấu hình trang web của bucket.

- GetLifecycleConfiguration: Quyền xem cấu hình vòng đời của bucket.

- GetObjectAcl: Quyền xem Access Control List (ACL) của đối tượng.

- GetObject: Quyền tải về nội dung của một đối tượng từ bucket.

- GetObjectTorrent: Quyền tải về đối tượng dưới dạng torrent.

- GetObjectVersionAcl: Quyền xem ACL của một phiên bản cụ thể của đối tượng.

- GetObjectVersion: Quyền tải về nội dung của một phiên bản cụ thể của đối tượng.

- GetObjectVersionTorrent: Quyền tải về phiên bản của đối tượng dưới dạng torrent.

- GetReplicationConfiguration: Quyền xem cấu hình sao chép của bucket.

- ListAllMyBuckets: Quyền xem danh sách tất cả các bucket mà người dùng có quyền truy cập.

- ListBucketMultiPartUploads: Quyền xem danh sách các tải lên đa phần trong bucket.

- ListBucket: Quyền xem danh sách các đối tượng trong bucket.

- ListBucketVersions: Quyền xem danh sách các phiên bản của đối tượng trong bucket.

- ListMultipartUploadParts: Quyền xem danh sách các phần của một tải lên đa phần.

- PutAccelerateConfiguration: Quyền cập nhật hoặc đặt cấu hình tăng tốc.

- PutBucketAcl: Quyền cập nhật hoặc đặt ACL của bucket.

- PutBucketCORS: Quyền cập nhật hoặc đặt cấu hình CORS của bucket.

- PutBucketLogging: Quyền cập nhật hoặc đặt cấu hình logging của bucket.

- PutBucketNotification: Quyền cập nhật hoặc đặt cấu hình thông báo của bucket.

- PutBucketPolicy: Quyền cập nhật hoặc đặt chính sách của bucket.

- PutBucketRequestPayment: Quyền cập nhật hoặc đặt cấu hình thanh toán yêu cầu của bucket.

- PutBucketTagging: Quyền cập nhật hoặc đặt các tag cho bucket.

- PutBucketVersioning: Quyền cập nhật hoặc đặt cấu hình phiên bản của bucket.

- PutBucketWebsite: Quyền cập nhật hoặc đặt cấu hình trang web của bucket.

- PutLifecycleConfiguration: Quyền cập nhật hoặc đặt cấu hình vòng đời của bucket.

- PutObjectAcl: Quyền cập nhật hoặc đặt ACL của một đối tượng.

- PutObject: Quyền tải lên một đối tượng vào bucket.

- PutObjectVersionAcl: Quyền cập nhật hoặc đặt ACL của một phiên bản cụ thể của đối tượng.

- PutReplicationConfiguration: Quyền cập nhật hoặc đặt cấu hình sao chép của bucket.

- RestoreObject: Quyền khôi phục một đối tượng từ trạng thái archiving.
