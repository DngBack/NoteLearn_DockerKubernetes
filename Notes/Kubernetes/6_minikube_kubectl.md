# Kubernetes, Minikube và Kubectl - Tổng hợp Note

## 1. Minikube là gì?

* Minikube là một **local Kubernetes cluster** chạy trên máy của bạn.
* Nó tạo ra **một môi trường cluster ảo** (control plane + node) để bạn deploy và test ứng dụng.
* **Vai trò chính:** cung cấp môi trường để chạy Pod, Deployment, Service và các object Kubernetes khác.

**Các lệnh cơ bản:**

```bash
minikube start           # khởi động cluster
minikube status          # kiểm tra cluster
minikube stop            # tạm dừng cluster
minikube delete          # xóa cluster
minikube dashboard       # mở giao diện trực quan
```

---

## 2. Kubectl là gì?

* Kubectl là **CLI để tương tác với Kubernetes cluster**.
* Nó gửi lệnh tới **API server** của cluster để tạo, cập nhật, xóa các object: Pod, Deployment, Service...
* Kubectl **không tạo cluster**, mà **tương tác và quản lý cluster** đã có.

**Ví dụ lệnh:**

```bash
kubectl get pods
kubectl get deployments
kubectl describe pod <name>
kubectl create deployment first-app --image=mydockerid/myapp:latest
kubectl delete deployment first-app
```

---

## 3. Mối quan hệ giữa Minikube và Kubectl

* **Minikube = cluster (môi trường thực thi)**
* **Kubectl = bộ điều khiển từ xa (CLI)**

**Workflow:**

```
kubectl lệnh → Minikube API server → Cluster tạo/manage Deployment & Pod → Pod chạy container
```

* Minikube chạy cluster → kubectl gửi lệnh → cluster tạo resource → Pod chạy container.
* Không có Minikube → kubectl không có cluster để thao tác.
* Không có kubectl → cluster vẫn chạy nhưng bạn không thể quản lý từ CLI.

---

## 4. Công cụ khác liên quan

| Công cụ               | Vai trò                                                 |
| --------------------- | ------------------------------------------------------- |
| Docker                | Build container image từ source code, push lên registry |
| Docker Hub / Registry | Lưu trữ image, cluster pull image khi deploy            |
| YAML / Helm           | Triển khai declarative, define Deployment/Pod/Service   |
| Minikube Dashboard    | GUI hiển thị cluster resources                          |
| Minikube addons       | Metrics, Ingress, Dashboard, v.v.                       |

---

## 5. Tổng quan workflow từ code đến chạy ứng dụng

```
[Source Code (Node.js)]
       │
       ▼
[Docker Build] → Docker Image (local)
       │
       ▼
[Docker Push] → Docker Hub / Registry
       │
       ▼
[Minikube Cluster]
       │
       ▼
[kubectl Apply/Create] → Deployment/Pod/Service
       │
       ▼
[Pod chạy container]
       │
       ▼
[Browser / External Access] (qua Service/NodePort/Ingress)
```

* Code → Docker → Registry → Minikube → kubectl → Pod → Browser

---

## 6. Key lessons

* Kubernetes deploys **containers**, không deploy trực tiếp source code.
* Docker image phải **accessible** với cluster (local hoặc registry).
* Kubectl là **công cụ CLI để điều khiển cluster**, Minikube là **cluster local**.
* Minikube dashboard giúp **quan sát trạng thái cluster, pod, deployment**.
* YAML/Helm giúp triển khai **declarative**, dễ quản lý nhiều resources.
* Docker Hub repo phải **tạo trước khi push**, không thể tự tạo từ `docker push`.
* Dùng tags rõ ràng khi push image (`username/repo:tag`).

# Tại sao tách Minikube và Kubectl trong Kubernetes

## 1️⃣ Lý do tách riêng

1. **Minikube = cluster, Kubectl = client**

   * Minikube: tạo và chạy cluster local (control plane + node)
   * Kubectl: tương tác với cluster qua API server

2. **Tách client và server giúp linh hoạt**

   * Kubernetes thiết kế theo mô hình **client-server**
   * Một **kubectl duy nhất** có thể quản lý nhiều cluster (local, cloud)

3. **Triển khai cloud dễ dàng**

   * Không cần cài full cluster local trên máy vẫn quản lý được cluster từ xa

4. **Minikube là tool học tập / test**

   * Minikube chỉ mô phỏng cluster local để học tập hoặc thử nghiệm
   * Kubectl dùng được cho Minikube và cả cluster production

---

## 2️⃣ Ví dụ trực quan

| Công cụ  | Tác dụng                                                    | Sử dụng ở đâu                            |
| -------- | ----------------------------------------------------------- | ---------------------------------------- |
| Minikube | Khởi tạo cluster local, chạy pod, deployment                | Local PC để học, test                    |
| Kubectl  | Tương tác cluster (tạo pod, deployment, service, xem logs…) | Local hoặc remote cluster, cloud cluster |

**Mối quan hệ:**

```
         ┌────────────┐
         │  Kubectl   │
         └─────┬──────┘
               │ gửi lệnh
       ┌───────▼────────┐
       │  Minikube      │  <-- Local cluster
       └────────────────┘

         ┌────────────┐
         │  Kubectl   │
         └─────┬──────┘
               │ gửi lệnh
       ┌───────▼────────┐
       │ Cloud Cluster  │  <-- Production cluster
       └────────────────┘
```

* **Kubectl** là universal client, có thể gửi lệnh đến bất kỳ cluster nào.
* **Minikube** là một trong các loại cluster mà kubectl thao tác.

---

## 3️⃣ Tóm tắt

* Tách Minikube và Kubectl giúp:

  * Linh hoạt, học tập dễ dàng
  * Dùng kubectl cho nhiều cluster khác nhau
  * Tuân theo kiến trúc **client-server** chuẩn của Kubernetes
