## Kubernetes là gì?

Trên trang chủ thì Kubernetes được mô tả là

“Hệ thống mã nguồn mở dùng để tự động hóa việc triển khai (deployment), mở rộng quy mô (scaling), và quản lý (management) các ứng dụng được đóng gói trong container.”

Như vậy, Kubernetes không phải là một phần mềm đơn lẻ, mà là một hệ thống hoàn chỉnh — bao gồm nhiều công cụ, thành phần, và quy trình giúp bạn quản lý việc triển khai các container một cách tự động, linh hoạt và ổn định.

Tuy nhiên, định nghĩa này vẫn khá trừu tượng. Vậy Kubernetes thực sự làm gì, sử dụng thế nào, và tại sao chúng ta cần nó?
Để hiểu rõ hơn, ta hãy quay lại vấn đề gốc: triển khai container (container deployment).

## Vấn đề khi triển khai container thủ công

Khi chúng ta triển khai container, nhất là trong môi trường thực tế, chúng ta có thể gặp nhiều vấn đề — đặc biệt nếu làm thủ công.

Giả sử ta tự tạo các máy chủ EC2 (máy ảo) trên AWS, cài Docker lên đó, rồi chạy các container của mình. Đây là cách triển khai thủ công nhất.
Cách này hoạt động được, nhưng đi kèm nhiều thách thức:

1. Quản lý bảo mật và cấu hình phức tạp

- Phải đảm bảo hệ điều hành và phần mềm trên máy chủ luôn được cập nhật.

- Phải tự cấu hình mạng, bảo mật, quyền truy cập, v.v.

- Tất cả đều do bạn tự làm.

2. Container có thể bị lỗi hoặc sập bất kỳ lúc nào

- Ứng dụng trong container có thể gặp lỗi.

- Khi đó container bị crash → ứng dụng không còn hoạt động.

- Bạn phải phát hiện và khởi động lại thủ công container bị lỗi.

- Điều này rõ ràng không khả thi nếu ứng dụng lớn hoặc hoạt động 24/7 — bạn không thể ngồi canh log cả ngày để khởi động lại container.

3. Không có khả năng tự động mở rộng (scaling)

- Khi lưu lượng truy cập (traffic) tăng cao, một container đơn lẻ có thể không xử lý kịp.

- Bạn phải tự tạo thêm container để chia tải.

- Khi traffic giảm, bạn phải tự xóa bớt để tiết kiệm tài nguyên.

- Quá trình này hoàn toàn thủ công và tốn công sức.

## Vấn đề về phân phối tải (Load Balancing)

Giả sử bạn có nhiều container đang chạy cùng một ứng dụng (ví dụ: 3 container phục vụ cùng một website).
Bạn cần đảm bảo rằng các yêu cầu (requests) từ người dùng được phân phối đều đến các container — thay vì dồn hết vào một container duy nhất.
Nếu không có cơ chế phân phối tải, bạn sẽ có:

- Một container quá tải, trong khi các container khác rảnh rỗi.

- Hiệu suất hệ thống không ổn định và dễ bị lỗi.
