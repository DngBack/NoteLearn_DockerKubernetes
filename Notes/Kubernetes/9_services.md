# Kubernetes Service & kubectl expose - Ghi chú tóm tắt

## 1. Mục tiêu

Tạo **Service** để expose (mở) ứng dụng đang chạy trong **Pod/Deployment** ra bên ngoài cluster, cho phép truy cập từ máy host hoặc Internet.

---

## 2. Các khái niệm chính

* **Pod**: đơn vị nhỏ nhất chạy container trong Kubernetes.
* **Deployment**: quản lý số lượng pod, tự động scale/update.
* **Service**: cung cấp một địa chỉ cố định (IP/Port) để truy cập đến các pod.
* **kubectl expose**: lệnh nhanh để tạo Service từ Deployment/Pod hiện có.
* **Minikube**: môi trường cluster cục bộ; có lệnh hỗ trợ đặc biệt để truy cập service.

---

## 3. Lệnh tạo Service bằng `kubectl expose`

```bash
kubectl expose deployment first-app --port=8080 --type=LoadBalancer
```

### Giải thích:

* `deployment first-app`: tạo service cho deployment tên `first-app`.
* `--port=8080`: cổng mà service lắng nghe.
* `--type=LoadBalancer`: chỉ định loại service.

> ⚠️ Nếu container lắng nghe ở cổng khác, thêm `--target-port=<container_port>`.

---

## 4. Các loại Service

| Loại             | Mô tả                                                              | Khi nào dùng                                            |
| ---------------- | ------------------------------------------------------------------ | ------------------------------------------------------- |
| **ClusterIP**    | Mặc định, chỉ truy cập trong cluster.                              | Dịch vụ nội bộ (DB, microservice nội bộ).               |
| **NodePort**     | Mở cổng trên mỗi node, truy cập qua `NODE_IP:NODE_PORT`.           | Khi cần truy cập từ bên ngoài mà không có LoadBalancer. |
| **LoadBalancer** | Cần hạ tầng hỗ trợ (AWS/GCP/Azure). Cấp IP ngoài và phân phối tải. | Khi triển khai trên cloud.                              |

---

## 5. Kiểm tra service

```bash
kubectl get services
# hoặc
kubectl get svc
```

Kết quả hiển thị: `NAME`, `TYPE`, `CLUSTER-IP`, `EXTERNAL-IP`, `PORT(S)`...

* Trên **cloud**: `EXTERNAL-IP` là địa chỉ thật.
* Trên **Minikube**: `EXTERNAL-IP` thường là `pending`.

---

## 6. Truy cập service trong Minikube

Khi dùng Minikube, `LoadBalancer` không tạo external IP, thay vào đó dùng:

```bash
minikube service first-app
```

👉 Tự mở trình duyệt tới địa chỉ dịch vụ.

Hoặc chỉ lấy URL:

```bash
minikube service first-app --url
```

### Ghi chú:

* Minikube chạy trong VM, nên IP public thật không tồn tại.
* Lệnh này ánh xạ port VM ra host để bạn truy cập cục bộ.
* Có thể dùng `minikube tunnel` để tạo external IP local (yêu cầu quyền admin).

---

## 7. Scale và cân bằng tải

Khi scale Deployment:

```bash
kubectl scale deployment first-app --replicas=3
```

Service tự động **load-balance** giữa các pod.

---

## 8. Ví dụ thực tế đầy đủ

```bash
# 1. Tạo deployment
kubectl create deployment first-app --image=my-node-app-image

# 2. Tạo service
kubectl expose deployment first-app --type=LoadBalancer --port=8080

# 3. Kiểm tra
kubectl get svc

# 4. Truy cập
minikube service first-app --url

# 5. Scale
kubectl scale deployment first-app --replicas=3
```

---

## 9. Lưu ý và mẹo thực tế

* Kiểm tra label: `kubectl get pods --show-labels` nếu Service không thấy pod.
* Nếu không vào được app → kiểm tra `targetPort` có khớp với `containerPort` không.
* Trên cloud, LoadBalancer có thể mất vài phút để cấp `EXTERNAL-IP`.

---

## 10. Tóm tắt nhanh

* `kubectl expose` = cách nhanh để tạo Service.
* 3 kiểu Service: `ClusterIP`, `NodePort`, `LoadBalancer`.
* Trên Minikube dùng `minikube service` để truy cập.
* Service + Deployment = mô hình cơ bản cho scale và load balancing.
