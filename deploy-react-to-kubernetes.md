
# 🚀 Deploying My First React App to Kubernetes with Minikube

This document details my first successful deployment of a Dockerized React application to a local Kubernetes cluster using Minikube on Windows.

---

## 📦 App Overview

- **App Type**: React Web App
- **Docker Image**: `khushal2204/webapp-demo:01`
- **Container Port**: 5173 (React Vite default port)

---

## 🧱 Step-by-Step Process

### 1. ✅ Docker Build

```sh
docker build -t khushal2204/webapp-demo:01 .
```

> This builds the React app image locally using the Dockerfile in the project directory.

Check the image:
```sh
docker images
```

### 2. 🔐 Docker Login & Push

```sh
docker login
docker push khushal2204/webapp-demo:01
```

> Pushes the image to Docker Hub so Kubernetes can pull it.

---

### 3. 🧰 Start Minikube

```sh
minikube start
```

> Starts a local Kubernetes cluster using Docker Desktop as the driver.

Check cluster status:

```sh
minikube status
```

---

### 4. 📦 Deploy App to Kubernetes

```sh
kubectl create deployment my-webapp --image=khushal2204/webapp-demo:01
kubectl get deployment
kubectl get pods
```

> A deployment and pod for the app is created. Wait until pod is in `Running` state.

---

### 5. 🌐 Expose the App via LoadBalancer

```sh
kubectl expose deployment my-webapp --type=LoadBalancer --port=5173
kubectl get services
```

> This exposes the app on the cluster. Since Minikube runs locally, `EXTERNAL-IP` will be `<pending>`.

Use this to access the app:

```sh
minikube service my-webapp
```

Expected output includes:
```
http://192.168.49.2:<node-port>
```

> This URL will open in your default browser and show the deployed React app.

---

### 6. 🔁 Redeploying (If Needed)

If you need to delete and re-deploy:

```sh
kubectl delete deployment my-webapp
kubectl create deployment my-webapp --image=khushal2204/webapp-demo:01
kubectl delete service my-webapp
kubectl expose deployment my-webapp --type=LoadBalancer --port=5173
```

---

## 🧠 Learnings

- `kubectl create deployment` pulls images directly from Docker Hub.
- Minikube uses Docker container as a local VM. Tunnel must stay open for `LoadBalancer` services.
- React Vite apps expose on port `5173` by default, so be sure to use `--port=5173` when exposing.

---

## 📅 Date

**10th July 2025**

---

## ✍️ Author

Khushal Rathi  
[GitHub](https://github.com/khushalRathi-DEV)
