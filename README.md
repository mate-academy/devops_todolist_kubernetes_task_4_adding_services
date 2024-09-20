## How to test

### Apply manifests

kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
```

### Apply services

kubectl apply -f .infrastructure/clisterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

### Enter busybox container

kubectl -n todoapp exec -it busybox -- sh
# make a request to the ClusterIP service
curl http://todoapp-cluster.todoapp.svc.cluster.local
```

### Port-forward

kubectl -n todoapp port-forward service/todoapp-cluster 8081:80
```

### NodePort

Link to the app: http://localhost:30007