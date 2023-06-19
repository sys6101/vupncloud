# Thuật toán Raft
![Alt text](/Picture/Storage/raft.png)
## Khái niệm

Thuật toán Raft là một thuật toán phân phối quyết định trong hệ thống phân tán. Nó được thiết kế để đảm bảo tính nhất quán (consistency) và độ tin cậy (fault-tolerance) trong một hệ thống phân tán, đặc biệt là trong môi trường có khả năng xảy ra sự cố.

Thuật toán Raft dựa trên ý tưởng của một nhóm các máy chủ đồng thuận với nhau để chọn một lính đồng đảng (leader) duy nhất. Các máy chủ còn lại được gọi là các người đồng đảng (followers). Leader đảm nhận vai trò chịu trách nhiệm cho việc nhận yêu cầu từ các khách hàng (clients), nhất quán hóa dữ liệu và phân phối kết quả cho các người đồng đảng.

## Hoạt động

### Election (Bầu cử):

Mỗi người đồng đảng có một thời gian chờ ngẫu nhiên và trong thời gian này, nếu không nhận được tin nhắn từ leader, họ sẽ tự xem mình là ứng cử viên (candidate) và bắt đầu quá trình bầu cử.
Ứng cử viên gửi các yêu cầu bầu cử đến các người đồng đảng khác và nếu nhận được đa số phiếu ủng hộ, ứng cử viên sẽ trở thành leader.
Nếu một người đồng đảng nhận được yêu cầu bầu cử từ ứng cử viên, họ sẽ phản hồi với phiếu ủng hộ và chấp nhận ứng cử viên làm leader.
### Leader Operation (Hoạt động của leader):

Leader nhận yêu cầu từ khách hàng và phân phối các yêu cầu cho các người đồng đảng.
Leader gửi các tin nhắn heartbeats định kỳ cho các người đồng đảng để duy trì sự nhất quán và tránh bầu cử mới.
### Log Replication (Sao chép nhật ký):

Mỗi yêu cầu được gửi bởi khách hàng được ghi vào log của leader.
Leader phân phối log cho các người đồng đảng và đợi cho đến khi log được sao chép đến đa số các người đồng đảng trước khi áp dụng yêu cầu.
### Safety (An toàn):

Raft đảm bảo tính nhất quán bằng cách đảm bảo rằng một yêu cầu chỉ được coi là đã thực hiện khi nó đã được áp dụng bởi leader và đa số các người đồng đảng.
Raft sử dụng cơ chế đồng thuận để đảm bảo rằng một yêu cầu sẽ không bị mất và sẽ không bị xung đột.     

Thuật toán Raft thiết kế một giao thức đồng thuận đơn giản, dễ hiểu và dễ triển khai. Nó cung cấp tính nhất quán và độ tin cậy trong hệ thống phân tán, giúp giảm thiểu sự cố và tăng tính sẵn sàng.
# Thuật toán Paxos

## Giới thiệu
- Trong một mạng, để đánh giá xem một process failure cần dựa trên kết quả đánh giá của đa số processes khác.
- Giao thức PAXOS ra đời để giải quyết vấn đề đánh giá này.
- Một process được coi là failure khi có nhiều hơn một nửa các processes nhận thấy rằng process đó đã failure (lý do có thể do mất kết nối, không running, etc)

Thuật toán Paxos là một thuật toán đồng thuận phân phối được sử dụng trong hệ thống phân tán. Nó được thiết kế để đảm bảo tính nhất quán (consistency) và độ tin cậy (fault-tolerance) trong một môi trường phân phối có khả năng xảy ra sự cố.

## Dưới đây là một tóm tắt ngắn gọn về thuật toán Paxos:

### Roles (Vai trò):

Proposer (Người đề xuất): Người đề xuất một giá trị để đồng thuận.

Acceptor (Người chấp nhận): Người nhận và xử lý các đề xuất từ proposer.

Learner (Người học): Người nhận thông tin về giá trị đã được đồng thuận.
### Phases (Các giai đoạn):

Prepare (Giai đoạn chuẩn bị): Proposer gửi một tin nhắn "prepare" chứa một số tiền đề (ballot number) đến tất cả các acceptor. Acceptors sẽ kiểm tra số tiền đề và trả về thông tin về các giá trị đã chấp nhận trước đó (nếu có).

Promise (Giai đoạn hứa): Acceptors phản hồi tin nhắn "promise" cho proposer với số tiền đề cao nhất mà họ đã nhận được và thông tin về giá trị đã chấp nhận trước đó (nếu có).

Accept (Giai đoạn chấp nhận): Nếu proposer nhận được đủ số lượng hứa từ acceptors và không có bất kỳ acceptor nào đã chấp nhận giá trị khác, proposer gửi một tin nhắn "accept" chứa giá trị mới đến tất cả các acceptors.      

Accepted (Giai đoạn đã chấp nhận): Acceptors nhận tin nhắn "accept" từ proposer và gửi tin nhắn "accepted" đến tất cả các learners để thông báo về giá trị đã chấp nhận.
### Safety (An toàn):

Paxos đảm bảo tính nhất quán bằng cách đảm bảo rằng chỉ có một giá trị duy nhất được chấp nhận và đồng thuận trong mỗi giai đoạn.           

Thuật toán Paxos phức tạp trong việc triển khai và hiểu đối với người mới, nhưng nó cung cấp tính nhất quán và độ tin cậy trong môi trường phân tán. Nó đã được sử dụng rộng rãi trong các hệ thống phân tán như các hệ thống cơ sở dữ liệu phân tán và hệ thống điều phối tài nguyên.




