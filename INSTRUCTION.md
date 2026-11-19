CHECK APP:

APPLY MANIFEST

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/
```

## 1. ClusterIP from busybox

The `busybox` pod is created from `.infrastructure/busybox.yml`.

```
kubectl exec -it busybox -n todoapp -- wget -qO- http://todoapp-clusterip.todoapp.svc.cluster.local/api/ready
```

## 2. port-forward

```
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
```

other terminal: `curl http://localhost:8080/api/ready` or open `http://localhost:8080` in browser.

## 3. NodePort

The app is available on port 30007 of the node: `http://localhost:30007` (for a local cluster).
For a remote cluster, use the node IP instead of `localhost`: `kubectl get nodes -o wide`.