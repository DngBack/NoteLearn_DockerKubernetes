## Tổng quan về Kubernetes

Kubernetes là một tập hợp các khái niệm và công cụ giúp bạn triển khai container ở bất kỳ nơi nào — trên cloud, on-premise, hoặc hybrid.

1. Pod - Đơn vị nhỏ nhất trong Kubernetes

Khi bạn muốn triển khai một container, trong Kubernetes nó sẽ được quản lý bởi một Pod.

Pod là đơn vị nhỏ nhất mà bạn có thể định nghĩa trong một file cấu hình của Kubernetes.

Một Pod có thể:

Chứa một container duy nhất (trường hợp phổ biến nhất).

Hoặc chứa nhiều container cần làm việc cùng nhau (ví dụ: một ứng dụng chính và một sidecar logging).

👉 Nói ngắn gọn: Pod = lớp vỏ bọc chạy container của bạn.

2. Worker Node – Nơi chạy các Pods

Mỗi Pod sẽ chạy trên một Worker Node.

Worker Node chính là máy chủ (thật hoặc ảo) cung cấp tài nguyên tính toán (CPU, RAM, Disk).

Ví dụ: trong AWS, một EC2 instance có thể được dùng làm Worker Node.

Trên mỗi Node có thể chạy nhiều Pod khác nhau nếu tài nguyên đủ.

Các thành phần chính trên một Worker Node:

Kubelet – quản lý các Pod trên Node, đảm bảo container đang chạy đúng như cấu hình.

Kube Proxy – quản lý network traffic, điều khiển việc:

Pods có thể giao tiếp ra Internet không.

Bên ngoài có thể truy cập Pod (ví dụ web app) như thế nào.

Container Runtime – ví dụ như Docker, containerd,… dùng để chạy container thực tế.

3. Master Node (Control Plane) – Trung tâm điều khiển

Mọi Worker Node và Pod đều được quản lý bởi Control Plane, nằm trên Master Node.

🎯 Vai trò:

Là “bộ não” của hệ thống Kubernetes.

Không trực tiếp chạy container, mà chỉ đạo các Worker Nodes làm việc.

Khi bạn triển khai một ứng dụng, bạn chỉ cần nói “Tôi muốn 3 Pod chạy app này”, và Control Plane sẽ tự phân bố các Pod đó lên các Worker Node thích hợp.

🧩 Thành phần chính trong Control Plane:

API Server – trung tâm tiếp nhận lệnh, là “cửa ngõ” để giao tiếp với Kubernetes.

Scheduler – quyết định Pod nào chạy ở Node nào.

Controller Manager – theo dõi trạng thái, tự khởi động lại hoặc thay thế Pod nếu bị lỗi.

etcd – cơ sở dữ liệu lưu toàn bộ trạng thái của cluster (như cấu hình, Pod đang chạy, service...).

☁️ 4. Cluster – Toàn bộ hệ thống Kubernetes

Cluster = Control Plane (Master Node) + các Worker Nodes.

Đây là một mạng lưới thống nhất nơi Master điều khiển Worker Nodes.

Trong thực tế:

Kubernetes có thể tương tác với API của nhà cung cấp cloud (AWS, GCP, Azure, v.v.).

Ví dụ: Kubernetes yêu cầu AWS tạo EC2 instances, Load Balancer, và các tài nguyên khác để triển khai hạ tầng tương ứng.

🧩 5. Khả năng mở rộng và phân tán

Khi nhu cầu tăng (traffic cao), Kubernetes có thể tự động thêm Pod mới (auto-scaling).

Các Pod này sẽ được phân bổ đều trên các Worker Node có sẵn.

Nếu một Node bị lỗi, Kubernetes sẽ chuyển Pod sang Node khác – đảm bảo tính khả dụng cao (high availability).
