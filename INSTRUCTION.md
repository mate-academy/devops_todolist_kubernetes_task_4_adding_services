# ToDo App - Kubernetes Deployment Instructions

## Deploy the application

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todoapp-pod-2.yml
kubectl apply -f .infrastructure/todoapp-clusterip.yml
kubectl apply -f .infrastructure/todoapp-nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

## 1. Test via ClusterIP service from a busybox container

```bash
# Wait for pods to be ready
kubectl wait --for=condition=ready pod -l app=todolist -n todoapp --timeout=120s

# Execute a curl request from the busybox pod to the ClusterIP service
kubectl exec -n todoapp busybox -- curl http://todoapp-clusterip/api/health
```

Expected output: `Health OK`

## 2. Test via port-forward

```bash
# Forward local port 8080 to the ClusterIP service
kubectl port-forward -n todoapp svc/todoapp-clusterip 8080:80

# In another terminal, access the app
curl http://localhost:8080/api/health
```

Expected output: `Health OK`

To access the web UI:
```bash
open http://localhost:8080
```

## 3. Access via NodePort service

```bash
# Get the NodePort (default is 30080)
kubectl get svc -n todoapp todoapp-nodeport

# Access the app using the node's IP and the NodePort
curl http://<NODE_IP>:30080/api/health
```

Expected output: `Health OK`

To access the web UI:
```bash
open http://<NODE_IP>:30080
```

> **Note:** On Minikube, use `minikube ip` to get the node IP.
> On kind, use `kind get clusters` and `kubectl cluster-info` to find the address.

## Cleanup

```bash
kubectl delete -f .infrastructure/
```
