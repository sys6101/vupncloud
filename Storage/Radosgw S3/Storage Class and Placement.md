## Storage Class and Placement

Storage Class là một khái niệm được sử dụng để đặc tả loại lưu trữ được sử dụng cho đối tượng. Ceph RADOSGW hỗ trợ nhiều loại Storage Class, mỗi loại có những đặc điểm và thuộc tính riêng. Dưới đây là một số Storage Class phổ biến:


STANDARD: Đây thường là loại lưu trữ được tối ưu hóa cho hiệu suất cao và thời gian truy cập nhanh. Dữ liệu được lưu trữ ở đây thường được đảm bảo rằng sẽ có thời gian đáp ứng thấp, phục vụ cho các ứng dụng yêu cầu hiệu suất cao như các ứng dụng web hoặc dịch vụ đám mây.

COLD: Đối với storage class này, ưu tiên có thể là sự tiết kiệm chi phí và không gian lưu trữ. Dữ liệu có thể được giữ ở đây khi không cần truy cập thường xuyên. Các phương pháp lưu trữ thường có thể bao gồm việc sử dụng các thiết bị lưu trữ có chi phí thấp hơn hoặc lưu trữ dữ liệu trên các tầng lưu trữ có hiệu suất thấp hơn.

INTELLIGENT_TIERING: Dùng để tối ưu hóa chi phí lưu trữ bằng cách tự động chuyển đối tượng giữa các tầng lưu trữ.

ONEZONE_IA: Lưu trữ dữ liệu ở một khu vực duy nhất, giảm chi phí so với STANDARD nhưng mất đi tính sẵn có cao.

GLACIER, DEEP_ARCHIVE: Dùng cho lưu trữ dữ liệu lâu dài với chi phí thấp, nhưng thời gian truy cập cao.

**Placement** trong radosgw là cách dữ liệu được phân phối và lưu trữ trên các node và xone khác nhau.Nó liên quan đến cách RADOS Gateway quản lý và định vị dữ liệu trên nhiều thành phần để đảm bảo tính sẵn sàng, độ tin cậy và hiệu suất.

Example

```
radosgw-admin zonegroup get
{
    "id": "9ae514b4-ced6-45c7-a02b-9a20b3046ea4",
    "name": "default",
    "api_name": "default",
    "is_master": "true",
    "endpoints": [],
    "hostnames": [],
    "hostnames_s3website": [],
    "master_zone": "bd84d433-f17c-4999-a496-014340f4e1fc",
    "zones": [
        {
            "id": "bd84d433-f17c-4999-a496-014340f4e1fc",
            "name": "default",
            "endpoints": [],
            "log_meta": "false",
            "log_data": "false",
            "bucket_index_max_shards": 11,
            "read_only": "false",
            "tier_type": "",
            "sync_from_all": "true",
            "sync_from": [],
            "redirect_zone": ""
        }
    ],
    "placement_targets": [
        {
            "name": "default-placement",
            "tags": [],
            "storage_classes": [
                "COLD"
            ]
        },
        {
            "name": "temporary",
            "tags": [],
            "storage_classes": [
                "STANDARD"
            ]
        }
    ],
    "default_placement": "default-placement",
    "realm_id": "",
    "sync_policy": {
        "groups": []
    }
}

```
 


