# Apply all manifests
kubectl apply -f namespace.yml
kubectl apply -f todoapp-pod.yml
kubectl apply -f busybox.yml
kubectl apply -f clusterip.yml
kubectl apply -f nodeport.yml


# Test ToDo application using the service port-forward command
kubectl port-forward service/todoapp-service 8081:80


# Test ClusterIP DNS via busybox
kubectl -n todoapp exec -it busybox -- sh

curl http://todoapp-service.todoapp.svc.cluster.local

exit

# Access an app using a NodePort Service via browser
localhost:30007
