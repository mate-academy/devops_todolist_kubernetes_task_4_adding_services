# Kubernetes Deployment Instructions for ToDo App

## 1. Apply all manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-pod-2.yml
kubectl apply -f .infrastructure/clusterip-service.yml
kubectl apply -f .infrastructure/nodeport-service.yml
kubectl apply -f .infrastructure/busybox.yml