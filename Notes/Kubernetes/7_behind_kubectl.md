# Behind Kubectl - Notes Markdown

## 1️⃣ Kubectl là gì?

* **CLI tool** để tương tác với Kubernetes cluster.
* Gửi lệnh tới **Control Plane (Master Node)** để tạo, cập nhật, xóa các object như Pod, Deployment, Service.
* Không tạo cluster, chỉ **quản lý và điều khiển** cluster.

---

## 2️⃣ Master Node và Worker Node

### Khi nào được tạo?

* **Khi cluster được khởi tạo**, không phải khi chạy kubectl.
* **Local (Minikube)**: chạy `minikube start` → tạo Master Node + Worker Node.
* **Cloud Cluster**: cloud provider tạo Master Node (managed) và Worker Node (VM/instance).

### Ở đâu?

* **Local**: trong VM hoặc container do Minikube tạo trên máy bạn.
* **Cloud**: trên hạ tầng cloud provider.

### Vai trò

| Node type   | Vai trò                                                          |
| ----------- | ---------------------------------------------------------------- |
| Master Node | Quản lý cluster, scheduler, API server, etcd, controller manager |
| Worker Node | Thực thi Pod, Container, giám sát trạng thái bằng kubelet        |

---

## 3️⃣ Workflow khi chạy `kubectl create deployment`

1. **Bạn chạy lệnh:**

```bash
kubectl create deployment first-app --image=mydockerid/myapp:latest
```

* Tạo **Deployment object** và gửi tới **Control Plane**.

2. **Control Plane xử lý:**

* Scheduler chọn Worker Node phù hợp.
* Tạo Pod dựa trên Deployment.
* Phân phối Pod tới Worker Node.

3. **Worker Node thực thi:**

* Kubelet tạo Pod & Container từ image.
* Giám sát trạng thái Pod & Container.

4. **Kết quả:**

* Pod & Container chạy ứng dụng Node.js.
* Ready để phục vụ traffic.

---

## 4️⃣ Sơ đồ trực quan

```
[ Kubectl create deployment ]
            │
            ▼
[ Control Plane (Master Node) ]
  ┌─────────────────────────┐
  │ Scheduler chọn Node      │
  │ Tạo Pod dựa trên Deployment │
  └─────────────────────────┘
            │
            ▼
[ Worker Node (kubelet) ]
  ┌─────────────────────────┐
  │ kubelet tạo Pod & Container │
  │ Giám sát Pod                │
  └─────────────────────────┘
            │
            ▼
     Pod & Container chạy ứng dụng
```

---

## 5️⃣ Key points

* **Kubectl là client**, Master & Worker Node là cluster.
* Người dùng **không cần quản lý trực tiếp Worker Node**; Kubernetes tự động.
* Master Node + Worker Node **được tạo khi cluster khởi tạo**, không phải lúc chạy kubectl.
* Kubectl có thể tương tác với **local cluster (Minikube)** hoặc **cloud cluster**.
* Việc chạy Deployment → Pod → Container **tất cả được tự động hóa** bởi Kubernetes.
* Bước tiếp theo là **expose ứng dụng** để truy cập từ bên ngoài cluster.
