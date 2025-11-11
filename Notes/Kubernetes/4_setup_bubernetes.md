# Hướng dẫn cài đặt Kubernetes trên Windows, macOS và Linux

Kubernetes là hệ thống quản lý container mạnh mẽ, giúp triển khai, quản lý và mở rộng ứng dụng dễ dàng. Dưới đây là hướng dẫn setup môi trường Kubernetes trên ba hệ điều hành phổ biến.

---

## 1. Yêu cầu cơ bản

Trước khi cài đặt, đảm bảo các yêu cầu sau:

- **CPU & RAM**: ít nhất 2 CPU và 4GB RAM (khuyến nghị 4 CPU + 8GB RAM).
- **Phần mềm cần thiết**:

  - Docker
  - Kubectl (CLI của Kubernetes)
  - Minikube (để chạy Kubernetes local)

- **Quyền quản trị**: cần quyền `sudo` trên Linux/macOS hoặc quyền Administrator trên Windows.
- Kết nối Internet để tải image và packages.

---

## 2. Cài đặt Docker

Docker là nền tảng container cần thiết để chạy Pods.

### Windows

1. Tải [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop).
2. Cài đặt và bật **WSL 2** nếu được yêu cầu.
3. Khởi động Docker Desktop và đảm bảo Docker daemon đang chạy.

### macOS

1. Tải [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop).
2. Cài đặt và chạy Docker.
3. Kiểm tra:

   ```bash
   docker --version
   ```

### Linux (Ubuntu/Debian)

1. Cài đặt Docker:

   ```bash
   sudo apt update
   sudo apt install -y docker.io
   sudo systemctl enable --now docker
   ```

2. Thêm user vào group docker (không cần sudo mỗi lần):

   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```

---

## 3. Cài đặt Kubectl

`kubectl` là CLI dùng để quản lý Kubernetes cluster.

### Windows

- Dùng PowerShell với quyền Administrator:

  ```powershell
  curl -LO "https://dl.k8s.io/release/v1.29.0/bin/windows/amd64/kubectl.exe"
  move .\kubectl.exe C:\Windows\System32\
  ```

### macOS

```bash
brew install kubectl
```

### Linux

```bash
curl -LO "https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```

---

## 4. Cài đặt Minikube

Minikube giúp chạy cluster Kubernetes trên máy local.

### Windows

1. Tải [Minikube Windows](https://minikube.sigs.k8s.io/docs/start/).
2. Cài đặt và thêm vào PATH.
3. Khởi chạy cluster:

   ```powershell
   minikube start --driver=docker
   ```

### macOS

```bash
brew install minikube
minikube start --driver=docker
```

### Linux

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=docker
```

---

## 5. Kiểm tra cluster

```bash
kubectl get nodes
kubectl get pods -A
minikube status
```

- `kubectl get nodes`: hiển thị Worker Node đang chạy.
- `kubectl get pods -A`: hiển thị tất cả Pods trong cluster.
- `minikube status`: kiểm tra trạng thái cluster.

---

## 6. Công cụ bổ sung (tùy chọn)

- **Helm**: quản lý chart và deploy ứng dụng trên Kubernetes.
- **K9s**: terminal UI quản lý cluster.
- **Lens**: GUI quản lý cluster Kubernetes.
- **kubectl plugins**: mở rộng chức năng `kubectl`.

---

## 7. Tổng kết

Sau khi setup xong:

1. Docker đang chạy và có thể tạo container.
2. Minikube tạo cluster Kubernetes local với ít nhất 1 Node.
3. `kubectl` có thể giao tiếp với cluster.

Bạn đã sẵn sàng để tạo **Pods**, **Deployments**, **Services** và triển khai ứng dụng trên Kubernetes.

---

**Tài liệu tham khảo:**

- [Kubernetes Official Docs](https://kubernetes.io/docs/home/)
- [Minikube Docs](https://minikube.sigs.k8s.io/docs/)
- [Docker Docs](https://docs.docker.com/)
