# Instructions for TodoApp Infrastructure
### 1. Deployment
To deploy all components, run the following commands from the root of the repository:

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/todo-cluster-service.yml
kubectl apply -f .infrastructure/todo-nodeport-service.yml
kubectl apply -f .infrastructure/busybox.yml
```

---
### 2. Testing via ClusterIP (Internal DNS)
The `todo-cluster-service` balances traffic between two pods using internal Kubernetes DNS. To test this from the `busybox` container:
```
kubectl exec -it busybox -n todoapp -- curl http://todo-cluster-service:8000/api/health/live/
```
---
### 3. Testing via Port-Forward
To access the application from your local machine's browser by forwarding traffic to the service:
```
kubectl port-forward svc/todo-cluster-service 8000:8000 -n todoapp
```

After running this, open your browser at: http://localhost:8000/

---
### 4. Testing via NodePort
NodePort exposes the application on a specific port of the cluster node.

**Step 1: Find the Cluster Node IP address**
```
kubectl get nodes -o wide
```
Take the address from the **INTERNAL-IP** column (e.g., `10.89.0.2`).

**Step 2: Access the application** Use that IP and the NodePort `30008`:
```
curl http://10.89.0.2:30008/api/health/live/
```
