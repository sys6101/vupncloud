# Thuật toán Raft
![Alt text](/Picture/Storage/raft.png)
## Khái niệm

Thuật toán Raft là một thuật toán phân phối quyết định trong hệ thống phân tán. Nó được thiết kế để đảm bảo tính nhất quán (consistency) và độ tin cậy (fault-tolerance) trong một hệ thống phân tán, đặc biệt là trong môi trường có khả năng xảy ra sự cố.

Thuật toán Raft dựa trên ý tưởng của một nhóm các máy chủ đồng thuận với nhau để chọn một lính đồng đảng (leader) duy nhất. Các máy chủ còn lại được gọi là các người đồng đảng (followers). Leader đảm nhận vai trò chịu trách nhiệm cho việc nhận yêu cầu từ các khách hàng (clients), nhất quán hóa dữ liệu và phân phối kết quả cho các người đồng đảng.

## Hoạt động

### Election (Bầu cử):

![Alt text](/Picture/Storage/raft1.png)

Mỗi người đồng đảng có một thời gian chờ ngẫu nhiên và trong thời gian này, nếu không nhận được tin nhắn từ leader, họ sẽ tự xem mình là ứng cử viên (candidate) và bắt đầu quá trình bầu cử.

Ứng cử viên gửi các yêu cầu bầu cử đến các người đồng đảng khác và nếu nhận được đa số phiếu ủng hộ, ứng cử viên sẽ trở thành leader.

Nếu một người đồng đảng nhận được yêu cầu bầu cử từ ứng cử viên, họ sẽ phản hồi với phiếu ủng hộ và chấp nhận ứng cử viên làm leader.

Trong thuật toán Raft, quá trình bầu cử trưởng nhóm (leader election) sẽ diễn ra như sau trong trường hợp cluster có 3 thành viên:

- Mỗi thành viên sẽ tự đề cử mình làm trưởng nhóm (leader) bằng cách gửi yêu cầu bầu cử (election request) đến các thành viên khác trong cluster.
- Khi một thành viên nhận được yêu cầu bầu cử, nó sẽ kiểm tra xem nếu nó chưa bầu cử cho ai hoặc nếu nó đã bầu cử cho một thành viên nào đó mà không thành công (ví dụ như không nhận được phản hồi), nó sẽ đồng ý bầu cử cho thành viên mới đề cử.
- Khi một thành viên nhận được đủ số phiếu bầu (quorum) từ các thành viên khác trong cluster, nó sẽ trở thành trưởng nhóm và gửi thông báo bầu cử thành công (election success) đến các thành viên khác trong cluster.
Các thành viên khác sẽ nhận được thông báo này và chuyển sang chế độ đồng bộ hóa với trưởng nhóm mới.       

Trong trường hợp cluster có 3 thành viên, để đảm bảo tính nhất quán, cần có ít nhất 2 phiếu bầu (quorum) để đưa ra quyết định cho một hoạt động. Do đó, để bầu cử thành công, cần có ít nhất 2 thành viên đồng ý bầu cử cho cùng một thành viên đề cử. Nếu chỉ có 1 hoặc không có thành viên nào đồng ý bầu cử cho cùng một thành viên đề cử, quá trình bầu cử sẽ không thành công và quá trình bầu cử sẽ được lặp lại.
### Leader Operation (Hoạt động của leader):

Leader nhận yêu cầu từ khách hàng và phân phối các yêu cầu cho các người đồng đảng.
Leader gửi các tin nhắn heartbeats định kỳ cho các người đồng đảng để duy trì sự nhất quán và tránh bầu cử mới.
### Log Replication (Sao chép nhật ký):
![Alt text](/Picture/Storage/raft2.png)
Mỗi yêu cầu được gửi bởi khách hàng được ghi vào log của leader.
Leader phân phối log cho các người đồng đảng và đợi cho đến khi log được sao chép đến đa số các người đồng đảng trước khi áp dụng yêu cầu.
### Safety (An toàn):

Raft đảm bảo tính nhất quán bằng cách đảm bảo rằng một yêu cầu chỉ được coi là đã thực hiện khi nó đã được áp dụng bởi leader và đa số các người đồng đảng.
Raft sử dụng cơ chế đồng thuận để đảm bảo rằng một yêu cầu sẽ không bị mất và sẽ không bị xung đột.     

