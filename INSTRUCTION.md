### Instruction file

## installation
# create a namespace
```bash
kubectl apply -f .infrastucture/namespaces.yml
```
# set the todoapp namespace as default
```bash
kubectl config set-context --current --namespace=todoapp
```
# create a pods
```bash
kubectl apply -f .infrastucture/todoapp-pod.yml
kubectl apply -f .infrastucture/busybox.yml
```
# create services
```bash
kubectl apply -f .infrastucture/clusterIp.yml
kubectl apply -f .infrastucture/nodeport.yml
```
# test the app by calling a ClusterIP service DNS from a busybox container
```bash
kubectl exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local
exit
```
# test ToDo application using the service `port-forward` command
```bash
kubectl port-forward service/todoapp-service 8081:80
exit
```
# check logs of the nods
```bash
kubectl logs todoapp
kubectl logs todoapp-1
```

# access an app using a NodePort Service
```bash
kubectl get services
curl http://localhost:30007
```
