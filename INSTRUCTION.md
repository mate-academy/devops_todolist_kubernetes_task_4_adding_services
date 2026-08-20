# Deploying an application in a Kubernetes cluster.
## Works with Docker Desktop
### Installation instructions:

1. Create a namespace.
```
kubectl apply -f namespace.yml
```
*For convenience, you could say to use the default namespace.*
```
kubectl config set-context --current --namespace=todoapp
```

2. Launch Busybox
```
kubectl apply -f busybox.yml
```

3. Launch todo.
```
kubectl apply -f todoapp-pod.yml
```
4. Launch the service ClusterIP.
```
kubectl apply -f clusterIp.yml
```
5. Launch the service NodePort
```
kubectl apply -f nodeport.yml
```

To test the programme by calling the ClusterIP DNS service from the busybox container, use the command:
```
kubectl -n todoapp exec -it busybox -- sh curl http://todoapp-service.todoapp.svc.cluster.local
```

To test the ToDo programme using the `port-forward` command in `service`, use the following command:
```
kubectl port-forward pod/todoapp 8080:8080
```

The application should be accessible via the NodePort service at the following address:

http://localhost:30008