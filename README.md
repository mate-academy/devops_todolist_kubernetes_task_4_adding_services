# Django ToDo list

This is a todo list web application with basic features of most web apps, i.e., accounts/login, API, and interactive UI

![ToDo logo](https://i.ibb.co/YDdCcZR/2.png)

## Setup

1. You need apply all manifests
```
kubectl apply -f .infrastructure
```

2. Test the application using a ClusterIP service DNS from a busybox container
```
kubectl exec -n todoapp -it busybox -- sh
curl http://todoapp-cluster-ip-service.todoapp.svc.cluster.local:8888
```

3. Test the application using a ClusterIP service DNS with port-forward
```
kubectl port-forward service/todoapp-cluster-ip-service 7777:8888 -n todoapp
```

4. To test the application using a NodePort service open in browser the next link
```
http://localhost:30001/
```
