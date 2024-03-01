1. Flatten:

Flatten trong Ceph Rados Gateway (RGW):

Ceph là một hệ thống lưu trữ phân tán mạnh mẽ và RGW là một thành phần của Ceph cho phép bạn thao tác với dữ liệu qua giao diện HTTP/RESTful.
Trong RGW, "flatten" có thể ám chỉ việc tạo ra một cấu trúc thư mục hoặc đường dẫn đối tượng sẽ dẫn đến một "đối tượng" (object) trong hệ thống lưu trữ Ceph. Điều này có thể thực hiện bằng cách xác định các đối tượng trong RGW sử dụng tên đối tượng và định dạng đường dẫn tương ứng.
Flatten trong OpenStack Swift:

OpenStack Swift là một dự án mã nguồn mở dựa trên lưu trữ đối tượng (object storage) và cũng có thể liên quan đến "flatten".
Trong Swift, "flatten" có thể đề cập đến việc sắp xếp hoặc định dạng các đối tượng và thư mục bên trong hệ thống lưu trữ đối tượng. Việc này có thể giúp tổ chức dữ liệu hiệu quả hơn và tối ưu hóa việc truy cập dữ liệu.
2. Snapshot:

Snapshot trong hệ thống lưu trữ:

Trong hệ thống lưu trữ dữ liệu, một snapshot là một bản sao của dữ liệu tại một thời điểm cụ thể. Snapshots được sử dụng cho các mục đích như sao lưu, bảo vệ dữ liệu khỏi mất mát hoặc sự cố, và để khôi phục dữ liệu đến trạng thái trước đó.
Ví dụ: Trong hệ thống quản lý cơ sở dữ liệu, bạn có thể tạo một snapshot của cơ sở dữ liệu hàng ngày để sao lưu dữ liệu của bạn.
Snapshot trong máy ảo (virtualization):

Trong ảo hóa, một máy ảo (VM) snapshot là một trạng thái hoặc hình ảnh của máy ảo tại một thời điểm nhất định. Điều này cho phép bạn ghi lại trạng thái của VM và sau đó khôi phục nó đến trạng thái đó sau khi thực hiện các thay đổi hoặc thử nghiệm. Điều này hữu ích trong việc thử nghiệm phần mềm và bảo vệ VM khỏi các thay đổi không mong muốn.


So sánh Snapshot vs Backup


**Tính chất:**

Backup: Backup là một bản sao dự phòng hoàn chỉnh của dữ liệu, thường được lưu trữ trên một phương tiện lưu trữ riêng biệt như đĩa cứng ngoài, băng, hoặc dịch vụ lưu trữ đám mây. Dữ liệu backup có thể giữ lâu dài và có thể phục hồi khi cần thiết.          
Snapshot: Snapshot là một trạng thái ảnh chụp tại một thời điểm cụ thể của hệ thống hoặc dữ liệu. Nó thường dựa trên việc ghi lại trạng thái hiện tại của dữ liệu mà không tạo ra một bản sao riêng biệt. Snapshot thường ngắn hạn và có thể thay đổi khi dữ liệu gốc thay đổi.

**Thời gian sử dụng:**

Backup: Backup thường được tạo và lưu trữ theo chu kỳ định kỳ, thường hàng ngày, hàng tuần hoặc hàng tháng, để đảm bảo rằng bạn có bản sao dự phòng mới nhất của dữ liệu của bạn.           
Snapshot: Snapshot có thể được tạo và sử dụng nhanh chóng và linh hoạt. Nó có thể được sử dụng cho mục tiêu như sao lưu ngay lập tức hoặc thử nghiệm tại một thời điểm cụ thể.

**Khả năng phục hồi:**

Backup: Backup cho phép bạn phục hồi dữ liệu và hệ thống đến một thời điểm trước đó hoàn toàn. Bạn có khả năng chọn bất kỳ thời điểm nào trong quá trình backup để phục hồi.                        
Snapshot: Snapshot thường được sử dụng để phục hồi dữ liệu hoặc hệ thống tại thời điểm ảnh chụp. Bạn có thể quay trở lại trạng thái snapshot, nhưng không thể chọn một thời điểm cụ thể trong quá khứ

