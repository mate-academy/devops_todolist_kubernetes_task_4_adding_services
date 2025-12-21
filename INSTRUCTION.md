<!-- to run app -->

kubectl apply -f todoapp-pod.yml
kubectl apply -f busybox.yml
kubectl apply -f todolist-cluster.yml
kubectl apply -f nodeport-service.yml

<!-- to test access via ClusterIP service DNS via busybox -->
kubectl exec -it busybox -- sh         
curl http://todolist-cluster.todoapp.svc.cluster.local

<!--  another way test ToDo application via browser http://localhost:8080/-->

kubectl port-forward service/todolist-cluster 8080:80

<!-- to test access via NodePort service via browser-->

http://localhost:30007/