### Application of manifests
Firstly you should open terminal, change dir `cd .infrastructure` run these commands to apply manifests:
```
kubectl apply -f todoapp-pod.yml
```
```
kubectl apply -f clusterIp.yml
```
```
kubectl apply -f nodeport.yml
```


### Test an app by calling a ClusterIP svc (busybox)
```
kubectl -n todoapp exec -it busybox -- sh
```
```
curl http://todolist-service.todoapp.svc.cluster.local/api/ready
```

### Test an app by calling a ClusterIP svc (port-forward)
```
kubectl port-forward -n todoapp service/todolist-service 8081:80 
```
Then open browser and enter `localhost:8081/api/ready`

### Access an app using a NodePort svc
Open browser and enter `localhost:30008/api/ready`
