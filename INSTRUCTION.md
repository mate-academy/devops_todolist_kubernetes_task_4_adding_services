1) Apply k8s containers:
```shell
kubectl apply -f ./.infrascture/
```
2) Check cluster service via busybox:
```shell
kubectl exec -it busybox -n todoapp
curl http://todoapp-service.todoapp.svc.cluster.local
```
3) Check cluster service via port-forward:
```shell
kubectl port-forward service/todoapp-service 8001:80 -n todoapp
```
4) Access the app via NodePort service:
```shell
http://localhost:30080
```