# Kubernetes Notes

## 1. Kiến thức cơ bản

- **Kubernetes (K8s)** là hệ thống điều phối container mã nguồn mở, giúp tự động hoá việc triển khai, mở rộng và quản lý ứng dụng container.
- Được Google phát triển, nay thuộc CNCF (Cloud Native Computing Foundation).
- Mục tiêu: chạy ứng dụng ổn định, dễ mở rộng, phục hồi lỗi, và quản lý tài nguyên hiệu quả.

---

## 2. Thành phần kiến trúc chính

### 2.1. Control Plane

- **API Server:** Giao tiếp trung tâm giữa người dùng và cluster.
- **etcd:** Cơ sở dữ liệu lưu trữ trạng thái cluster (key-value store).
- **Controller Manager:** Theo dõi và tự động khắc phục trạng thái hệ thống.
- **Scheduler:** Phân công Pod đến các Node phù hợp.

### 2.2. Node Components

- **Kubelet:** Agent chạy trên mỗi node, quản lý container.
- **Kube Proxy:** Quản lý mạng và cân bằng tải nội bộ.
- **Container Runtime:** (VD: Docker, containerd) để chạy container.

---

## 3. Các khái niệm cốt lõi

- **Pod:** Đơn vị triển khai nhỏ nhất trong Kubernetes, chứa một hoặc nhiều container.
- **Deployment:** Quản lý vòng đời của Pod, hỗ trợ rollback và scaling.
- **Service:** Tạo IP ổn định để truy cập Pod.
- **ConfigMap & Secret:** Quản lý cấu hình và dữ liệu nhạy cảm.
- **Namespace:** Phân tách tài nguyên trong cluster.

---

## 4. Cách hoạt động

1. Người dùng gửi lệnh (kubectl apply) đến API Server.
2. API Server ghi nhận thay đổi vào etcd.
3. Controller Manager giám sát và tạo tài nguyên tương ứng (Pod, Service,...).
4. Scheduler chọn Node phù hợp cho Pod.
5. Kubelet trên Node nhận lệnh và khởi tạo container qua Container Runtime.

---

## 5. Cài đặt môi trường Kubernetes

### 5.1. Windows

- Cài **kubectl**: tải từ [https://dl.k8s.io/release/v1.xx.x/bin/windows/amd64/kubectl.exe](https://dl.k8s.io/release/v1.xx.x/bin/windows/amd64/kubectl.exe)
- Cài **Minikube**: `choco install minikube`
- Bật Hyper-V hoặc Docker Desktop.
- Khởi động: `minikube start`

### 5.2. macOS

- Cài **kubectl**: `brew install kubectl`
- Cài **Minikube**: `brew install minikube`
- Khởi động: `minikube start --driver=docker`

### 5.3. Linux (Ubuntu/Debian)

- Cài **kubectl**:

  ```bash
  curl -LO https://dl.k8s.io/release/v1.xx.x/bin/linux/amd64/kubectl
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```

- Cài **Minikube**:

  ```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```

- Khởi động: `minikube start --driver=docker`

---

## 6. Lệnh cơ bản với kubectl

| Mục đích       | Lệnh                                     |
| -------------- | ---------------------------------------- |
| Kiểm tra nodes | `kubectl get nodes`                      |
| Kiểm tra pods  | `kubectl get pods -A`                    |
| Tạo tài nguyên | `kubectl apply -f file.yaml`             |
| Xoá tài nguyên | `kubectl delete -f file.yaml`            |
| Xem logs       | `kubectl logs <pod-name>`                |
| Truy cập shell | `kubectl exec -it <pod-name> -- /bin/sh` |

---

## 7. Gợi ý học tập & tài liệu

- Trang chủ: [https://kubernetes.io](https://kubernetes.io)
- Học qua thực hành: [Katacoda Kubernetes](https://www.katacoda.com/courses/kubernetes)
- Sách: _Kubernetes Up & Running_ (O’Reilly)

---

## 8. Ghi chú thêm

- Nắm vững **Pod Lifecycle** và **Service Types (ClusterIP, NodePort, LoadBalancer)**.
- Làm quen với YAML config.
- Hiểu rõ **kubectl context** khi làm việc với nhiều cluster.
- Có thể dùng **Lens IDE** để quan sát cluster dễ dàng hơn.
