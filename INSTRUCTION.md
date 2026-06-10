Clone repository
git clone repository
go to folder repositiry
create namespace
kubectl apply -f namespace.yml
check namespases
kubectl get ns
create app
kubectl apply -f todoapp-pod.yml
create busybox
kubectl apply -f busybox.yml
create service clusterip
kubectl apply -f clusterIp.yml
create node port
kubectl apply -f nodeport.yml
test clusterip
kubectl port-forward service/todoapp-service 8080:80 -n todoapp
go to page http://localhost:8080
or
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-service.todoapp.svc.cluster.local
test nodeport
go to page http://localhost:30007