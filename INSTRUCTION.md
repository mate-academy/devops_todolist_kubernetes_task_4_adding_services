# ToDo App — Kubernetes Testing Instructions

## Set Default Namespace for the Context

Set `todoapp` as the default namespace for the current context (`docker-desktop`) so you don't need to pass `-n todoapp` with every command:

```bash
kubectl config set-context docker-desktop --namespace=todoapp
```

Verify it was applied:

```bash
kubectl config get-contexts
# NAMESPACE column should show "todoapp" for docker-desktop
```

Now commands like `kubectl get pods` work without `-n todoapp`.

To switch back to the `default` namespace:

```bash
kubectl config set-context docker-desktop --namespace=default
```

---

## Apply All Manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIP.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/busybox.yml
```

Verify everything is running:

```bash
kubectl get pods
kubectl get services
```

---

## 1. Test via ClusterIP DNS from the BusyBox Container

The ClusterIP service is only reachable inside the cluster. Use the busybox pod (already in the same `todoapp` namespace) to call it by DNS name.

```bash
# Open a shell inside the busybox pod
kubectl exec -it busybox -- sh
```

Inside the shell:

```sh
# Short DNS form (works within the same namespace)
curl http://todoapp-service/api/health

# Fully qualified DNS form
curl http://todoapp-service.todoapp.svc.cluster.local/api/health
```

Kubernetes will automatically load-balance requests between `todoapp` and `todoapp-1` pods.

---

## 2. Test via `port-forward`

Forward a local port to the ClusterIP service (no node exposure needed):

```bash
kubectl port-forward service/todoapp-service 8080:80
```

In a **second terminal**, run:

```bash
# Health check
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

The NodePort service exposes the app on port **30007** of every cluster node.

On **Docker Desktop**, NodePort is accessible directly on `localhost`:

```bash
curl http://localhost:30007/api/health
curl http://localhost:30007/api/todos
```

Or open `http://localhost:30007` in your browser.

On a **real / cloud cluster**, get the node IP first:

```bash
kubectl get nodes -o wide
# Use the INTERNAL-IP or EXTERNAL-IP from the output
curl http://<NODE_IP>:30007/api/todos
```

> On cloud clusters (EKS / GKE / AKS) make sure port `30007` is allowed in the node security group / firewall.

---

## Cleanup

```bash
kubectl delete -f .infrastructure/busybox.yml
kubectl delete -f .infrastructure/nodeport.yml
kubectl delete -f .infrastructure/clusterIP.yml
kubectl delete -f .infrastructure/todoapp-pod.yml
kubectl delete -f .infrastructure/namespace.yml
```