### Let's create network and set it to default

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl config set-context --current --namespace=todoapp
```

### Create pods and services:

```bash
kubectl apply -f .infrastructure/busybox.yml
kubectl apply -f .infrastructure/todoapp-pod.yml

kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
```

### Check that all works
```bash
kubectl get pods
kubectl get svc
```

### Test clusterIP service:
```bash
kubectl exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local
exit

kubectl port-forward service/todoapp-service 8070:80
go to browser:
localhost:8070
```

### Test nodeport service:
```bash
kubectl exec -it busybox -- sh
curl http://todoapp-nodeport-service.todoapp.svc.cluster.local
exit

kubectl port-forward service/todoapp-nodeport-service 8070:80
go to browser:
localhost:8070
```