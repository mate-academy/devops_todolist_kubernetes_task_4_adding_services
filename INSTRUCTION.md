## Testing ClusterIP with busybox:

```bash
kubectl exec -it busybox -n todoapp -- curl http://todoapp-clusterip.todoapp.svc.cluster.local
```

## Testing via port-forward:

```bash
kubectl port-forward svc/todoapp-clusterip 8080:80 -n todoapp
```

Open http://localhost:8080

## Access via NodePort:

1. Find the node IP:

```bash
kubectl get nodes -o wide
```

2. Open in browser: `http://<NodeIP>:30007`
