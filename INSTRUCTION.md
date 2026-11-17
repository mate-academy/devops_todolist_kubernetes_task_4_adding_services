### To run the app
```bash
kubectl apply -f .infrastructure/
```

### To test an app by calling a ClusterIP service DNS from a busybox container
To connect to busybox container
```bash
kubectl -n todoapp exec -it busybox -- sh
```
Run curl command:
```bash
curl http://todoapp-service.todoapp.svc.cluster.local
```
> Where: `curl http://<clusterIp-service-name>.<namespace>.svc.cluster.local`


### To test ToDo application using the service port-forward command
```bash
kubectl port-forward service/todoapp-service 8081:80 -n todoapp
```
> Where: `kubectl port-forward service/<clusterIp-service-name> 8081:80`

### To access an app using a NodePort Service
Open in browser: [localhost:30007](http://localhost:30007)
