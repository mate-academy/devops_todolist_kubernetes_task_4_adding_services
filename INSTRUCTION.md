Preporation:
run the container with busybox to execute it later:
```
kubectl run busybox --image=busybox -n todoapp --restart=Never -- sleep 3600
```

Create namespace, pods and services:

```
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml
kubectl apply -f .infrastructure/clusterIP.yml
kubectl apply -f .infrastructure/nodeport.yml
```

To test ClusterApi from busybox run:
```
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local

```

To test port forward on 8081 run:
```
kubectl port-forward svc/todoapp-service 8081:80 -n todoapp
```

To test NodePort Service:

Go to localhost:30007 #30007 is the NodePort in NodePort manifest