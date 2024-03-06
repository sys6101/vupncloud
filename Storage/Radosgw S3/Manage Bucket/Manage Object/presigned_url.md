## Chia sẻ link file

```
s3client.generate_presigned_url('get_object', Params = {'Bucket': 'test-bucket', 'Key': 'test.txt'}, ExpiresIn = 3600)
```
Output
```
http://vupn.cloud/test-bucket/error.html?AWSAccessKeyId=6LTZ9JO2QX6NA6JPOCX9&Signature=HGOJjamXiaOzrUyKa4uSErnoDwY%3D&Expires=1709284780
```

## Tạo link file để client upload file

```
presigned_url_with_metadata = s3.generate_presigned_url('put_object', Params={'Bucket':bucket_name, 'Key':'vupn.txt','ContentType':'text/html'}, ExpiresIn=3600)
print(presigned_url_with_metadata)



Output:

http://10.3.53.210:8080/test-bucket/vupn.txt?AWSAccessKeyId=6LTZ9JO2QX6NA6JPOCX9&Signature=cbnbOUGIRIX4Yg1%2F7wTFLIE7DiA%3D&content-type=text%2Fhtml&Expires=1709285969
```

Với link không có metadata thì sử dụng lệnh duới đây

`curl --request PUT --upload-file <đường dẫn đến file> "<url>"`         

Với link có metadata thì cần thêm header để đẩy thêm header lên server.

`curl -X PUT -T "./results.txt" -H "Content-Type: text/html" "http://10.3.53.210:8080/test-bucket/vupn.txt?AWSAccessKeyId=6LTZ9JO2QX6NA6JPOCX9&Signature=9uLwEtwP7ZhFgm5odIXDI6SW4DQ%3D&content-type=text%2Fhtml&Expires=1709285821"`


### Chú ý


Khi sử dụng tính năng này, cần chú ý khi sử dụng presigned-url, số lượng header được sinh ra ở trong câu lệnh presigned-url sẽ phải có tương ứng với cả lượng header khi sử dựng các client để upload file lên server.

Ví dụ ở dưới khi gen code server, có 2 header là Content-Type và x-amz-acl thì khi sử dụng các client, ví dụ như ở trên là sử dụng lệnh curl thì cũng phải có 2 header đó là Content-Type và x-amz-acl.

Nếu không có header tương ứng giữa presigned-url và client khi upload lên sẽ có thể xảy ra 1 trong 2 trường hợp sau:

Sinh ra lỗi 400 bad request khi sử dụng 1 các tool client như curl          
