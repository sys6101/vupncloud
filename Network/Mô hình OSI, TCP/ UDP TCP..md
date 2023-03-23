# UDP and TCP
UDP (User Datagram Protocol) và TCP (Transmission Control Protocol) là hai giao thức truyền tải dữ liệu quan trọng trong mô hình TCP/IP. Dưới đây là sự khác biệt giữa UDP và TCP:

- Độ tin cậy: TCP cung cấp độ tin cậy khi truyền tải dữ liệu bằng cách đảm bảo rằng tất cả các gói tin được gửi đến đích và đúng thứ tự. UDP không cung cấp độ tin cậy và có thể gửi các gói tin mất mát hoặc trùng lặp.

- Kiểm soát lỗi: TCP sử dụng kiểm soát lỗi để đảm bảo độ tin cậy trong khi UDP không sử dụng kiểm soát lỗi.

- Phản hồi: TCP yêu cầu phản hồi từ người nhận để xác nhận rằng dữ liệu đã được gửi thành công. UDP không yêu cầu phản hồi.

- Kích thước gói tin: Kích thước gói tin tối đa của UDP là 65.535 byte, trong khi kích thước gói tin tối đa của TCP là 65.535 byte nếu sử dụng tối đa cửa sổ trượt.

- Thời gian phản hồi: Thời gian phản hồi của UDP nhanh hơn so với TCP do UDP không phải chờ đợi phản hồi từ người nhận.

- Ứng dụng: UDP thường được sử dụng cho các ứng dụng cần tốc độ truyền tải dữ liệu cao như trò chơi trực tuyến, truyền dữ liệu trực tiếp (streaming), v.v. TCP thường được sử dụng cho các ứng dụng cần độ tin cậy cao như email, truyền tải tập tin, v.v.

Tóm lại, UDP và TCP đều là giao thức truyền tải dữ liệu quan trọng trong mô hình TCP/IP. UDP cung cấp tốc độ truyền tải dữ liệu nhanh hơn, nhưng không đảm bảo độ tin cậy và không yêu cầu phản hồi. Trong khi đó, TCP đảm bảo độ tin cậy khi truyền tải dữ liệu, nhưng thời gian truyền tải chậm hơn do yêu cầu phản hồi từ người nhận. Việc lựa chọn sử dụng UDP hay TCP phụ thuộc vào yêu cầu của ứng dụng cụ thể.