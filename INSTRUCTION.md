# ToDo App Deployment and Testing Instructions

This repository contains Kubernetes manifests to deploy a ToDo application as two separate pods and expose them via both ClusterIP and NodePort services. It also includes readiness and liveness probes, environment variables, and service testing instructions.

## 📁 Prerequisites

- A Kubernetes cluster (e.g., local with minikube, or any remote cluster)
- kubectl configured and connected to your cluster
- Docker installed and logged in to Docker Hub
- Python 3.8+ installed (for local testing)

## 🚀 Deployment

Apply all the manifests in the correct order to create the namespace, deploy both pods, and expose them via ClusterIP and NodePort services:

```sh
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f clusterip.yml
kubectl apply -f nodeport.yml
```

## 🧪 Testing

### 1. 🔄 Call ClusterIP Service from BusyBox

To test the ClusterIP service internally within the cluster:

1. Deploy a BusyBox pod:

```sh
kubectl apply -f busybox.yml
```

2. Enter the BusyBox pod:

```sh
kubectl -n todoapp exec -it busybox -- sh
```

3. Make a request to the ClusterIP Service

```sh
curl http:/todoapp-service.todoapp.svc.cluster.local
```

You should see the HTML or API response from one of the ToDo app pods.

### 2. 📦 Port-Forward the Service for Local Access

To test the ToDo application using port-forward:

```sh
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

Then visit http://localhost:8080 in your browser.

### 3. 🌐 Access the App Using NodePort

To access the app via the NodePort service, visit http://localhost:30007 in your browser.


