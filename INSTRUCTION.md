# Django Todo llist testing instructions
## 1. Applying all the manifests:
```bash
cd .infrastructure
```
```bash
kubectl apply -f namespace.yml
```
```bash
kubectl apply -f busybox.yml
```
```bash
kubectl apply -f todoapp-pod.yml
```
```bash
kubectl apply -f clusterip.yml
```
```bash
kubectl apply -f nodePort.yml
```
## 2. Testing the application
### Calling ClasterIp service from busybox container
Connecting to the pod
```bash
kubectl exec -it -n todoapp busybox -- sh
``` 
Use this command inside shell
```bash
curl http://todoapp-service.todoapp.svc.cluster.local
```
### Testing application using port-forward
```bash
kubectl port-forward svc/todoapp-service 8081:80
```
Follow the link to check 
```
http://localhost:8081
```
### Testing NodePort 
Open the link in browser to check
```
http://localhost:30003
```