Thuật toán Raft thiết kế một giao thức đồng thuận đơn giản, dễ hiểu và dễ triển khai. Nó cung cấp tính nhất quán và độ tin cậy trong hệ thống phân tán, giúp giảm thiểu sự cố và tăng tính sẵn sàng.

Giả sử bạn có một cluster Raft với 3 node A, B và C, trong đó node A là leader. Khi một node bị down, quá trình được mô phỏng như sau:

- Node A sẽ gửi các entries mới nhất đến node B và C, để đảm bảo rằng các node này có dữ liệu mới nhất. 

![Alt text](/Picture/Storage/image.png)      
- Nếu node A bị down, node B và C sẽ bắt đầu bầu cử (election) để chọn ra một leader mới. Các node sẽ gửi các yêu cầu bầu cử (request for votes) đến các node khác trong cluster.

![Alt text](/Picture/Storage/image-1.png)

- Nếu một node nhận được đa số phiếu (quá nửa số node trong cluster), nó sẽ trở thành leader mới. Nếu không, các node sẽ tiếp tục gửi yêu cầu bầu cử cho đến khi có leader mới được chọn.

- Khi một leader mới được chọn, các node khác sẽ yêu cầu leader mới nhận các entries mới nhất từ node trước đó đến thời điểm hiện tại.

- Leader mới sẽ gửi các entries mới nhất đến các node khác trong cluster để đồng bộ hóa dữ liệu.

Các node trong cluster sẽ kiểm tra xem trạng thái của chúng có phù hợp với trạng thái của leader mới không. Nếu không, chúng sẽ cập nhật trạng thái của mình để phù hợp với trạng thái của leader mới.

Khi quá trình đồng bộ hoàn tất, cluster sẽ tiếp tục hoạt động với leader mới và các node khác như bình thường.

Trong quá trình này, Raft đảm bảo rằng các entries mới nhất được sao chép đến các node khác trong cluster và leader mới được chọn một cách an toàn và đáng tin cậy.
# Thuật toán Paxos

## Giới thiệu
- Trong một mạng, để đánh giá xem một process failure cần dựa trên kết quả đánh giá của đa số processes khác.
- Giao thức PAXOS ra đời để giải quyết vấn đề đánh giá này.
- Một process được coi là failure khi có nhiều hơn một nửa các processes nhận thấy rằng process đó đã failure (lý do có thể do mất kết nối, không running, etc)

Thuật toán Paxos là một thuật toán đồng thuận phân phối được sử dụng trong hệ thống phân tán. Nó được thiết kế để đảm bảo tính nhất quán (consistency) và độ tin cậy (fault-tolerance) trong một môi trường phân phối có khả năng xảy ra sự cố.

## Dưới đây là một tóm tắt ngắn gọn về thuật toán Paxos:

### Roles (Vai trò):

- Proposer: Người đề xuất một giá trị để đồng thuận.

- Acceptor: Người nhận và xử lý các đề xuất từ proposer.

- Learner: Người nhận thông tin về giá trị đã được đồng thuận.
### Phases (Các giai đoạn):

- Prepare (Giai đoạn chuẩn bị): Proposer gửi một tin nhắn "prepare" chứa một số tiền đề (ballot number) đến tất cả các acceptor. Acceptors sẽ kiểm tra số tiền đề và trả về thông tin về các giá trị đã chấp nhận trước đó (nếu có).

- romise : Acceptors phản hồi tin nhắn "promise" cho proposer với số tiền đề cao nhất mà họ đã nhận được và thông tin về giá trị đã chấp nhận trước đó (nếu có).

- Accept (Giai đoạn chấp nhận): Nếu proposer nhận được đủ số lượng chứa từ acceptors và không có bất kỳ acceptor nào đã chấp nhận giá trị khác, proposer gửi một tin nhắn "accept" chứa giá trị mới đến tất cả các acceptors.      

- Accepted (Giai đoạn đã chấp nhận): Acceptors nhận tin nhắn "accept" từ proposer và gửi tin nhắn "accepted" đến tất cả các learners để thông báo về giá trị đã chấp nhận.
### Safety (An toàn):

Paxos đảm bảo tính nhất quán bằng cách đảm bảo rằng chỉ có một giá trị duy nhất được chấp nhận và đồng thuận trong mỗi giai đoạn.           

