# Pre-configuration

### Set `todoapp` namespace
```
kubectl apply -f .\.infrastructure\namespace.yml
kubectl config set-context --current --namespace=todoapp
```

### Run pods
```
kubectl apply -f .\.infrastructure\busybox.yml
kubectl apply -f .\.infrastructure\clusterIP.yml
kubectl apply -f .\.infrastructure\nodeport.yml
kubectl apply -f .\.infrastructure\todoapp-pod.yml
```

# Test `todoapp` using ClusterIP

```
kubectl exec -it busybox -- sh
curl http://todoapp-clusterip.todoapp.svc.cluster.local:80
```

# Test `todoapp` using port forward

```
kubectl port-forward service/todoapp-clusterip 8080:80
curl http://localhost:8080
```

#  Test `todoapp` using port NodePort

### Get cluster node’s IP
```
kubectl get nodes -o wide
```

### curl inside cluster
```
curl http://<NODE_INTERNAL_IP>:30008
```

### curl using external IP outside cluster
```
curl http://localhost:30008
```
