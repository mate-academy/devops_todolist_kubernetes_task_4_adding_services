# INSTRUCTION.md

## 📦 Project Overview

This project is a **Django-based ToDo List application**, containerized and deployed to Kubernetes. It includes:

- Two app pods (`todoapp`, `todoapp-1`) running the ToDo app.
- ClusterIP and NodePort services for internal and external access.
- A `busybox` pod to test service discovery within the cluster.

---

## 🚀 Getting Started

> ⚠️ Make sure your Kubernetes cluster is up and running (e.g., Minikube, Kind, or a real K8s cluster).

### 1. Apply Kubernetes Manifests

```bash
kubectl apply -f namespace.yaml
kubectl apply -f todoapp-pod.yaml       # defines both todoapp and todoapp-1
kubectl apply -f clusterip-service.yaml
kubectl apply -f nodeport-service.yaml
kubectl apply -f busybox.yaml
```

---

## 🔍 How to Test

### ✅ Test with ClusterIP Service from BusyBox Pod

1. Enter the `busybox` pod:

```bash
kubectl exec -it -n todoapp busybox -- sh
```

2. Inside the pod, run the following curl command:

```sh
curl todoapp-cluster-ip-service.todoapp.svc.cluster.local
```

> You should see the ToDo app landing page or its HTML response.

---

### 🔄 Test with `kubectl port-forward`

You can also forward the ClusterIP service port to your local machine:

```bash
kubectl port-forward -n todoapp service/todoapp-cluster-ip-service 8080:80
```

Then, open your browser and navigate to:

```
http://localhost:8080
```

> The ToDo app UI should be accessible.

---

### 🌐 Access via NodePort Service

To access the app externally:

1. Get your node’s IP. If you're using Minikube:

```bash
minikube ip
```

2. Access the app using the NodePort:

```
http://<node-ip>:30007
```

> Example:
```
http://192.168.49.2:30007
```

---