Thuật toán Paxos phức tạp trong việc triển khai và hiểu đối với người mới, nhưng nó cung cấp tính nhất quán và độ tin cậy trong môi trường phân tán. Nó đã được sử dụng rộng rãi trong các hệ thống phân tán như các hệ thống cơ sở dữ liệu phân tán và hệ thống điều phối tài nguyên.


Trong hệ thống phân tán Paxos, nếu một server bị down, các phase sẽ hoạt động như sau:

- Trong phase "prepare": Nếu server bị down là một acceptor, nó sẽ không thể gửi thông điệp "promise" cho proposer, và proposer sẽ không nhận được đủ số lượng thông điệp "promise" để tiếp tục đề xuất. Tuy nhiên, nếu server bị down là một proposer, các acceptor vẫn có thể gửi thông điệp "promise" và thuật toán vẫn có thể tiếp tục hoạt động.

- Trong phase "accept": Nếu server bị down là một acceptor, nó sẽ không thể gửi thông điệp "accepted" để thông báo rằng giá trị đề xuất đã được chấp nhận. Tuy nhiên, các acceptor khác vẫn có thể gửi thông điệp "accepted" và proposer sẽ tiếp tục tiến hành các bước để đồng bộ hóa giá trị đề xuất.

- Sau khi một giá trị đề xuất đã được chấp nhận: Nếu server bị down là một nút trong hệ thống phân tán, các nút khác vẫn có thể tiếp tục thực hiện các hành động cần thiết để xử lý giá trị đề xuất. 


![Alt text](/Picture/Storage/paxos.png)          

- [1] Proposer gửi một thông điệp "prepare" với một số nhận dạng duy nhất (n).  

- [2] Mỗi acceptor kiểm tra xem nếu n lớn hơn hoặc bằng số phiên bản lớn nhất mà nó đã đồng ý trước đó (vn), nó sẽ gửi một thông điệp "promise" cho proposer, bao gồm số phiên bản lớn nhất mà nó đã đồng ý trước đó.    
 
- [3] Nếu proposer nhận được đủ số lượng thông điệp "promise" từ các acceptor, nó sẽ gửi một thông điệp "accept" tới tất cả các acceptor, bao gồm nhận dạng duy nhất (n) và giá trị đề xuất (v).          
- [4] Mỗi acceptor sẽ kiểm tra xem giá trị đề xuất này chưa từ chối bởi bất kỳ acceptor nào khác, nếu chưa, nó sẽ gửi thông điệp "accepted" cho proposer, để thông báo rằng giá trị đề xuất đã được chấp nhận.       

- [5] Sau khi một giá trị đề xuất đã được chấp nhận bởi đủ số lượng acceptor giá trị đề xuất đã được chấp nhận và được áp dụng trên toàn bộ hệ thống.

### Số lượng node
Trong Paxos và Raft, số lượng node được khuyến nghị nên là số lẻ (ví dụ: 3, 5, 7, 9) để đảm bảo tính sẵn sàng cao và khả năng chịu lỗi tốt hơn. Điều này liên quan đến cách các giao thức phân tán này hoạt động.

Trong Paxos, số lượng node được khuyến nghị nên là từ 3 đến 5. Trong một cluster Paxos với số lượng node là lẻ, khi một node gặp sự cố hoặc bị ngắt kết nối, các node còn lại vẫn có thể hoạt động để đạt được sự đồng bộ và đồng nhất. Trong trường hợp có số lượng node là chẵn, tình huống có thể xảy ra là hai nhóm node có số lượng bằng nhau không thể đạt được đồng thuận vì mỗi nhóm có số phiếu bình đẳng.

Tương tự, trong Raft, số lượng node khuyến nghị cũng nên là số lẻ để đảm bảo tính sẵn sàng cao và khả năng chịu lỗi tốt hơn. Với số lượng node lẻ, các quyết định có thể được đưa ra theo đa số. Ví dụ, trong một cluster Raft có 5 node, để đạt được đồng thuận, ít nhất 3 node phải đồng ý với quyết định đó.
```
3 node cluster can handle 1 node fail (the majority is 2 nodes).
4 node cluster can handle 1 node fail (the majority is 3 nodes).
5 node cluster can handle 2 node fail (the majority is 3 nodes).
6 node cluster can handle 2 node fail (the majority is 4 nodes).
```





