
# 🔄 Updating a React App in Kubernetes with Zero Downtime

This document explains how I updated a Dockerized React application running in a Kubernetes cluster using `kubectl set image`. Kubernetes handled the update with **zero downtime** by managing rolling updates internally.

---

## 🔁 What Changed?

- **Old Image**: `khushal2204/webapp-demo:01`
- **New Image**: `khushal2204/webapp-demo:02`
- The React app was updated locally, rebuilt, pushed to Docker Hub, and the deployment was updated via `kubectl`.

---

## 🧱 Steps Performed

### 1. 🔨 Rebuild Docker Image

```sh
docker build -t khushal2204/webapp-demo:02 .
```

**Output:**
```
 => => naming to docker.io/khushal2204/webapp-demo:02
 => => unpacking to docker.io/khushal2204/webapp-demo:02
```

### 2. 🚀 Push to Docker Hub

```sh
docker push khushal2204/webapp-demo:02
```

**Output:**
```
digest: sha256:1205a0e2c90ed9d10cfc34a29b0bc0502953d44ba0b62d5b3c7b55f32b0b4e3e
```

---

### 3. ✅ Check Existing Deployment

```sh
kubectl get deployments
kubectl get pods
```

**Before Update:**
```
NAME                         READY   STATUS    RESTARTS   AGE
my-webapp-5c68c646f7-5km4v   1/1     Running   0          31m
```

> The old pod is still running from image `:01`.

---

### 4. 🔁 Update the Deployment Image

```sh
kubectl set image deployment my-webapp webapp-demo=khushal2204/webapp-demo:02
```

**Output:**
```
deployment.apps/my-webapp image updated
```

> This triggers a **rolling update**. Kubernetes creates a new replica with the new image and ensures the new pod is running before terminating the old one.

---

### 5. 📦 Check Pod Rollout

```sh
kubectl get pods
```

**During Rollout:**
```
NAME                         READY   STATUS              RESTARTS   AGE
my-webapp-57bcbb845d-l6ltx   0/1     ContainerCreating   0          10s
my-webapp-5c68c646f7-5km4v   1/1     Running             0          31m
```

**After Rollout:**
```
NAME                         READY   STATUS    RESTARTS   AGE
my-webapp-57bcbb845d-l6ltx   1/1     Running   0          49s
```

> Kubernetes automatically terminated the older pod after the new pod was `READY`. **No downtime!**

---

## 🧠 Key Learnings

- `kubectl set image` is the go-to command for updating container images in a live Kubernetes deployment.
- Kubernetes ensures **zero downtime** with rolling updates.
- Old pods are kept alive until new pods are up and healthy.
- You can watch rollout status using:
  ```sh
  kubectl rollout status deployment my-webapp
  ```

---

## ✅ Final Check

```sh
kubectl get services
minikube service my-webapp
```

> The app continues to be served on the same URL, now with the updated code.

---

## 📅 Date

**10th July 2025**

---

## ✍️ Author

Khushal Rathi  
[GitHub](https://github.com/khushalRathi-DEV)
