Apply this commands to run app in k8s
```bash
cd .infrastructure
kubectl apply -f namespace.yml
kubectl config set-context --current --namespace=todoapp
kubectl apply -f todoapp-pod.yml
kubectl apply -f ClusterIP.yml
kubectl apply -f nodeport.yml
```

1. To test an app by calling a ClusterIP service DNS from a busybox container
```bash
kubectl exec -it busybox -- sh
```
Inside shell make request through curl
```
curl http://todoapp-service.todoapp.svc.cluster.local
```
2. to test ToDo application using the service `port-forward` command
```bash
kubectl port-forward service/todoapp-service 8081:80
```
Open browser on `localhost:8081`
3. to access an app using a NodePort Service Open browser on `localhost:30007`