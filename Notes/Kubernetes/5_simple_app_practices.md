# Kubernetes Node.js App Deployment Guide

## 1. Build Docker Image

```bash
# Build image from local Dockerfile
docker build -t kub-first-app .
```

## 2. Check Minikube Cluster

```bash
# Check if Minikube is running
minikube status

# If not running, start Minikube (Docker driver example)
minikube start --driver=docker
```

## 3. Deploy Node.js App (Local Image)

```bash
# Create deployment using local image (may fail in Minikube)
kubectl create deployment first-app --image=kub-first-app

# Check deployments
kubectl get deployments

# Check pods
kubectl get pods
```

> ⚠️ Likely error: `ImagePullBackOff` because Minikube VM cannot access local Docker image.

## 4. Push Image to Docker Hub

```bash
# Tag image with Docker Hub username
docker tag kub-first-app mydockerid/kub-first-app:latest

# Login to Docker Hub
docker login

# Push image to Docker Hub
docker push mydockerid/kub-first-app:latest
```

> Note: Must create repository `kub-first-app` on Docker Hub first.

## 5. Redeploy with Remote Image

```bash
# Delete old deployment
kubectl delete deployment first-app

# Create deployment using Docker Hub image
kubectl create deployment first-app --image=mydockerid/kub-first-app:latest

# Check deployment status
kubectl get deployments
kubectl get pods
```

## 6. Open Kubernetes Dashboard

```bash
minikube dashboard
```

> Dashboard shows deployments, pods, status, labels, and internal IP addresses.

## 7. Key Lessons Learned

* Kubernetes deploys **containers**, not source code.
* Docker image must be **accessible by the cluster**, either local (via Minikube Docker environment) or remote (Docker Hub).
* `kubectl create deployment` is **imperative approach**; later can use declarative YAML.
* Minikube provides a **web dashboard** to visualize cluster resources.
* Docker Hub repository must exist before `docker push`, cannot auto-create from CLI.
* Use proper **tags** when pushing to Docker Hub (`username/repo:tag`).
* For CI/CD, Docker Hub API or other registries (GitHub Container Registry) can automate repo creation.
