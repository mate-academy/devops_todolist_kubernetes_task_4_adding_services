# ToDo App — Kubernetes Testing Instructions

## Apply All Manifests

```bash
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f todoapp-pod2.yml
kubectl apply -f clusterip-service.yml
kubectl apply -f nodeport-service.yml
kubectl apply -f busybox.yml
```

Verify everything is running:

```bash
kubectl get pods -n todoapp
kubectl get services -n todoapp
```

---

## 1. Test via ClusterIP DNS from the BusyBox Container

The ClusterIP service is only reachable inside the cluster. Use the busybox pod (already in the same `todoapp` namespace) to call it by DNS name.

```bash
# Open a shell inside the busybox pod
kubectl exec -it busybox -n todoapp -- sh
```

Inside the shell:

```sh
# Short DNS form (works within the same namespace)
curl http://todoapp-clusterip/

# Fully qualified DNS form
curl http://todoapp-clusterip.todoapp.svc.cluster.local/

# Check the todos API
curl http://todoapp-clusterip.todoapp.svc.cluster.local/api/todos
```

Kubernetes will automatically load-balance requests between `todoapp` and `todoapp2` pods.

---

## 2. Test via `port-forward`

Forward a local port to the ClusterIP service (no node exposure needed):

```bash
kubectl port-forward service/todoapp-clusterip 8080:80 -n todoapp
```

In a **second terminal**, run:

```bash
# Homepage / health check
curl http://localhost:8080/api/health

# List all todos
curl http://localhost:8080/api/todos

# Create a new todo
curl -X POST http://localhost:8080/api/todos \
  -H "Content-Type: application/json" \
  -d '{"title": "Buy milk", "completed": false}'
```

Or open `http://localhost:8080` in your browser.

Press `Ctrl+C` to stop the tunnel.

---

## 3. Access the App via NodePort Service

The NodePort service exposes the app on port **30080** of every cluster node.

### Get the Node IP

```bash
# Real cluster
kubectl get nodes -o wide

# Minikube
minikube ip
```

### Send requests

```bash
# Replace <NODE_IP> with the IP from above
curl http://<NODE_IP>:30080/api/todos
```

Or open `http://<NODE_IP>:30080` in your browser.

### Minikube shortcut

```bash
minikube service todoapp-nodeport -n todoapp
```

> On cloud clusters (EKS / GKE / AKS) make sure port `30080` is allowed in the node security group / firewall.

---

## Cleanup

```bash
kubectl delete -f busybox.yml
kubectl delete -f nodeport-service.yml
kubectl delete -f clusterip-service.yml
kubectl delete -f todoapp-pod2.yml
kubectl delete -f todoapp-pod.yml
kubectl delete -f namespace.yml
```
