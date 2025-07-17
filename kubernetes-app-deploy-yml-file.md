# Deploying Node.js App to Kubernetes using YAML

This guide documents the process of deploying a React.js app to a Kubernetes cluster using YAML configuration files. It includes the application of deployment and service files, service exposure via Minikube, and verification through command outputs.
Refer the YAML_CONFIG folder for YML files
---

## Step 1: Apply the Deployment YAML

```bash
kubectl apply -f deployment-node-app.yml
```

**Output:**
```
deployment.apps/my-node-app unchanged
```

This applies the `deployment-node-app.yml` file, which defines the desired state for the Node.js app deployment.

---

## Step 2: Apply the Service YAML

```bash
kubectl apply -f service-node-app.yml
```

**Output:**
```
service/service-my-node-app created
```

This creates a service to expose the Node.js app inside the cluster. The service is of type `LoadBalancer` for external access.

---

## Step 3: Verify Services

```bash
kubectl get services
```

**Output:**
```
NAME                  TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes            ClusterIP      10.96.0.1       <none>        443/TCP          6d18h
my-webapp             LoadBalancer   10.99.172.218   <pending>     5173:30710/TCP   5d2h
service-my-node-app   LoadBalancer   10.103.97.240   <pending>     5173:31969/TCP   14s
```

We can see our service `service-my-node-app` listed here. The `EXTERNAL-IP` is `<pending>` because we're using Minikube, which doesn't support external load balancers natively.

---

## Step 4: Expose the Service using Minikube

```bash
minikube service service-my-node-app
```

**Output:**
```
|-----------|---------------------|-------------|---------------------------|
| NAMESPACE |        NAME         | TARGET PORT |            URL            |
|-----------|---------------------|-------------|---------------------------|
| default   | service-my-node-app | http/5173   | http://192.168.49.2:31969 |
|-----------|---------------------|-------------|---------------------------|
🎉  Opening service default/service-my-node-app in default browser...
```

This command starts a Minikube tunnel and opens your service in the browser using the local IP (e.g., `http://192.168.49.2:31969`).

---

## Summary

✅ Deployment and service YAML files help automate the configuration of Kubernetes objects.  
✅ `kubectl apply` is used to create/update resources from these files.  
✅ Minikube provides a way to expose LoadBalancer services using its `service` command.