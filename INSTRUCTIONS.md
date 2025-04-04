# Django Todo llist testing instructions
## 1. Applying all the manifests:
```bash
kubectl apply -f .infrastructure
```

## 2. Testing the application
### Calling ClasterIp service from busybox container
Connecting to the pod
```bash
kubectl exec -it -n todoapp busybox -- sh
``` 
Use this command inside shell
```bash
curl http://todolist-service.todoapp.svc.cluster.local
```
### Testing application using port-forward
```bash
kubectl port-forward svc/todolist-service 8081:80
```
Follow the link to check 
```
http://localhost:8081
```
### Testing NodePort 
Open the link in browser to check
```
http://localhost:30007
```