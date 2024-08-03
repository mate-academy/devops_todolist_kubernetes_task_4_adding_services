# Apply namespace 
kubectl apply -f .\.infrastructure\namespace.yml

# Apply pods 
kubectl apply -f .\.infrastructure\todoapp-pod.yml

# Apply busybox 
kubectl apply -f .\.infrastructure\busybox.yml

# Apply ClusterIP
kubectl apply -f .\.infrastructure\clusterip.yml

# Apply NodePort
kubectl apply -f .\.infrastructure\nodeport.yml

# Enter busybox container and test connection
kubectl -n todoapp exec -it busybox -- sh
curl http://todoapp-clusterip.todoapp.svc.cluster.local

# Test ToDo application using the service port-forward command
kubectl port-forward service/todoapp-clusterip 8081:80

# Access an application using NodePort Service
http://localhost:30011
