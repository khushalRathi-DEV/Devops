
# Multi-Container Kubernetes App with MongoDB and Node.js
Refer the multi_container_db-app_demo folder for the node js server code and the yml files(config-map,mongo-db and node-app).

This guide documents the steps taken to create a **multi-container app** in Kubernetes consisting of:
- A MongoDB database (`mongo-app` pod)
- A Node.js server (`node-app` pod)

We use **ConfigMaps** to manage environment variables and ensure connectivity between the services.

---

## 🛠️ Kubernetes Pods Status

```bash
kubectl get pods
```

**Output:**
```
NAME                         READY   STATUS              RESTARTS      AGE
mongo-app-7d5f46f47b-4h7cl   1/1     Running             1 (15m ago)   38h
node-app-68c7fccdf8-k8dds    0/1     ContainerCreating   0             3m
node-app-68c7fccdf8-lf2pv    0/1     ContainerCreating   0             3m
```

- `mongo-app` is running successfully.
- `node-app` pods are in **ContainerCreating** status, possibly pulling images or waiting for dependencies.

---

## 🌐 Kubernetes Services

```bash
kubectl get service
```

**Output:**
```
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes         ClusterIP      10.96.0.1        <none>        443/TCP          8d
service-mongodb    ClusterIP      10.107.41.226    <none>        27017/TCP        13m
service-node-app   LoadBalancer   10.108.176.241   <pending>     8080:30471/TCP   3m10s
```

### Key Points:
- `service-mongodb` is a ClusterIP service, accessible only inside the cluster.
- `service-node-app` is exposed as a **LoadBalancer** (used with Minikube to tunnel to localhost).

---

## 🌍 Accessing the Node.js App

We use `minikube service` to expose the Node.js app locally.

```bash
minikube service service-node-app
```

**Output:**
```
| NAMESPACE |       NAME       | TARGET PORT |            URL            |
|-----------|------------------|-------------|---------------------------|
| default   | service-node-app |        8080 | http://192.168.49.2:30471 |
```

Then it starts a tunnel:

```
🏃  Starting tunnel for service service-node-app.
|-----------|------------------|-------------|------------------------|
| NAMESPACE |       NAME       | TARGET PORT |          URL           |
|-----------|------------------|-------------|------------------------|
| default   | service-node-app |             | http://127.0.0.1:49889 |
```

Now the app is accessible at: `http://127.0.0.1:49889`

---

## 📦 Connecting Containers Using ConfigMap

We used a ConfigMap to inject the MongoDB connection URI into the Node.js container as an environment variable.

### Benefits:
- Decouples configuration from the code.
- Makes it easy to update values without modifying image.

```yaml
env:
  - name: MONGODB_URI
    valueFrom:
      configMapKeyRef:
        name: mongo-config
        key: uri
```

This is how the Node app connects to MongoDB securely and dynamically.

---

## ✅ What I Learned

- How to run a multi-container Kubernetes app using **Pods**, **Services**, and **ConfigMaps**.
- How to expose internal apps to outside world using `minikube service` + tunneling.
- The difference between **ClusterIP** and **LoadBalancer** service types.
- Managing environment variables effectively in Kubernetes.

