
# Persistent Volume & Persistent Volume Claim in Kubernetes

In this section, I learned how to use **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)** in a multi-container application with MongoDB and Node.js. The primary goal was to ensure that MongoDB's data remains **persistent even after pod restarts**.

Refer the multi_container_app_persistentvolume file 
---

## 💡 Concepts

### 1. Persistent Volume (PV)
A PV is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using StorageClasses.

### 2. Persistent Volume Claim (PVC)
A PVC is a request for storage by a user. It specifies size, access modes, etc. Once bound to a PV, it allows pods to mount the storage.

### 3. Why use PV/PVC?
Without persistent volumes, any data stored in a pod is lost if the pod dies or restarts. Using PV/PVC ensures data is **stored outside of the pod lifecycle**.

---

## 📁 Step-by-step

### ✅ Create Persistent Volume

```bash
kubectl apply -f host-pv.yml
```

**Output:**
```
persistentvolume/host-pv created
```

```bash
kubectl get pv
```

**Output:**
```
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   AGE
host-pv   1Gi        RWO            Retain           Available           standard        13s
```

---

### ✅ Create Persistent Volume Claim

```bash
kubectl apply -f host-pvc.yml
```

**Output (initial error fixed by correcting YAML field):**
```
persistentvolumeclaim/host-pvc created
```

```bash
kubectl get pvc
```

**Output:**
```
NAME       STATUS   VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
host-pvc   Bound    host-pv   1Gi        RWO            standard        3s
```

---

### ✅ Apply MongoDB Deployment

```bash
kubectl apply -f mongo-db.yml
```

Initially, there was an error due to `claimName` field being at the wrong level. After correcting it:

**Output:**
```
deployment.apps/mongo-app created
service/service-mongodb unchanged
```

---

### ✅ Apply Node.js Deployment

```bash
kubectl apply -f node-app.yml
```

**Output:**
```
deployment.apps/node-app created
service/service-node-app created
```

---

### 🔁 Restart Test

Deleted MongoDB deployment and reapplied to confirm persistence.

```bash
kubectl delete deployment mongo-app
kubectl apply -f mongo-db.yml
```

**Output:**
```
deployment.apps/mongo-app created
service/service-mongodb unchanged
```

Data stored earlier remained **intact** thanks to the persistent volume!

---

## ✅ Conclusion

I successfully set up a persistent storage mechanism for MongoDB using PV and PVC. This ensures data is retained across pod or cluster restarts.

---

