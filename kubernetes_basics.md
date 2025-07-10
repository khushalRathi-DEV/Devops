
# Kubernetes: Getting Started with Minikube on Windows

This document captures my first hands-on experience with Kubernetes using Minikube and Docker Desktop as the driver. I deployed a sample Nginx application, exposed it, accessed it via browser, and cleaned up afterward.

---

## ✅ Environment Details

- **OS**: Windows 11 Home Single Language (Build 26100.4652)
- **Minikube Version**: v1.36.0
- **Docker**: Docker Desktop (driver for Minikube)
- **Kubernetes Version**: v1.33.1

---

## 🚀 Steps Performed

### 1. Start Minikube

```sh
minikube start
```

> Minikube automatically selected Docker as the driver and pulled the base image. A fallback image was used from Docker Hub due to connectivity issues with `gcr.io`.

---

### 2. Verify Cluster Context

```sh
kubectl config current-context
kubectl config get-contexts
```

Expected output:
```
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   default
```

---

### 3. Deploy a Sample App (Nginx)

```sh
kubectl create deployment my-nginx --image=nginx:latest
```

Check deployment status:
```sh
kubectl get deployments
kubectl get pods
```

> Initially, `READY` was 0/1. After a few seconds, pod status became `Running` and `READY` changed to 1/1.

---

### 4. Launch Kubernetes Dashboard

```sh
minikube dashboard
```

> Opened the web dashboard at a local proxy URL.  
> Optional: Enable full metrics with:

```sh
minikube addons enable metrics-server
```

---

### 5. Expose the Deployment

```sh
kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
```

Check the service:
```sh
kubectl get services
```

> EXTERNAL-IP shows `<pending>`, which is expected with Minikube. To get the actual access URL:

```sh
minikube service my-nginx
```

> Output:
```
http://192.168.49.2:32203
```

Also starts a tunnel and opens service in the browser at:
```
http://127.0.0.1:<some-port>
```

---

### 6. Clean Up

Delete the deployment:

```sh
kubectl delete deployment my-nginx
```

Check to confirm resources are removed:

```sh
kubectl get deployments
kubectl get pods
```

---

### 7. Delete Minikube Cluster

```sh
minikube delete
```

> This removes the Docker container and local config files.

Check Docker:

```sh
docker ps
```

> No containers should be running.

---

## 📘 Learnings & Notes

- Minikube is a great way to set up a local Kubernetes cluster for development.
- When using the Docker driver on Windows, Minikube creates a container that acts as a VM.
- The `minikube service` command is crucial for accessing exposed services locally.
- The Kubernetes Dashboard is useful for visualizing workloads and services.
- `kubectl` is the command-line tool to interact with Kubernetes clusters.

---

## 🔧 Troubleshooting

- ❗ If `EXTERNAL-IP` is stuck at `<pending>`, use `minikube service <name>` instead.
- ❗ If you see issues pulling images from `gcr.io`, Minikube may fallback to Docker Hub images.
- ⚠️ When using Docker driver, you must keep the tunnel terminal open for `LoadBalancer` services.

---

## 📂 Next Steps

- Explore Kubernetes concepts like:
  - Services (NodePort vs LoadBalancer)
  - ConfigMaps and Secrets
  - Ingress
  - Helm Charts
- Try creating multi-container applications using `kubectl apply -f <yaml>`.

---

## 🧠 Commands Reference

```sh
minikube start
kubectl create deployment my-nginx --image=nginx:latest
kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
kubectl get pods
kubectl get services
kubectl delete deployment my-nginx
minikube delete
```

---

## 📅 Date

**10th July 2025**

---

## ✍️ Author

Khushal Rathi  
[GitHub](https://github.com/khushalRathi-DEV)  
